# 🍇Understanding Liquidity Provider Returns

#### 💰 Understanding Liquidity Provider Returns

SwapX incentivizes users to provide liquidity by rewarding them with fees generated from trading activities within the pools. However, acting as a market maker is a complex activity that carries specific risks.

**Impermanent Loss Risk ⚠️**

There is a risk of loss (known as **Impermanent Loss**) during periods of significant and sustained price volatility in the underlying asset, compared to simply holding the asset (HODL).

***

#### 💧 The Mechanism of Liquidity Tokens

When a user provides liquidity, the protocol issues non-tradable **"liquidity tokens"** to track their ownership share in the pool.

* **Example Scenario:**
  * An LP adds **10,000 DAI** and **100 WETH** (Total Value: $20,000).
  * The pool currently holds **100,000 DAI** and **1,000 ETH**.
  * The LP's share is $\frac{10,000}{100,000} = \mathbf{10%\}$ of the total liquidity.
  * The contract mints liquidity tokens entitling the LP to **10%** of the pool's assets.
* **Accounting Tool:** Liquidity tokens are simply an accounting tool used to track the percentage share each provider is entitled to. As others add or withdraw tokens, new liquidity tokens are minted or burned to maintain the relative percentage share.

***

#### 📉 Price Change Impact (Impermanent Loss Illustrated)

Let's examine the effect of a price change using the Constant Product Formula: $$x * y = k$$ Where $x$ and $y$ are the quantities of the two tokens, and $k$ is the constant product. The price is $P = y/x$.

**Scenario: Price Doubling from $100 to $150**

| Metric                 | Initial State (Price: $100)            | After Arbitrage (New Price: $150) | LP Withdrawal (10% Share)                       |
| ---------------------- | -------------------------------------- | --------------------------------- | ----------------------------------------------- |
| Pool Assets (DAI, ETH) | 100,000 DAI, 1,000 ETH                 | 122,400 DAI, 817 ETH              | 12,240 DAI, 81.7 ETH                            |
| LP Initial HODL Value  | $10,000 + (100 \times $150) = $25,000$ | N/A                               | N/A                                             |
| LP Withdrawal Value    | N/A                                    | N/A                               | $(12,240) + (81.7 \times $150) \approx $24,500$ |

> **Conclusion:** By providing liquidity, the LP's final value (\$$24,500$) is less than the HODL value (\$$25,000$). This difference is the unrealized **Impermanent Loss** (approximately \$$500$ in this specific scenario).

***

#### 💸 Fee Revenue vs. Impermanent Loss

No one provides liquidity through charity. The primary revenue source for LPs is the trading fees:

* **Fee Distribution:** **0.3%** of all trading volume is distributed proportionally to all liquidity providers.
* **Collection:** By default, these fees are returned to the liquidity pool, but LPs can collect them at any time.

The profitability of providing liquidity depends on the trade-off between the accumulated fee revenue and the potential impermanent loss from directional price changes.

> **Key Takeaway:** The more **back-and-forth volatility** a token pair experiences (meaning more trades), the better for LPs, as it generates more fees without a sustained, one-directional price change that causes significant Impermanent Loss.

***

#### 📉 Calculating Impermanent Loss

The phenomenon where the value of a liquidity provider's stake decreases relative to simply holding the underlying assets is called **Impermanent Loss (IL)**. This loss is temporary and disappears if the price returns to the initial level.

We can quantify Impermanent Loss based on the price ratio ($P\_r$) between the current price and the price at the time of deposit:

$$\text{Impermanent Loss} = \frac{2 \times \sqrt{P_r}}{1 + P_r} - 1$$

**Price Change vs. Loss**

The loss is symmetrical, meaning doubling the price results in the same loss as halving it.

| Price Change ($P\_r$) | Impermanent Loss (Relative to HODL) |
| :-------------------: | :---------------------------------: |
|       **1.25x**       |              0.6% loss              |
|       **1.50x**       |              2.0% loss              |
|  **2x** (Double/Half) |            **5.7% loss**            |
|         **3x**        |              13.4% loss             |
|         **4x**        |              20.0% loss             |
|         **5x**        |              25.5% loss             |
