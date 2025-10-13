---
title: Read Defeating Nondeterminism in LLM Inference
date: 2025-10-13
lang: en
duration: 8min
tags:
  - Deterministic
  - LLM
---

_Reference: [Defeating Nondeterminism in LLM Inference](https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/)_

[[toc]]

## Problems

1.  Some kernels on GPUs are **nondeterministic**.
2.  However, all the kernels used in a language model’s forward pass are **deterministic**.
3.  Moreover, the forward pass of an LLM inference server (like vLLM) can also be claimed to be **deterministic**.
4.  Nevertheless, from the perspective of anybody using the inference server, the results are **nondeterministic**.

## Root cause

### 普遍认为的观点

- **Floating-point non-associativity**

  > Floating-point arithmetic in GPUs exhibits non-associativity, meaning _(a+b)+c ≠ a+(b+c)_ due to finite precision and rounding errors. This property directly impacts the computation of attention scores and logits in the transformer architecture, where parallel operations across multiple threads can yield different results based on execution order.

  ![An illustration of floating-point addition precision, showing that adding 1230 and 23.4 results in 1253.4, but with 3-digit precision, it becomes 1250.](/images/Defeating_Nondeterminism/image.png)

  - **`atomicAdd` does not provide determinism**
    - The order in which the results accumulate is purely dependent on which core finishes first.
    - 原子加法是解决GPU并行计算中**多核心向同一地址写入冲突**的关键工具，但它以**牺牲操作顺序的确定性**为代价，这在处理浮点数累加时尤其重要。

- **Concurrent execution**

### 真实的原因 : **load (and thus batch-size) nondeterministically varies**

> [!TIP]
> 实现确定性的关键在于：**不是改变浮点数的性质，而是控制它的行为**。

## Performance issue

- `atomicAdd` is not needed for the following two reasons:

  - **批次维度的充足并行性:**
    **同时处理500个向量，而不是一个。在这种情况下，我们可以让每个GPU核心负责处理一个完整的向量。每个核心独立地完成其向量的归约，而无需与其他核心进行通信或共享内存地址。这完全消除了对原子加法的需求。**
  - **实现确定性的替代策略: 即使在批次并行性不足的情况下，神经网络库也发展出了多种策略来在不牺牲性能的同时实现确定性。这些方法可以替代原子加法**
    - **分治归约（Split or Tree Reduction）**:
      - **方法**：将一个大的归约任务（例如，对100个元素求和）分解成多个较小的、独立的子任务（例如，五个核心分别对20个元素求和）。
      - **确定性**：每个子任务都是独立的，不会发生竞态条件。最终，只需对少数几个子结果进行非并行的“清理”归约，或者使用同步机制（如信号量）来保证最终的累加顺序是确定的。
    - **信号量（Semaphores）**:
      - **方法**：在需要多个线程块累加到同一地址时，使用信号量来控制它们的访问顺序。
      - **确定性**：信号量确保了累加操作将按照一个预定的、确定的顺序进行，从而避免了由于浮点数非结合性引起的微小差异。

- How to balance **performance** and **determinism**
  - **小批次下的性能问题**
    - **核心闲置**：当批次大小减小时，最终会出现**核心数量多于批次元素数量**的情况。这导致一些GPU核心闲置，降低了硬件的利用率和整体性能。
    - **解决办法**：一个优秀的内核工程师会采取之前提到的方案（如原子加法或分治归约）来解决这个问题，以确保即使在小批次下也能保持高并行度和性能。
  - **矛盾：性能 vs. 确定性**
    - **改变归约策略**：为了在小批次下保持性能，我们必须改变归约策略。例如，从“每个核心处理一个完整的批次元素”的策略，转变为“多个核心共同处理一个批次元素”的分治归约。
    - **破坏批次不变性**：正是这种策略的改变，**破坏了批次不变性**。因为在不同的批次大小下，计算路径和归约顺序是不同的，这导致了结果的微小差异。

## Forward pass

**在训练和推理中的不同**

- **训练（Training）**: The forward pass is the first step of the training process. After getting the output, the neural network compares it with the ground truth, calculates the **loss**, and then updates the model's weights through **backpropagation** to reduce the loss.
- **推理（Inference）**: After the model is trained, the forward pass is the **only** computational process. At this point, the model is fixed, and we only care about using it to make predictions or generate results for new data. Therefore, “forward pass” is equivalent to “inference” here.

> [!NOTE]
> The root of nondeterminism is not in the model's internal calculations, but in the **external environment** and **parallel processing**.

- **前向传播的确定性**:
  > “the forward pass itself being “deterministic” is not sufficient to ensure that a system that includes it is deterministic.”
  > This acknowledges that the forward pass itself (a single computation from input to output) is deterministic. That is, if you give the model a fixed input, it will produce the exact same output every time.
- **系统级别的非确定性**:
  > “what if our request’s output depended on the parallel user requests... from their perspective our overall LLM inference is also nondeterministic!”
  > True nondeterminism comes from the **dynamic batching** system. In a production environment, to improve efficiency, inference servers usually bundle multiple user requests into a large batch for parallel processing. The requests in this batch are random and will be different each time it runs.

## Batch invariance

- A property where the output of a request **does not depend on its batch size**.
- In LLM inference services, to improve efficiency, the server dynamically batches multiple user requests for parallel processing.
  - **理想情况（Batch Invariance）**: If a model's inference process is **batch-invariant**, then whether your request is processed alone (batch size of 1) or with 99 other requests (batch size of 100), your output will be **exactly the same**.
  - **实际情况（缺乏 Batch Invariance）**: As the original text states, the LLM's forward pass may lack batch invariance. This means that **changes in batch size can lead to slight differences in the output**.

## How to make **kernels batch-invariant**

- We must make every kernel batch-invariant.
- We only need to worry about the 3 operations that involve reductions:
  - **RMSNorm**
    ```python
    # x: [batch_size, hidden_dim]
    # weight: [hidden_dim]
    def rms_norm(x, weight):
        return x * torch.rsqrt(torch.mean(x ** 2, dim=-1, keepdim=True)) * weight
    ```
  - **Matrix multiplication**: [Split-K Matmul](https://github.com/NVIDIA/cutlass/blob/main/media/docs/cpp/efficient_gemm.md#parallelized-reductions).
    - Non-optimized versions can lose about 20% performance.
  - **Attention**.
- NVLink-Sharp in-switch reductions are deterministic on Blackwell as well as Hopper with CUDA 12.8+.
