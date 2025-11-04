# 🏷️ How is the Price Determined?

### 🏷️ How is the Price Determined?

As we learned in the protocol overview, every pair on SwapX is actually underpinned by a liquidity pool. A liquidity pool is a smart contract that holds the balances of two different tokens and enforces rules about depositing and withdrawing them.

The primary rule is the constant product formula ($$ $x \times y = k$ $$).

When a token is withdrawn (bought), a proportional amount of the other token must be deposited (sold) to maintain this constant. The ratio of tokens in the pool, combined with the constant product formula, ultimately determines the price at which a swap is executed.

***

### ⚙️ How SwapX Handles Prices

#### V1: On-Chain Calculation

In SwapX V1, trades were always executed at the "best" price, which was calculated at the moment of execution. This calculation was done using one of two different formulas, depending on whether the trade specified an exact input or an exact output amount. The functional difference between these two was minimal, but the mere existence of two methods added conceptual complexity.

#### V2: Periphery-Driven Pricing

An initial attempt to support both functions in V2 proved to be inelegant. Therefore, the decision was made not to provide any pricing functions in the core contracts.

Instead, the V2 pair contract simply checks _after_ each trade that the invariant ($$ $x \times y = k$ $$) is satisfied (accounting for fees).

This approach has two major benefits:

1. Separation of Concerns: Rather than enforcing the invariant via a pricing function, V2 simply ensures its own security by transparently verifying the invariant _after_ the fact.
2. Flexibility: V2 pairs can more naturally support other potential trade types that may emerge (e.g., trading at a specific price at the time of execution).

At a high level, this means that in SwapX V2, trades must be priced in the periphery (i.e., by contracts _outside_ the core pair).

The good news is that the SwapX library provides various functions designed to make this process very simple, and all swap functions in the router are designed with this in mind.

***

### 📈 Pricing Trades

When swapping tokens on SwapX, you generally want one of two outcomes:

1. Receive the maximum possible amount of output tokens for an exact input amount.
2. Pay the minimum possible amount of input tokens for an exact output amount.

To calculate these amounts, a contract must look up the pair's current reserves to see what the current price is.

#### The Oracle Problem

However, it is not safe to perform this lookup and rely on the result without access to an external price feed (an oracle).

Let's imagine a naive smart contract wants to send 10 DAI to the DAI/WETH pair and receive as much WETH as possible based on the current reserve rate. If this naive contract simply looks up the current price and executes the trade at that price, it is highly vulnerable to front-running and can suffer significant economic losses.

To understand why, consider a malicious actor who sees this transaction before it is confirmed:

1. The attacker executes a swap _just before_ the naive swap, drastically changing the DAI/WETH price.
2. The naive swap executes at this new, unfavorable exchange rate.
3. The attacker then swaps back, restoring the price to what it was before the naive swap and capturing a profit.

This type of attack is relatively cheap, low-risk, and often profitable.

#### The Solution: Oracles and Slippage

To prevent such attacks, a swap must be submitted with knowledge of the "fair" price at which it _should_ execute. In other words, the swap needs access to an oracle to ensure the "best" execution it gets from SwapX is close enough to what the oracle considers the "true" price.

> What is an oracle?
>
> An oracle can be as simple as an off-chain observation of a pair's current market price.

Due to arbitrage, the in-block reserve ratio of a pair is typically close to the "true" market price. Therefore, if a user submits a transaction with this knowledge, they can ensure that their losses due to front-running are strictly limited.

This is exactly how the SwapX frontend ensures transaction safety.

* It calculates the optimal input/output amount based on the _observed in-block price_.
* It then executes the swap using the router, which guarantees the swap will execute at a rate no worse than $$ $x\%$ $$ difference from the observed in-block rate.
* $$ $x$ $$ is the user-specified slippage tolerance (which defaults to 0.5%).

***

#### ➡️ Exact Input

If you wish to send an exact amount of input tokens in exchange for as many output tokens as possible, you need to use `getAmountsOut`.

* SDK Equivalent: `getOutputAmount` (or `minimumAmountOut` for slippage calculations).

#### ⬅️ Exact Output

If you wish to receive an exact amount of output tokens for as few input tokens as possible, you need to use `getAmountsIn`.

* SDK Equivalent:
