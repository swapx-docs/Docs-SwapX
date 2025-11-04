# 💰 Understanding Returns (Impermanent Loss)

### 💰 Understanding Returns (Impermanent Loss)

SwapX incentivizes users to add liquidity to trading pools by rewarding providers with fees generated when other users trade with those pools.

Note: Market making is often a complex activity. There is a risk of loss compared to simply holding the assets (HODLing), especially during periods when the underlying asset price fluctuates significantly and continuously.

***

#### 💧 Liquidity Provision Example

Let's walk through a concrete example of how liquidity provision and returns are calculated.

| **Event**     | **LP Action**                                                                                 | **Pool State**                                                                                          | **LP Share**           |
| ------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- | ---------------------- |
| Initial State | LP adds 10,000 DAI and 100 WETH (Total value: $20,000)                                        | Total Pool: 100,000 DAI and 1,000 WETH                                                                  | 10% of total liquidity |
|               | The contract mints and sends "liquidity tokens" to the LP, entitling them to 10% of the pool. | _Note: These tokens are non-speculative and are only an accounting tool to track the LP's entitlement._ |                        |

If other people subsequently add/remove tokens, new liquidity tokens are minted/burned to keep everyone's relative percentage share of the pool constant.

#### 📉 Impact of Price Change

Now, let's assume the price on an external market (Coinbase) rises from $100 to $150 per WETH.

1. Arbitrage: After some arbitrage, the SwapX contract should reflect this change. Traders will add DAI and remove WETH until the new ratio is 150:1.
2. New Pool State: The new pool figures will be approximately 122,400 DAI and 817 WETH.
   * _Check:_ $$ $122,400 \times 817 \approx 100,000,000$ $$ (our constant product) and $$ $122,400 / 817 \approx 150$ $$ (our new price).
3. LP Withdrawal: The LP's 10% share now yields 12,240 DAI and 81.7 WETH.
   * Total Market Value: $$ $\approx \$24,500$ $$.
4. HODL Value: If the LP had simply held their initial $$ $10,000$ $$ DAI and $$ $100$ $$ WETH, the total value would be: $$ $10,000 + (100 \times \$150) = \$25,000$ $$.
5. Loss Due to Market Making: The LP has incurred a loss of approximately $500 in profit compared to HODLing.

***

#### 🎁 Earning Swap Fees

Clearly, no one provides liquidity purely out of charity.

Instead, 0.3% of all trading volume is distributed proportionally to all liquidity providers.

* By default, these fees are put back into the liquidity pool, but they can be claimed at any time.
* The overall return is a trade-off between the fee income and the losses caused by directional price movements.
* The more price fluctuation (back and forth) a pool experiences, the better for LPs.

***

#### ❓ Why Might My Liquidity Value Decrease?

To understand why an LP's share value might drop despite fee income, we need to look closer at the formula SwapX uses to manage trades.

1\. The Constant Product Formula (Ignoring Fees):

\$$\text{eth\\\_liquidity\\\_pool} \times \text{token\\\_liquidity\\\_pool} = \text{constant\\\_product}\$$

2\. Price Relationship:

For trades that are very small compared to the pool size:

\$$\text{eth\\\_price} \approx \frac{\text{token\\\_liquidity\\\_pool\}}{\text{eth\\\_liquidity\\\_pool\}}\$$

3\. Pool Size at Any Price:

Combining these, we can calculate the size of each pool at any given price (assuming total liquidity remains constant):

\$$\text{eth\\\_liquidity\\\_pool} = \sqrt{\frac{\text{constant\\\_product\}}{\text{eth\\\_price\}}}\$$\$$\text{token\\\_liquidity\\\_pool} = \sqrt{\text{constant\\\_product} \times \text{eth\\\_price\}}\$$

#### 🌊 Impermanent Loss Explained

Let's examine the effect of a price change on an LP's share.

| **Scenario**       | **Details**                                                                     |
| ------------------ | ------------------------------------------------------------------------------- |
| Initial Investment | LP provides 1 XOC and 100 DAI (1% of a pool containing 100 XOC and 10,000 DAI). |
| Initial Price      | 1 XOC = 100 DAI.                                                                |
| New Price          | 1 XOC = 120 DAI (a 1.2x change).                                                |

**The Loss Calculation:**

1. New Pool Reserves (using formulas):
   * $$ $\text{eth\_liquidity\_pool} \approx 91.2871$ $$ (XOC)
   * $$ $\text{dai\_liquidity\_pool} \approx 10,954.4511$ $$ (DAI)
2. LP's New Share (1%): $$ $0.9129$ $$ XOC and $$ $109.54$ $$ DAI.
3. Current Value of LP Share (converted to DAI): $$ $(0.9129 \times 120) + 109.54 \approx 219.09$ $$ DAI.
4. HODL Value: The initial $$ $1$ $$ XOC and $$ $100$ $$ DAI is now worth $$ $(1 \times 120) + 100 = 220$ $$ DAI.
5. Impermanent Loss: The LP lost $$ $\approx 0.91$ $$ DAI by providing liquidity instead of HODLing.

This loss is called Impermanent Loss because it disappears if the price returns to the level it was when the LP added liquidity.

#### 📏 Impermanent Loss Formula

We can derive a formula to calculate the magnitude of impermanent loss based on the price ratio between the time of supply and now:

\$$\text{impermanent\\\_loss} = \frac{2 \times \sqrt{\text{price\\\_ratio\}}}{1 + \text{price\\\_ratio\}} - 1\$$

| **Price Change (Ratio)**  | **Loss Relative to HODL** |
| ------------------------- | ------------------------- |
| 1.25x                     | 0.6% loss                 |
| 1.50x                     | 2.0% loss                 |
| 1.75x                     | 3.8% loss                 |
| 2x (Price Doubles/Halves) | 5.7% loss                 |
| 3x                        | 13.4% loss                |
| 4x                        | 20.0% loss                |
| 5x                        | 25.5% loss                |

> Crucial Note: The loss is the same regardless of the direction the price changes (i.e., the loss from the price doubling is the same as the loss from it halving).
