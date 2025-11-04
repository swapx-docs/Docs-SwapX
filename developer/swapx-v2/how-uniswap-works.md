# 💡 How Uniswap Works

### 💡 How Uniswap Works (The Core Mechanism)

Uniswap is an Automated Liquidity Protocol driven by the Constant Product Formula ($$ $x \cdot y = k$ $$) and implemented through a system of non-upgradeable smart contracts on the Ethereum blockchain. It champions decentralization, censorship resistance, and security by removing the need for trusted intermediaries.

#### 💧 Liquidity Pools

* Each Uniswap smart contract (or pair) manages a Liquidity Pool composed of reserves for two ERC-20 tokens.
* Anyone can become a Liquidity Provider (LP) by depositing an equivalent value of the two underlying tokens, in exchange for Pool Tokens.
* Pool Tokens proportionally track the LP's share of the total reserves and can be redeemed for the underlying assets at any time.

#### 🔄 Automated Market Maker (AMM)

* The token pair acts as an AMM, ready to swap one token for another as long as the Constant Product Formula is maintained: $$ $x \cdot y = k$ $$.
* $$ $x$ $$ and $$ $y$ $$ are the reserve balances, and $$ $k$ $$ (the invariant) is their product, which ideally remains constant from a trade's perspective.
* The formula's property is that larger trades (relative to the reserves) incur a much worse execution rate than smaller trades.

#### 💸 Transaction Fees

* Uniswap charges a 0.30% fee on trades, which is added to the reserves. This effectively increases $$ $k$ $$ with every transaction.
* This fee serves as payment to the LPs, realized when they burn their Pool Tokens to withdraw their share of the total reserves.
* _Note: In the future, this fee may be reduced to 0.25%, with the remaining 0.05% potentially deducted as a protocol-wide fee._

#### ⚖️ Price Equilibrium via Arbitrage

* Since the relative price of the token pair can only change via trades, discrepancies between the Uniswap price and external market prices create arbitrage opportunities.
* This mechanism ensures the Uniswap price consistently tends towards the market clearing price.

#### Further Reading

* Token Swaps: To understand how token exchanges work in practice, see \[Swaps].
* Liquidity Provision: To learn about how Liquidity Pools operate, see \[Pools].
* Protocol Mechanics: To understand the underlying smart contract code, go to \[Smart Contracts].

***

SwapX is committed to building a safe, efficient, and user-friendly decentralized financial platform. Start exploring SwapX and experience the infinite possibilities of the next generation of decentralized trading!
