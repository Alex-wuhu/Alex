---
title: GPU TEE and Confidential Computing
date: 2025-10-10
duration: 8min
tags:
  - GPU
  - TEE
---

An overview of how Trusted Execution Environments (TEE) and Confidential Computing work, with a focus on their application in GPUs.

[[toc]]

## TEE 保护了什么 (What TEE Protects)

1.  **Data in Motion (运动中的数据)**: 从一个位置传输到另一个位置的数据.
    - SSL/TLS（安全套接层/传输层安全）是一种安全协议，它可以在数据通过网络（如互联网）传输时对其进行加密。
2.  **Data at Rest (静态数据)**: 存储在存储设备上的数据.
    - 通过加密已经解决 (This is already solved by encryption).
3.  **Data-in-use (使用中的数据)**: CC 保护 (Protected by Confidential Computing).
    - 计算机内存中创建了一个安全的隔离环境（称为**可信执行环境**，即 **TEE**).

## What is a Trustworthy System?

A trustworthy system is made up of **hardware and software that can be verified** as trustworthy using mechanisms to verify the authenticity of technologies that make up the system.

![Base layers of a trustworthy system](/images/GPU_TEE/trustworthy-system-layers.png)

## Confidential Computing

Confidential Computing requires specific CPU hardware SKUs to be enabled.

### GPU 核心挑战 (Core Challenges for GPU)

**CPU 已经通过 MMU 建立了基本的内存隔离机制，但这种机制没有考虑数据的加密。** 因此，需要在不破坏现有安全框架的前提下，找到一种方法，让 GPU 能够安全、高效地访问和处理那些被 CPU 加密的内存数据。这通常需要 GPU 硬件本身具备解密能力，或者通过一个安全的通道与 CPU 协同完成解密工作。

#### 解决方案: Bounce Buffer

![Bounce Buffer Diagram](/images/GPU_TEE/bounce-buffer-diagram.png)

- **非安全页面的作用 (Role of Non-Secure Pages)**

  - CPU 供应商提供了在“机密模式”（即 CPU 正在处理机密数据）下分配这些页面的能力。这些页面对系统中的所有组件（包括 GPU）都是可见的，因此被称为“非安全”。
  - 尽管这些页面是可访问的，但只要存放在其中的**数据是加密的**，其机- **GPU 的数据传输 (Data Transfer for GPU)**:
  - GPU 可以访问这些非安全页面（也就是**弹跳缓冲区**, bounce buffer).
  - GPU 将加密的数据从这个缓冲区**复制**到它自己的内部存储器中。

- **VM、GPU、数据的处理流程 (Processing flow for VM, GPU, and Data)**:
  1.  VM 负责**加密**数据。
  2.  VM 将加密后的数据发送到这个非安全页面，即**弹跳缓冲区**。
  3.  GPU 从弹跳缓冲区将加密数据**复制**到其内部内存。
  4.  在内部，GPU 利用其硬件能力**解密**数据。
  5.  解密后，GPU 对数据进行**处理**（例如，进行AI计算、渲染等）。
  6.  处理完成后，GPU 再次对数据**加密**。
  7.  GPU 将加密后的数据放回弹跳缓冲区。
  8.  最后，GPU 会通知 VM，数据已经准备好，可以被取回和使用。

### Secure and Trusted Boot

Through the previous processing, Confidential Computing is ready. However, many activities begin long before the Operating System is ready to provision VMs with GPUs attached.

安全启动是一种**安全机制**，旨在确保计算机在启动过程中只加载和运行**可信的、经过签名的软件**。它的主要目的是防止恶意软件（如 Rootkit 或 Bootkit）在操作系统启动之前，通过篡改启动过程来获得控制权。

### Attestation

Attestation of running hardware is paramount to the adage “trust but verify”.

The Host should run attestation on **all available hardware before launching** their hypervisor and creating VMs for their final Confidential workloads. The Host, after doing their own attestation checks, will pass appropriate hardware to their hypervisor and will provision a CVM.

## Nvidia Confidential Computing

### 1. Goals

- **Data and Code Confidentiality**: Protect all application code and data in the VM instance from being read by the host.
- **Data and Code Integrity**: Protect all application code and data in the VM instance from being altered by the host.
- **Basic Physical Attacks**: Interposers on buses such as PCIe and DDR memory cannot leak data or code.

### 2. H100 CC Initialization Process

![H100 CC Initialization Process](/images/GPU_TEE/h100-cc-initialization.png)

## Attesting the GPU

### 1. NVIDIA Remote Attestation Service

![NVIDIA Remote Attestation Service](/images/GPU_TEE/nvidia-remote-attestation.png)

### 2. 流程 (Process)

1.  **验证 GPU 真实性 (Verify GPU Authenticity)**:
    - 从 GPU 设备本身或 NVIDIA 的设备身份服务中，获取一个**设备身份证书（Device Identity Certificate）**。
    - 每个 GPU 芯片中都嵌入了一个**设备唯一标识符（PDI）**，可用于检索证书。
    - 将该证书与 NVIDIA 的证书颁发机构进行比对，即可确认该设备确实由 NVIDIA 制造。
    - 值得注意的是，每个 H100 GPU 中都有一个唯一的**身份私钥（IK）**，这个私钥在制造过程中被烧入熔丝，并且所有副本都被销毁，以确保其唯一性和安全性。
2.  **检查证书是否被吊销 (Check if the certificate is revoked)**
3.  **远程与本地认证服务 (Remote and Local Attestation Services)**
