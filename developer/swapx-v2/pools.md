# 💧 Pools

### 💧 Pools

#### 📝 Introduction

Every SwapX liquidity pool is a trading venue for a pair of ERC20 tokens.

When a pool contract is created, its balance of each token is initially zero. For the pool to begin facilitating trades, someone must seed it with an initial deposit of each token.

**Initial Price Setting**

The first liquidity provider (LP) sets the initial price for the pool. They are strongly incentivized to deposit an equal value of both tokens into the pool.

> Why equal value?
>
> If the first LP deposits tokens at a ratio different from the current market rate, it instantly creates a profitable arbitrage opportunity that external parties are highly likely to exploit.

**Adding Liquidity**

When subsequent LPs add funds to an existing pool, they must deposit the paired tokens proportional to the current price. If they do not, the liquidity they add is also exposed to arbitrage. If they believe the current price is incorrect, they may first arbitrage it to their desired level, and then add liquidity at that price.

***

#### 🪙 Pool Tokens (LP Tokens)

Whenever liquidity is deposited into a pool, a unique token (Liquidity Token or LP Token) is minted and sent to the provider's address. These tokens represent the specific LP's contribution to the pool.

* The proportion of liquidity provided to the pool determines the amount of LP tokens the provider receives.
* If a provider is seeding a new pool, the amount of LP tokens they receive will be equal to $$ $\sqrt{x \times y}$ $$, where $$ $x$ $$ and $$ $y$ $$ represent the amounts of each token provided.

**Fee Accrual and Redemption**

1. Fee Collection: Every time a trade occurs, the sender is charged a 0.3% fee.
2. Fee Distribution: The fee is proportionally distributed among all LPs in the pool upon completion of the transaction.
3. Redemption: To retrieve the underlying liquidity, plus any accrued fees, the LP must "burn" their LP tokens, effectively exchanging them for their share of the liquidity pool _plus_ the proportional fees.

> LP Tokens as Assets: Since LP tokens are themselves tradable assets, liquidity providers can sell, transfer, or otherwise use their LP tokens in any manner they see fit.

***

#### 💡 Why Pools? (The AMM Advantage)

SwapX is unique because it does not use an order book to derive asset prices or match buyers and sellers of tokens. Instead, SwapX uses what are called liquidity pools, powered by an Automated Market Maker (AMM) model.

| **Feature**              | **Traditional Order Book**                                                                                         | **SwapX Liquidity Pool**                                                     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| Liquidity Representation | Discrete orders placed by individuals on a centralized system.                                                     | Pooled assets governed by a smart contract formula ($$ $x \times y = k$ $$). |
| Management               | Requires active management, continuous updating by market makers, and complex infrastructure/algorithms.           | Passive; governed autonomously by code once seeded.                          |
| Infrastructure           | Requires centralized intermediary infrastructure to host and match orders, creating control points and complexity. | Blockchain-native; autonomous smart contract execution.                      |
| Accessibility            | Often limited to advanced traders due to infrastructure and algorithms.                                            | Open, permissionless, and inclusive access model.                            |

**The Blockchain-Native Approach**

While order books are fundamental to finance, they have significant limitations, especially when applied to decentralized environments:

* They require intermediary infrastructure, which creates control points.
* They require the active participation and management of market makers, limiting participation.
* They were invented in a world with relatively few trading assets, making them ill-suited for an ecosystem where anyone can create a new, often low-liquidity token.

SwapX focuses on the strengths of platforms like Ethereum, re-imagining token exchange from first principles. A blockchain-native liquidity protocol should leverage:

* The trusted code execution environment.
* The autonomous, perpetually running virtual machine.
* The open, permissionless, and inclusive access model that results in an exponentially growing ecosystem of virtual assets.

**Interacting with Pools**

To reiterate, a pool is simply a smart contract, which users interact with by calling functions on it:

* Swapping tokens is calling the `swap` function on a pool contract instance.
* Providing liquidity is calling the `deposit` function.

Just as end-users can interact with the SwapX protocol via an interface, developers can interact directly with the smart contracts, integrating SwapX functionality into their own applications without relying on intermediaries or requiring permission.
