---
description: Trade
---

# 💸 What is Trade

## Token Swap

Token exchange on SwapX is a **peer-to-peer asset exchange** achieved entirely through blockchain smart contracts.

***

### Core Mechanism: Non-Custodial Trading

Decentralized exchange ensures you maintain full control of your assets:

* **User Control:** Users always control their **private keys** via personal wallets (like MetaMask).
* **Direct Settlement:** Assets are **not deposited** into the exchange. Smart contracts complete the settlement directly on the blockchain.

#### Exchange Implementation Methods

SwapX utilizes the **AMM model**, contrasting with the Order Book method:

| Method              | Mechanism                                     | Pricing                                                                                                          | Example               |
| ------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------- |
| **AMM Mode**        | Based on **Liquidity Pools**                  | Price is calculated automatically by the ratio of tokens in the pool, following a formula like **$x \* y = k$**. | SwapX (Uniswap V2/V3) |
| **Order Book Mode** | On-chain order matching of buy/sell requests. | Price determined by the best match between bid and ask orders.                                                   | dYdX                  |

> **SwapX Trading:** Token Exchange on SwapX is a simple method where liquidity comes directly from the **liquidity pools** created on SwapX (`https://swapx.exchange/pools`).

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

### 💸 Exchange Fees

When you swap (trade) tokens on the SwapX exchange, you pay a transaction fee based on the liquidity pool type used for the swap.

#### Liquidity Provider Fees

* **Fee Rate:** A fixed fee of **0.3%** is charged for swapping tokens.
* **Distribution:** This fee is instantly shared by **Liquidity Providers (LPs)** in proportion to their contribution to the liquidity reserve.
* **Mechanism:** The swap fees are immediately deposited into the liquidity reserve. This increases the total value of the pool, thereby increasing the value of the LP Tokens as a payment to all liquidity providers proportional to their pool share. Fees are collected when LPs burn their tokens to redeem their proportional share of the underlying reserve.

#### Protocol Fees

* **Current Status:** There are **currently no protocol fees**.
* **Future Potential:** A **0.05% protocol fee** may come into effect in the future.
  * This fee is equivalent to $\frac{1}{6}$ of the 0.30% fee (approximately $16.6\overline{6}%$).
  * **Recipient:** If the `feeTo` address is not the null address (`address(0)`), the fee is valid, and the `feeTo` address is the recipient.
  * **Impact:** This amount **does not affect the fees paid by traders** but does affect the net amount received by liquidity providers, as a fraction of the 0.3% is diverted to the protocol.

