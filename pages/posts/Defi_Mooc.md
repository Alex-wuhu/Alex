---
title: DeFi MOOC from Berkeley
date: 2025-09-29
lang: en
duration: 10min
tags:
  - Defi
  - Blockchain
---

[[toc]]

## Stablecoins

### Three Types of Stablecoins

1.  **Reserve-Based (储备型货币)**

    - The most common type.
    - Each stablecoin is backed by an equivalent value of real-world assets.
    - **Examples:** Tether (USDT), USD Coin (USDC).

2.  **Collateral-Based (加密资产抵押型稳定币)**

    - Backed by other cryptocurrencies (e.g., Ethereum, Bitcoin).
    - **How it works:** To mint 100 stablecoins, you might need to lock up $150 worth of Ethereum as collateral. If the value of the collateral drops below a certain threshold, it's automatically liquidated to protect the stablecoin's value.
    - **Example:** Dai (DAI), managed by the decentralized MakerDAO protocol.

3.  **Algorithmic (算法型稳定币)**
    - Not backed by any assets. Instead, algorithms are used to maintain price stability.
    - **How it works:** The protocol increases the money supply when the price is above $1 and decreases it when the price is below $1.
    - These can be extremely fragile and prone to "death spirals" during market panic.

## Automated Market Makers (AMM)

### Price Curves

1.  **Uniswap Invariant (x \* y = k)**

    - **Core Idea:** Maintains a constant product `k` of the quantities of two assets, `x` and `y`.
    - **Price Curve:** A hyperbola. The price is stable when the pool is balanced, but changes dramatically (high slippage) when one asset is heavily traded.
    - **Use Case:** Suitable for any pair of volatile assets (e.g., ETH/DAI).

2.  **Stableswap Invariant (Curve)**
    - **Core Idea:** A hybrid model that behaves like `x + y = k` when prices are close to 1:1 and transitions to `x * y = k` as prices diverge.
    - **Price Curve:** Nearly a straight line around the 1:1 price point, resulting in very low slippage for large trades.
    - **Use Case:** Designed for assets with similar prices, like stablecoins (USDT/USDC/DAI) or wrapped assets (WBTC/renBTC).

### Liquidity Mining

> Liquidity mining is a way to incentivize users to provide liquidity to a pool.

- **Rewards:** Liquidity providers earn rewards from two sources:
  - Trading fees
  - Liquidity mining rewards (e.g., governance tokens)
- Rewards are proportional to the amount of liquidity provided.
- Liquidity can be added or removed at any time.

### Impermanent Loss

> Impermanent Loss is the potential loss you experience by providing liquidity to a pool compared to just holding the assets (HODLing).

![Impermanent Loss Diagram](/images/Defi_Mooc/impermanent-loss.png)

## Arbitrage

> Arbitrage is the practice of profiting from price differences for the same asset across multiple markets. Arbitrageurs help synchronize prices.

### Bellman-Ford Algorithm (DeFiPoser-ARB)

- Used for negative cycle detection to find arbitrage opportunities across multiple markets.
- **Profitable Condition:** `p1 * p2 * p3 > 1` or `(-logP1) + (-logP2) + (-logP3) < 0`

![Bellman-Ford Arbitrage Diagram](/images/Defi_Mooc/bellman-ford.png)

### Theorem Solver (DeFiPoser-SMT)

- Another method for finding arbitrage opportunities by encoding the DeFi model and using heuristics for path pruning.

![Theorem Solver Diagram](/images/Defi_Mooc/theorem-solver.png)

## Lending and Borrowing

![On-chain Architecture Diagram](/images/Defi_Mooc/on-chain-architecture.png)

- **Drawbacks:** On-chain lending protocols cannot use real-world information like assets, salary, or debt history.
- **Leverage (Debt Multiplier):** A strategy to amplify your position by repeatedly borrowing and re-hypothecating assets.

### Key Terminology

1.  **Collateral:** Assets that serve as a security deposit for a loan.
2.  **Over-Collateralization:** The value of the collateral is greater than the value of the loan. This is the standard in DeFi.
3.  **Under-Collateralization:** The value of the collateral is less than the value of the loan.
4.  **Liquidation:** If the value of the collateral falls below a certain threshold (e.g., 150% of the debt value), anyone can liquidate the debt position.
5.  **Health Factor:** A score representing the safety of your loan. A health factor below 1 is at high risk of liquidation.
    ![Health Factor Diagram](/images/Defi_Mooc/health-factor.png)
6.  **Liquidation Spread (清算价差):** A bonus or discount that a liquidator receives for liquidating a position. This incentivizes liquidators to participate.
    > Value of Collateral to Claim = Value of Debt to Repay \* (1 + Liquidation Spread)
7.  **Close Factor:** The maximum proportion of a debt that can be repaid in a single liquidation event. This protects borrowers from losing all their collateral at once.
    > Value of Debt to Repay < Close Factor \* Total Value of Debts

---

## Reference

- [DeFi MOOC, Fall 2022 - Lecture 1: DeFi Overview](https://www.youtube.com/watch?v=xeST_tbc1O4&list=PLS01nW3RtgopKAIGCW92jePNHwTGk5GmK)
