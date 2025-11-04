# 🤝 Market Maker Integration

## 🤝 Market Maker Integration

### 💡 Basic Concepts of Market Maker Integration

Market Maker Integration is the process of combining the resources and capabilities of multiple liquidity sources—such as market makers, Automated Market Maker (AMM) pools, and other liquidity providers—through technology and strategy.

* Goal: To optimize market liquidity, improve transaction efficiency, and reduce transaction costs.
* Relevance: It is a key mechanism used widely in both traditional financial markets and the crypto space, especially within Decentralized Exchanges (DEXs).

***

### ⭐ Core Goals of Integration

| **Goal**           | **Description**                                                                                                                                       |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Improve Liquidity  | Integrates liquidity from multiple sources to reduce bid-ask spreads and enhance transaction depth.                                                   |
| Optimize Execution | Utilizes intelligent routing to allocate transactions to the best-priced market maker or liquidity pool, ensuring users get the best execution price. |
| Reduce Costs       | Reduces intermediary costs and transaction fees by promoting competitive quotes and minimizing slippage.                                              |
| Enhance Stability  | Allows the integrated system to more efficiently handle extreme market fluctuations, balancing supply and demand to prevent drastic price swings.     |

***

### 🔗 XoneChain Market Maker Integration

SwapX has integrated with market makers on the XoneChain to provide traders with lower-cost transaction execution.

#### 🛣️ Routing with AMM and Market Makers

SwapX employs a smart router that analyzes real-time quotes from two sources for a specific token swap:

1. Existing AMM Liquidity Pool: The built-in SwapX pool (e.g., for XOC/PANDA).
2. External Market Maker: Quotes requested from the integrated market maker for the same transaction.

The router then routes the transaction request to the liquidity source (AMM or Market Maker) that offers the best execution price at that moment.

#### 🪙 Currently Supported Assets

The following assets are currently supported. This list may change (increase/decrease) based on the market maker's operational capacity:

* Major Currencies: XOC, WXOC
* Stablecoin: USDH
* Other Popular Assets: PANDE, PHX, T1, T2, PANDA, PNUTX, PnutX, TEST, NUTX, PEPE

> ⚠️ Important Note: Unlike Automated Market Makers (AMMs) which support arbitrary amounts, market makers cannot trade in arbitrary amounts. The amount they are willing to execute depends entirely on their own available liquidity. It is not uncommon for large orders to be not fully executed. Users are advised to carefully check the quotes to ensure the price and quantity of each transaction meet their requirements.
