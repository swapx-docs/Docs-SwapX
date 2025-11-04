# 🏂单笔交换

````
### 🔄 单笔交换 (Single-Hop Swaps)

交换是与 SwapX 协议最常见的交互。以下示例展示了如何使用 `swapExactInputSingle` 和 `swapExactOutputSingle` 两个函数实现单路径交换。

> **重要提示：** 交换示例**不是可用于生产的代码**，而是以简单方式实现，仅用于**学习目的**。通过智能合约进行交易时，最重要的是需要访问外部价格来源（预言机），否则交易可能会因执行价格不佳而遭受重大损失。

#### 📜 合约设置与先决条件

我们声明用于编译合约所需的 Solidity 版本，并启用 `abi-coder v2`，这是用于执行交换时对复杂数据结构进行编码和解码所必需的功能。

```solidity
// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity =0.7.6;
pragma abicoder v2;
````

***

**🛠️ SwapExamples 合约结构**

我们创建 `SwapExamples` 合约，并声明一个类型为 `ISwapRouter` 的不可变公共变量 `swapRouter`，以便调用路由器的接口函数。

> 设计考虑： 为简化起见，在此示例中我们将交换路由器作为构造函数参数传入。更高级的合约应侧重于安全地继承路由器。

Solidity

```
contract SwapExamples {
    ISwapRouter public immutable swapRouter;

    // 为本示例硬编码代币合约地址和池费用等级。
    // 在生产环境中，合约可以修改为接受输入参数，以实现每笔交易的更大灵活性。
    address public constant DAI = 0x6B175474E89094C44Da98b954EedeAC495271d0F;
    address public constant WETH9 = 0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2;
    address public constant USDC = 0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48;

    // 本示例中，我们将池费用设置为 0.3%。
    uint24 public constant poolFee = 3000;

    constructor(ISwapRouter _swapRouter) {
        swapRouter = _swapRouter;
    }
    // ... 交换函数紧随其后
}
```

***

#### 1️⃣ 精确输入单笔交换 (`swapExactInputSingle`)

`swapExactInputSingle` 函数用于执行精确输入交换，即将一种代币的固定数量 (`amountIn`) 交换为另一种代币的最大可能数量 (`amountOut`)。

**🔒 批准与转移流程**

通过智能合约进行交换时，代币处理至关重要：

1. 调用地址必须 `approve` _此合&#x7EA6;_&#x6765;花费输入代币（`DAI`）。
2. 此合约执行 `safeTransferFrom` 将代币从调用者账户提取。
3. _此合约_（现在持有代币）必须 `approve` SwapX 协议路由器合约来使用这些代币执行交换。

**💻 函数: `swapExactInputSingle`**

Solidity

```
    /// @notice 通过调用交换路由器中的 `exactInputSingle`，使用 DAI/WETH9 0.3% 池将固定数量的 DAI 交换为最大可能数量的 WETH9。
    /// @dev 调用地址必须批准此合约花费其至少 `amountIn` 数量的 DAI。
    /// @param amountIn 将被交换的 DAI 的精确数量。
    /// @return amountOut 收到的 WETH9 数量。
    function swapExactInputSingle(uint256 amountIn) external returns (uint256 amountOut) {
        // 1. 将指定数量的 DAI 从 msg.sender 转移到此合约。
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountIn);

        // 2. 授权路由器花费此合约持有的 DAI。
        TransferHelper.safeApprove(DAI, address(swapRouter), amountIn);

        // 3. 填充 ExactInputSingleParams
        // 注意：amountOutMinimum 设置为 0（在生产中存在重大风险）。sqrtPriceLimitX96 设置为 0（不活动）。
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

        // 4. 执行交换。
        amountOut = swapRouter.exactInputSingle(params);
    }
```

***

#### 2️⃣ 精确输出单笔交换 (`swapExactOutputSingle`)

`swapExactOutputSingle` 函数用于执行精确输出交换，即将输入代币的最小可能数量 (`amountIn`) 交换为输出代币的固定数量 (`amountOut`)。

> 关于输入可变性的说明： 由于输入数量是可变的，调用地址必须批准并转移他们愿意花费的最大数量 (`amountInMaximum`)。交换结束后，任何未花费的输入代币都必须退还给调用者。

**💻 函数: `swapExactOutputSingle`**

Solidity

```
    /// @notice 交换最小可能数量的 DAI，以获得固定数量的 WETH9。
    /// @dev 调用地址必须批准此合约花费其 DAI。建议批准一个略高于预期数量的值。
    /// @param amountOut 期望从交换中获得的 WETH9 的精确数量。
    /// @param amountInMaximum 愿意为指定数量的 WETH9 花费的最大 DAI 数量。
    /// @return amountIn 交换中实际花费的 DAI 数量。
    function swapExactOutputSingle(uint256 amountOut, uint256 amountInMaximum) external returns (uint256 amountIn) {
        // 1. 将指定数量的 `amountInMaximum` 的 DAI 转移到此合约。
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountInMaximum);

        // 2. 授权路由器花费指定的 `amountInMaximum` 的 DAI。
        TransferHelper.safeApprove(DAI, address(swapRouter), amountInMaximum);

        // 3. 填充 ExactOutputSingleParams
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

        // 4. 执行交换，返回为获得所需 amountOut 而实际花费的 DAI 数量 (`amountIn`)。
        amountIn = swapRouter.exactOutputSingle(params);

        // 5. 退款：如果实际花费的数量 (`amountIn`) 小于 `amountInMaximum`，则退还差额并重置路由器授权。
        if (amountIn < amountInMaximum) {
            TransferHelper.safeApprove(DAI, address(swapRouter), 0);
            TransferHelper.safeTransfer(DAI, msg.sender, amountInMaximum - amountIn);
        }
    }
```
