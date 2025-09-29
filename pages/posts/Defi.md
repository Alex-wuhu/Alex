---
title: Defi lecture
date: 2025-09-29
lang: en
duration: 10min
---

[[toc]]

## Stable coin

### Three stablecoin Types

1. Reserve-Based(储备型货币) ： 最常见
   - 每发行一个稳定币，背后都有**等值的、现实世界中**的资产作为储备。
   - **代表项目：** **Tether (USDT)** 和 **USD Coin (USDC)**。
2. Collateral-Based (加密资产抵押型稳定币)
   - **工作原理：** 假设你想铸造 100 个稳定币。你需要抵押价值超过 100 美元的加密资产（比如价值 150 美元的以太坊）。如果以太坊价格下跌，你的抵押物价值低于某个阈值，系统会自动清算，以保护稳定币的价值。
   - **以加密资产**（如以太坊、比特币等）作为抵押物
   - **代表项目：** **Dai (DAI)**。它是去中心化的，由 MakerDAO 协议管理
3. Algorithmic Based (算法型稳定币)
   - 它们不依赖任何实体资产或加密资产抵押，**而是通过算法来维持价格稳定**。
   - 简单来说，当稳定币价格高于 1 美元时，协议会通过算法增加货币供应量；当价格低于 1 美元时，则减少供应量。
   - 极端脆弱，一旦市场恐慌或算法失灵，可能会导致“死亡螺旋”，使稳定币价格迅速崩盘。

## AMM

### Price Curve

1. Uniswap Invariant (x \* y = k)
   - **核心思想：** 保持两种资产数量的乘积为一个常数 `k`。
   - `x` = 资产 A 的数量
   - `y` = 资产 B 的数量
   - `k` = 恒定的乘积
   - **价格曲线特性：** 这条曲线是一个双曲线（Hyperbola）。
     - 当流动性池中的两种资产数量接近时，价格变动相对平稳。
     - 当其中一种资产被大量兑换，导致其数量大幅减少时，价格曲线会变得非常陡峭，价格急剧上涨，这被称为**滑点（Slippage）**。
   - **适用场景：** Uniswap 的 `x * y = k` 模型适用于任何波动性资产的交易对，如 ETH/DAI、WBTC/ETH 等。它的设计目标是在整个价格区间内提供流动性，以适应资产价格的大幅波动。
2. Stableswap Invariant (Curve)
   - **核心思想：** 这是一种混合型曲线，结合了加法和乘法。在价格接近 1:1 的时候，使用**加法（`x + y = k`）**模型，在价格偏离 1:1 较远时，再逐渐过渡到**乘法（`x * y = k`）**模型。
   - 具体公式非常复杂，核心思想是：`x + y` 接近于 `k`，同时 `x * y` 也接近于 `k`。

- **价格曲线特性：**
  - 在价格等于或接近 1:1 的区域，曲线非常平坦，几乎是一条直线。这意味着即使是大额交易，滑点也极小。
  - 当价格偏离 1:1 越来越远时，曲线逐渐弯曲，变得像 Uniswap 的双曲线。
- **适用场景：** 专门为交易价格相近的资产（如稳定币 USDT/USDC/DAI，或者包装过的比特币 WBTC/renBTC）而设计。

### Liquidity Mining = Incentive

- 2 Types of rewards in Defi Pools
  - Trading fees
  - Liquidity Mining rewards
- Liquidity Mining
  - An incentive to provide liquidity to a pool
  - Proportional rewards in terms of liquidity
  - Can be added/removed anytime

### Impermanent Loss

- 无常损失（Impermanent Loss）是当你把资产放入流动性池中，因为资产价格波动而产生的一种**潜在损失**。
- 简单来说，**这个损失就是你将资产放入流动性池，与你单纯持有（HODL）这些资产相比，所造成的价值差额。**
  ![[Pasted image 20250907182751.png]]

## Arbitrage

- Multiple Markets with
  - The same assets X and Y
  - Different prices for X and Y
- Prices are synchronized by "arbitrageurs"
  - Profit from the price difference
  - Requires to perform at least one transaction

### Bellman Ford algorithm(DeFiPoser-ARB)

- Negative cycle detection
- Works among multiple markets
- Used in traditional finance in Defi
  ![[Pasted image 20250907181155.png]]
  Profitable condition : p1xp2xp3 >1 or (-logP1) + (-logP2)+(-logP3) <0

### Theorem Solver(DeFiPoser-SMT)

- Needs to encode the Defi model
- Apply heuristics for path pruning
  ![[Pasted image 20250907181348.png]]

## Lending and Borrowing

- On-chain Architecture ![[Pasted image 20250922232506.png]]

- **Drawbacks** : real world所有的info均无法使用(assets , salary , debt)
- **Leverage == A debt multiplier**
  - 杠杆（leverage）可以被非常形象地理解为**“债务乘数”** , 通过**反复借贷和再抵押，来放大你的资产头寸和相应债务的策略**。简单来说，你不是一次性借一笔钱，而是利用借来的钱，作为新的抵押品，从而借到更多的钱。这个循环每重复一次，你的债务和风险敞口就会像滚雪球一样变大。

### Terminology

1. Collateral : Assets that serve as a security deposit
2. Over-Collateralization : Borrower has to provide => **Free to use**
   - Value(collateral assets) > value (granted loan)
3. Under-Collateralization => **Restricted**
   - Value(collateral assets) < value (granted loan)
4. Liquidation
   - If value(collateral) <= 150% x value(debt)
   - anyone can liquidate the debt position
5. Health Factor![[Pasted image 20250922234048.png]]
   - When the health < 1 : 很容易被清算
6. Liquidation Spread (清算价差)
   - Bonus , or discount , that a liquidator can collect when liquidating collateral
   - Value of Collateral to claim = **Value of Debt to Repay X (1+LS)**
   - 主要作用是**激励清算人（liquidators）**。清算人是那些监控借贷协议并执行清算操作的外部参与者。当清算条件被满足时，他们会介入，偿还借款人的债务，并作为回报，获得一部分被清算抵押品。
7. Close Factor :
   - maximum proportion of the debt that is allowed to be repaid in a single fixed spread liquidation(可以被强制偿还的**最大债务比例** , 防止他们在一次清算中失去全部抵押品。)
   - Value of **Debt to Repay < CF X Total value of Debts**
