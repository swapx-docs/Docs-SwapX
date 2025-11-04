# 🪂Multi-Hop Swaps (V3)

#### 🔗 Multi-Hop Swaps (V3)

These examples illustrate two styles of multi-hop swaps available on V3. **Note:** These are simplified implementations for **learning purposes only** and are **not production-ready code**.

**📜 Contract Setup & Prerequisites**

We declare the Solidity version and enable `abi-coder v2` to allow for encoding/decoding of nested arrays and structs in `calldata`, a feature essential for executing multi-hop swaps.

```solidity
// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity =0.7.6;
pragma abicoder v2;
```

**🛠️ SwapExamples Contract**

We define the `SwapExamples` contract and declare an immutable public variable `swapRouter` of type `ISwapRouter`. This allows us to call functions within the `ISwapRouter` interface.

> Design Consideration: For simplicity, the swap router is passed as a constructor argument in this example. More advanced contracts detail safe inheritance of the swap router.

Solidity

```
contract SwapExamples {
    ISwapRouter public immutable swapRouter;

    // Hardcoded Token Addresses and Pool Fee for this example.
    // In production, these would typically be input parameters for flexibility.
    address public constant DAI = 0x6B175474E89094C44Da98b954EedeAC495271d0F;
    address public constant WETH9 = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2;
    address public constant USDC = 0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48;

    // Pool Fee set to 0.3%
    uint24 public constant poolFee = 3000;

    constructor(ISwapRouter _swapRouter) {
        swapRouter = _swapRouter;
    }
    // ... swap functions follow
}
```

***

#### 1️⃣ Exact Input Multi-Hop

An Exact Input Multi-Hop swap exchanges a fixed amount of an input token for the maximum possible amount of a desired output token, potentially involving any number of intermediate swaps.

In this example, we swap DAI → USDC → WETH9.

**⚙️ Key Inputs**

| **Parameter**      | **Description**                                                                                                              |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- |
| `path`             | A sequence of `(tokenAddress, fee, tokenAddress, ...)` used to calculate the necessary pool addresses for the swap sequence. |
| `recipient`        | The target address for the outbound asset (WETH9).                                                                           |
| `deadline`         | The Unix timestamp after which the transaction will revert.                                                                  |
| `amountIn`         | The fixed amount of the inbound asset (DAI).                                                                                 |
| `amountOutMinimum` | The minimum acceptable amount of the outbound asset. Set to `0` for this simplified example.                                 |

**💻 Function: `swapExactInputMultihop`**

Solidity

```
    /// @notice Swaps a fixed amount of DAI for the maximum possible amount of WETH9 through intermediary pools (DAI -> USDC -> WETH9).
    /// @dev The calling address must approve this contract to spend at least `amountIn` of its DAI.
    /// @param amountIn The amount of DAI to be swapped.
    /// @return amountOut The amount of WETH9 received after the swap.
    function swapExactInputMultihop(uint256 amountIn) external returns (uint256 amountOut) {
        // 1. Transfer `amountIn` of DAI from msg.sender to this contract.
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountIn);

        // 2. Approve the router to spend the DAI.
        TransferHelper.safeApprove(DAI, address(swapRouter), amountIn);

        // 3. Path Encoding: (tokenIn, fee, sharedToken, fee, tokenOut)
        // Path: (DAI, 0.3%, USDC, 0.3%, WETH9)
        ISwapRouter.ExactInputParams memory params =
            ISwapRouter.ExactInputParams({
                path: abi.encodePacked(DAI, poolFee, USDC, poolFee, WETH9),
                recipient: msg.sender,
                deadline: block.timestamp,
                amountIn: amountIn,
                amountOutMinimum: 0 // For production, use SDK or on-chain price oracle
            });

        // 4. Executes the swap.
        amountOut = swapRouter.exactInput(params);
    }
```

***

#### 2️⃣ Exact Output Multi-Hop

An Exact Output Multi-Hop swap exchanges a variable amount of an input token for a fixed, desired amount of an outbound token. This is a less common multi-hop technique.

In this example, we aim to receive a fixed amount of WETH9 by swapping DAI → USDC → WETH9.

**⚙️ Key Inputs**

| **Parameter**     | **Description**                                                                               |
| ----------------- | --------------------------------------------------------------------------------------------- |
| `path`            | A sequence of `(tokenAddress, fee, tokenAddress, ...)` encoded in reverse order of the swaps. |
| `recipient`       | The target address for the outbound asset (WETH9).                                            |
| `deadline`        | The Unix timestamp after which the transaction will revert.                                   |
| `amountOut`       | The fixed, desired amount of the outbound asset (WETH9).                                      |
| `amountInMaximum` | The maximum amount of the input asset (DAI) willing to be spent.                              |

**⚠️ Reverse Path Encoding**

For an exact output swap, the path is encoded in reverse because the swaps are executed in the reverse order of the path to achieve the final desired output.

| **Parameter** | **Encoding Order**                              | **Token Sequence** |
| ------------- | ----------------------------------------------- | ------------------ |
| Exact Input   | `(Token In, Fee, Shared Token, Fee, Token Out)` | DAI → USDC → WETH9 |
| Exact Output  | `(Token Out, Fee, Shared Token, Fee, Token In)` | WETH9 → USDC → DAI |

**💻 Function: `swapExactOutputMultihop`**

Solidity

```
    /// @notice Swaps a minimum possible amount of DAI for a fixed amount of WETH9 through intermediary pools (DAI -> USDC -> WETH9).
    /// @dev Calling address must approve this contract to spend at least `amountInMaximum` of its DAI.
    /// @param amountOut The desired amount of WETH9.
    /// @param amountInMaximum The maximum amount of DAI willing to be spent.
    /// @return amountIn The actual amount of DAI spent.
    function swapExactOutputMultihop(uint256 amountOut, uint256 amountInMaximum) external returns (uint256 amountIn) {
        // 1. Transfer the specified `amountInMaximum` of DAI to this contract.
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountInMaximum);

        // 2. Approve the router to spend `amountInMaximum` of DAI.
        TransferHelper.safeApprove(DAI, address(swapRouter), amountInMaximum);

        // 3. Path Encoding (Reverse): (tokenOut, fee, sharedToken, fee, tokenIn)
        // Path: (WETH9, 0.3%, USDC, 0.3%, DAI)
        ISwapRouter.ExactOutputParams memory params =
            ISwapRouter.ExactOutputParams({
                path: abi.encodePacked(WETH9, poolFee, USDC, poolFee, DAI),
                recipient: msg.sender,
                deadline: block.timestamp,
                amountOut: amountOut,
                amountInMaximum: amountInMaximum
            });

        // 4. Executes the swap, returning the actual amount of DAI spent (`amountIn`).
        amountIn = swapRouter.exactOutput(params);

        // 5. Refund: If less than `amountInMaximum` was spent, refund the difference and reset approval.
        if (amountIn < amountInMaximum) {
            TransferHelper.safeApprove(DAI, address(swapRouter), 0);
            TransferHelper.safeTransferFrom(DAI, address(this), msg.sender, amountInMaximum - amountIn);
        }
    }
```
