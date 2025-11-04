# 🔄 Swaps

### 🔄 Swaps

#### 📝 Introduction

A token swap in SwapX is a straightforward way to exchange one ERC-20 token for another.

For the end-user, the swap is intuitive:

1. The user selects an input token and an output token.
2. They specify the input amount, and the protocol calculates how many output tokens they will receive.
3. They execute the swap with a single click and instantly receive the output tokens in their wallet.

In this guide, we will look at what happens at the protocol level during a swap to gain a deeper understanding of how SwapX works.

**Mechanism: AMM vs. Order Book**

Swaps in SwapX differ from those on traditional platforms. SwapX does not use an order book to represent liquidity or determine prices. Instead, SwapX uses an Automated Market Maker (AMM) mechanism to provide instant feedback on rates and slippage.

As covered in the protocol overview, every pair on SwapX is underpinned by a liquidity pool. This pool is a smart contract that holds the balances of two unique tokens and enforces rules for deposits and withdrawals.

The governing rule is the constant product formula ($$ $x \times y = k$ $$). When either token is withdrawn (bought), a proportional amount of the other token must be deposited (sold) to keep the constant product maintained.

***

#### 🔬 Anatomy of a Swap

At the most fundamental level, all swaps in SwapX V2 occur within a single function, aptly named `swap`:

Solidity

```
function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data);
```

**Receiving Tokens**

It is clear from the function signature that SwapX requires the `swap` caller to specify how many output tokens they wish to receive via the `amount0Out` and `amount1Out` parameters, corresponding to the desired quantity of `token0` and `token1`.

**Sending Tokens (Payment)**

What is less obvious is how SwapX receives the tokens that serve as payment for the swap.

Typically, a smart contract requiring tokens to perform a function asks the caller to first perform an `approve` on the token contract, and then calls a function that, in turn, calls `transferFrom` on the token contract.

This is not how the V2 Pair accepts tokens.

Instead, the pair checks its token balances at the end of every interaction. Then, at the beginning of the next interaction, the current balance is compared against the stored value to determine the quantity of tokens sent by the current interactor.

> Key Takeaway:
>
> The tokens must be transferred into the Pair _before_ calling `swap`. (The only exception to this rule is [Flash Swaps](https://www.google.com/search?q=insert-link-to-flash-swap-docs-if-available\&authuser=3)).
>
> This means that to safely use the `swap` function, it must be called from another smart contract. The alternative method (transferring tokens to the pair and _then_ calling `swap`) is unsafe in a non-atomic context because the sent tokens would be vulnerable to immediate arbitrage.
