# ⚠️ Considerations When Building on SwapX

### ⚠️ Considerations When Building on SwapX

When integrating SwapX V2 into another on-chain system, you must take special care to avoid security vulnerabilities, manipulation vectors, and the potential loss of funds.

> Key Integration Levels
>
> It's important to first state that smart contract integration can happen at two levels:
>
> * Directly with the Pair Contracts: This offers maximum flexibility but requires the most work to get right.
> * Through the Router: This offers more limited functionality but provides stronger safety guarantees.

There are two primary categories of risk when building on SwapX V2.

***

#### 1. "Static" Errors

The first category involves what might be called "static" errors. These are mistakes that can be identified without considering external market conditions.

Examples include:

* Sending too many tokens to a pair during a swap.
* Requesting too few tokens in return from a swap.
* Allowing a transaction to sit in the mempool for too long, to the point where the sender's expectations about the price are no longer accurate.

These errors can be resolved with fairly straightforward logical checks. Performing these logical checks is the primary purpose of the Router.

Those who interact with pairs directly must perform these checks themselves (with help from the [library](https://www.google.com/search?q=insert-link-to-library-docs-if-available\&authuser=3)).

***

#### 2. "Dynamic" Risks

The second category involves "dynamic" risks related to runtime pricing. Because Ethereum transactions occur in an adversarial environment, naive smart contracts can and will be exploited for profit.

For example, imagine a smart contract that checks the asset ratio in a SwapX pool at runtime and conducts a trade, assuming that this ratio represents the "fair" or "market" price of those assets.

This contract is highly vulnerable to manipulation.

A malicious actor could easily insert transactions before and after the naive transaction (a "sandwich" attack). This would cause the smart contract to trade at a terrible price, profiting the attacker at the trader's expense, before returning the contract to its original state—all at a low cost.

> A Note on Mitigation
>
> An important caveat is that these types of attacks can be mitigated by trading in high-liquidity pools or by trading at low values.
