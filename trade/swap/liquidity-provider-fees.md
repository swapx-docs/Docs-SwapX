# 💧 Liquidity Provider Fees

## 💸 Fees

SwapX employs a transparent and structured fee mechanism. The fee structure currently focuses on rewarding Liquidity Providers (LPs).

### 💧 Liquidity Provider Fees

A flat 0.3% fee is charged on every token exchange (swap) transaction.

* Fee Distribution: The entire 0.3% fee is immediately added to the liquidity reserve, and is then distributed proportionally to all liquidity providers based on their contribution to the pool.
* Mechanism: The immediate deposit of the exchange fee into the liquidity reserve increases the value of the outstanding liquidity tokens. This serves as a payment to all liquidity providers, proportional to their share in the pool.
* Collection: The fee is collected by burning liquidity tokens to eliminate their proportional share in the base reserve. _\[Note: This is a direct quote from the original source. The standard AMM model usually does not involve burning LP tokens for fee collection.]_
*   Invariant Impact: The addition of the fee to the liquidity pool causes the invariant ($$ $k$ $$) to increment at the end of each transaction.

    > 💡 Invariant: In a single transaction, the invariant represents the $$ $token0\_pool \times token1\_pool$ $$ at the end of the previous transaction.

***

### ⚙️ Protocol Fees

Currently, there are no protocol fees charged on transactions.

* Future Implementation: A 0.05% fee per transaction _within the protocol scope_ may be implemented in the future.
  * This potential 0.05% protocol fee is equivalent to $$ $1/6$ $$ (16.6%) of the standard 0.30% Liquidity Provider Fee.
* Fee Recipient: If the designated `feeTo` address is not the zero address (`address(0)` or `0x00...00`), then the protocol fee is considered valid and the `feeTo` address will be the recipient.
* Impact: The implementation of the protocol fee does not affect the 0.3% fee paid by traders, but it does affect the amount received by liquidity providers (as a portion of the 0.3% would be diverted).
