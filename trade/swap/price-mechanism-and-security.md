# 💰 Price Mechanism & Security

## 💰 Price Mechanism & Security

SwapX's price determination is rooted in the principles of Automated Market Makers (AMM) and is reinforced with transaction security measures in V2.

### ⚖️ How is Price Determined? (Constant Product)

Each token pair on SwapX is backed by a Liquidity Pool—a smart contract that holds balances of two tokens.

* Core Rule: The pool enforces the constant product formula ($$ $x \cdot y = k$ $$).
* Price Dynamics: When withdrawing (buying) tokens, a proportional amount of the other token must be deposited (sold) to maintain the constant ($$ $k$ $$).
* Final Price: The ratio (proportion) of tokens currently held in the pool, combined with the constant product formula, ultimately determines the price at which the swap is executed.

***

### 🔄 SwapX V2 Price Handling

SwapX V2 shifts the responsibility for pricing away from the core contract, streamlining the design and enhancing security.

* V1 Legacy: V1 calculated the "best" execution price using one of two different formulas depending on whether exact input or output was specified, which added conceptual complexity.
* V2 Evolution: V2 removes all pricing functions from the core.
  * Instead, V2 pairs simply and transparently ensure their own security by checking the invariants directly _after_ each trade (while accounting for fees).
  * This design ensures a good separation of concerns by forcing transactions to be priced externally.
* Implementation: The SwapX library provides various functions to simplify this external pricing process, which is utilized by all swap functionality in the router.

***

### 🛡️ Transaction Pricing & Front-Running Prevention

When swapping, users want to receive the maximum output for a given input, or pay the minimum input for a given output. However, relying on a simple reserve ratio lookup is unsafe.

* The Risk (Front-Running): A smart contract that naively executes a swap based on the pair's current reserve ratio is vulnerable to front-running attacks.<sup>1</sup> A malicious actor can execute a small swap right before the naive transaction to manipulate the price, let the transaction execute at an unfavorable rate, and then swap back, profiting from the victim's loss.
* The Solution (Slippage & Oracles): A swap must be submitted with knowledge of the "fair" price at which it should be executed.
  * This requires the swap to ensure that the execution price obtained from SwapX is sufficiently close to what an external observation (an "oracle") considers the "true" market price.
  * Security Mechanism: Due to arbitrage, a pair's in-block reserve ratio is typically close to the true market price. Users submit transactions with this knowledge in mind to severely limit losses.
  * Frontend Example: The SwapX frontend calculates the optimal input/output based on the observed in-block price and performs the swap using a router, guaranteeing the exchange rate is no less than a user-specified slippage tolerance (default 0.5%) of the observed rate.
