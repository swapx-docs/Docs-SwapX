# 🏂Single-Hop Swaps

````
### 🔄 Single-Hop Swaps

Swaps are the most common interaction with the SwapX protocol. These examples demonstrate two single-path swap implementations using the `swapExactInputSingle` and `swapExactOutputSingle` functions.

> **Important Note:** These swap examples are **not production-ready code** and are simplified for **learning purposes**. When transacting via smart contracts, always integrate an external price source (oracle) to prevent substantial loss due to poor execution price.

#### 📜 Contract Setup & Prerequisites

We declare the necessary Solidity version and enable `abi-coder v2`, which is required for encoding and decoding complex data structures used in the swap execution.

```solidity
// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity =0.7.6;
pragma abicoder v2;
````

***

**🛠️ SwapExamples Contract Structure**

The `SwapExamples` contract is created with an immutable public variable `swapRouter` of type `ISwapRouter`, enabling calls to the router's interface functions.

> Design Consideration: The swap router is passed via the constructor for simplicity in these examples. More advanced contracts should focus on safely inheriting the router.

Solidity

```
contract SwapExamples {
    ISwapRouter public immutable swapRouter;

    // Hardcoded Token Addresses and Pool Fee for this example.
    // In production, contracts can be modified to accept input parameters for greater flexibility per transaction.
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

#### 1️⃣ Exact Input Single Swap (`swapExactInputSingle`)

The `swapExactInputSingle` function executes an exact input swap: trading a fixed amount of one token (`amountIn`) for the maximum possible amount of another token (`amountOut`).

**🔒 Approvals and Transfers**

When swapping via a smart contract, the following steps are crucial for token handling:

1. The calling address must `approve` _this contract_ to spend the input token (`DAI`).
2. This contract then executes a `safeTransferFrom` to pull the tokens from the caller's account.
3. _This contract_ (now holding the tokens) must then `approve` the SwapX router contract to spend the tokens to execute the swap.

**💻 Function: `swapExactInputSingle`**

Solidity

```
    /// @notice Swaps a fixed amount of DAI for the maximum possible amount of WETH9 using the DAI/WETH9 0.3% pool.
    /// @dev The calling address must approve this contract to spend at least `amountIn` of its DAI.
    /// @param amountIn The exact amount of DAI that will be swapped for WETH9.
    /// @return amountOut The amount of WETH9 received.
    function swapExactInputSingle(uint256 amountIn) external returns (uint256 amountOut) {
        // 1. Transfer the specified amount of DAI from msg.sender to this contract.
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountIn);

        // 2. Approve the router to spend the DAI held by this contract.
        TransferHelper.safeApprove(DAI, address(swapRouter), amountIn);

        // 3. Populate ExactInputSingleParams
        // Note: amountOutMinimum is set to 0 (risky in production). sqrtPriceLimitX96 is set to 0 (inactive).
        ISwapRouter.ExactInputSingleParams memory params =
            ISwapRouter.ExactInputSingleParams({
                tokenIn: DAI,
                tokenOut: WETH9,
                fee: poolFee,
                recipient: msg.sender,
                deadline: block.timestamp,
                amountIn: amountIn,
                amountOutMinimum: 0, 
                sqrtPriceLimitX96: 0
            });

        // 4. Executes the swap.
        amountOut = swapRouter.exactInputSingle(params);
    }
```

***

#### 2️⃣ Exact Output Single Swap (`swapExactOutputSingle`)

The `swapExactOutputSingle` function executes an exact output swap: trading the minimum possible amount of an input token (`amountIn`) for a fixed amount of an output token (`amountOut`).

> Note on Input Variance: Since the input amount is variable, the calling address must approve and transfer the maximum amount they are willing to spend (`amountInMaximum`). Any unspent input tokens must be refunded to the caller after the swap.

**💻 Function: `swapExactOutputSingle`**

Solidity

```
    /// @notice Swaps the minimum possible amount of DAI for a fixed amount of WETH9.
    /// @dev Calling address must approve this contract to spend its DAI. Approval for a slightly higher amount than anticipated is recommended.
    /// @param amountOut The exact amount of WETH9 to receive from the swap.
    /// @param amountInMaximum The maximum amount of DAI willing to be spent.
    /// @return amountIn The amount of DAI actually spent in the swap.
    function swapExactOutputSingle(uint256 amountOut, uint256 amountInMaximum) external returns (uint256 amountIn) {
        // 1. Transfer the specified `amountInMaximum` of DAI to this contract.
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountInMaximum);

        // 2. Approve the router to spend the specified `amountInMaximum` of DAI.
        TransferHelper.safeApprove(DAI, address(swapRouter), amountInMaximum);

        // 3. Populate ExactOutputSingleParams
        ISwapRouter.ExactOutputSingleParams memory params =
            ISwapRouter.ExactOutputSingleParams({
                tokenIn: DAI,
                tokenOut: WETH9,
                fee: poolFee,
                recipient: msg.sender,
                deadline: block.timestamp,
                amountOut: amountOut,
                amountInMaximum: amountInMaximum,
                sqrtPriceLimitX96: 0
            });

        // 4. Executes the swap, returning the actual amount of DAI spent (`amountIn`).
        amountIn = swapRouter.exactOutputSingle(params);

        // 5. Refund: If amount spent (`amountIn`) is less than `amountInMaximum`, refund the difference and reset router approval.
        if (amountIn < amountInMaximum) {
            TransferHelper.safeApprove(DAI, address(swapRouter), 0);
            TransferHelper.safeTransfer(DAI, msg.sender, amountInMaximum - amountIn);
        }
    }
```
