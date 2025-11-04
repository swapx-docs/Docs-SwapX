# ⚖️ Fee Structure

#### 1. Liquidity Provider Fees

| Item                 | Detail                                                                                                                                                                                                                                                |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Fee Rate**         | **0.3%** of the swapped token value.                                                                                                                                                                                                                  |
| **Distribution**     | Allocated proportionally to liquidity providers (LPs) based on their contribution to the reserves.                                                                                                                                                    |
| **Mechanism**        | The swap fee is **immediately deposited** into the liquidity reserve, increasing the value of the liquidity tokens. The fees are effectively paid to LPs by burning their liquidity tokens to remove a proportional share of the underlying reserves. |
| **Invariant Impact** | Since fees are added to the liquidity pool, the invariant increases at the end of every transaction. In a single transaction, the invariant represents `token0_pool / token1_pool` at the end of the previous transaction.                            |

#### 2. Protocol Fees

| Item                     | Detail                                                                                                                                        |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **Current Status**       | Currently **no** protocol fees are charged.                                                                                                   |
| **Future Potential**     | A fee of **0.05%** may be levied in the future.                                                                                               |
| **Proportion**           | This 0.05% fee is equivalent to **⅙ (16.6̅%)** of the total 0.30% fee.                                                                        |
| **Impact**               | This amount does **not** affect the fee paid by the trader, but it reduces the amount received by the liquidity provider.                     |
| **Activation Condition** | The fee becomes effective if `feeTo` is not `address(0)` (`0x0000000000000000000000000000000000000000`), indicating a recipient for the fees. |
