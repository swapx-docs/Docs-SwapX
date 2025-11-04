# 🪂多跳交换

````
### 🔗 多跳交换 (Multi-Hop Swaps)

以下示例是 v3 上可用的两种多跳交换样式的实现。**注意：** 以下示例并非可用于生产的代码，而是以简单方式实现，仅用于**学习目的**。

#### 📜 合约设置与先决条件

我们声明将用于编译合约的 Solidity 版本，并启用 `abi-coder v2`，以便在 `calldata` 中对任意嵌套数组和结构进行编码和解码，这是我们在执行多跳交换时使用的功能。

```solidity
// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity =0.7.6;
pragma abicoder v2;
````

***

**🛠️ SwapExamples 合约**

我们创建一个名为 `SwapExamples` 的合约，并声明一个类型为 `ISwapRouter` 的不可变公共变量 `swapRouter`。这使我们能够调用 `ISwapRouter` 接口中的函数。

> 设计考虑： 为简化起见，在此示例中我们将交换路由器作为构造函数参数传入。更高级的示例合约将详细说明如何安全地继承交换路由器。

Solidity

```
contract SwapExamples {
    ISwapRouter public immutable swapRouter;

    // 为本示例硬编码代币合约地址和池费用等级。
    // 在生产环境中，您通常会使用输入参数来提高灵活性。
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

#### 1️⃣ 精确输入多跳 (Exact Input Multihop)

精确输入多跳交换将在给定输入代币上交换固定数量，以获得给定输出的最大可能数量，并且可以包含任意数量的中间交换。

在此示例中，我们执行 DAI → USDC → WETH9 的交换。

**⚙️ 关键输入**

| **参数**             | **描述**                                                           |
| ------------------ | ---------------------------------------------------------------- |
| `path`             | 一个 `(tokenAddress, fee, tokenAddress, ...)` 序列，计算交换序列中每个池合约地址所需。 |
| `recipient`        | 出站资产 (WETH9) 的目标地址。                                              |
| `deadline`         | 交易将被撤销的 Unix 时间，以防价格大幅波动。                                        |
| `amountIn`         | 入站资产 (DAI) 的固定数量。                                                |
| `amountOutMinimum` | 出站资产的最小可接受数量。本简化示例中设置为 `0`。                                      |

**💻 函数: `swapExactInputMultihop`**

Solidity

```
    /// @notice 通过中间池（DAI -> USDC -> WETH9）交换固定数量的 DAI，以获得最大可能数量的 WETH9。
    /// @dev 调用地址必须授权此合约至少花费其 `amountIn` 数量的 DAI。
    /// @param amountIn 要交换的 DAI 数量。
    /// @return amountOut 交换后收到的 WETH9 数量。
    function swapExactInputMultihop(uint256 amountIn) external returns (uint256 amountOut) {
        // 1. 将 `amountIn` 数量的 DAI 从 msg.sender 转移到此合约。
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountIn);

        // 2. 授权路由器花费 DAI。
        TransferHelper.safeApprove(DAI, address(swapRouter), amountIn);

        // 3. 路径编码：(tokenIn, fee, sharedToken, fee, tokenOut)
        // 路径: (DAI, 0.3%, USDC, 0.3%, WETH9)
        ISwapRouter.ExactInputParams memory params =
            ISwapRouter.ExactInputParams({
                path: abi.encodePacked(DAI, poolFee, USDC, poolFee, WETH9),
                recipient: msg.sender,
                deadline: block.timestamp,
                amountIn: amountIn,
                amountOutMinimum: 0 // 生产环境中应使用 SDK 或链上预言机
            });

        // 4. 执行交换。
        amountOut = swapRouter.exactInput(params);
    }
```

***

#### 2️⃣ 精确输出多跳 (Exact Output Multihop)

精确输出多跳交换将用可变数量的输入代币交换固定数量的出站代币。这是多跳交换中不太常见的技术。

在此示例中，我们旨在通过 DAI → USDC → WETH9 交换来接收固定数量的 WETH9。

**⚙️ 关键输入**

| **参数**            | **描述**                                                     |
| ----------------- | ---------------------------------------------------------- |
| `path`            | 一个 `(tokenAddress, fee, tokenAddress, ...)` 序列，按交换的反向顺序编码。 |
| `recipient`       | 出站资产 (WETH9) 的目标地址。                                        |
| `deadline`        | 交易将被撤销的 Unix 时间，以防价格大幅波动。                                  |
| `amountOut`       | 所需的固定数量的出站资产 (WETH9)。                                      |
| `amountInMaximum` | 愿意为指定数量 WETH9 交换的 DAI 的最大数量。                               |

**⚠️ 路径反向编码**

对于精确输出交换，路径是反向编码的，因为为了达到最终所需的输出，交换是按照路径的相反顺序执行的。

| **参数** | **编码顺序**                     | **代币序列**           |
| ------ | ---------------------------- | ------------------ |
| 精确输入   | `(输入代币, 费用, 共享代币, 费用, 输出代币)` | DAI → USDC → WETH9 |
| 精确输出   | `(输出代币, 费用, 共享代币, 费用, 输入代币)` | WETH9 → USDC → DAI |

**💻 函数: `swapExactOutputMultihop`**

Solidity

```
    /// @notice 通过中间池交换最小可能数量的 DAI，以获得固定数量的 WETH9（DAI -> USDC -> WETH9）。
    /// @dev 调用地址必须授权此合约花费至少 `amountInMaximum` 数量的 DAI。
    /// @param amountOut 所需的 WETH9 数量。
    /// @param amountInMaximum 愿意为指定数量 WETH9 交换的最大 DAI 数量。
    /// @return amountIn 实际花费的 DAI 数量。
    function swapExactOutputMultihop(uint256 amountOut, uint256 amountInMaximum) external returns (uint256 amountIn) {
        // 1. 将指定的 `amountInMaximum` 转移到此合约。
        TransferHelper.safeTransferFrom(DAI, msg.sender, address(this), amountInMaximum);
        
        // 2. 授权路由器花费 `amountInMaximum`。
        TransferHelper.safeApprove(DAI, address(swapRouter), amountInMaximum);

        // 3. 路径编码（反向）：(tokenOut, fee, sharedToken, fee, tokenIn)
        // 路径: (WETH9, 0.3%, USDC, 0.3%, DAI)
        ISwapRouter.ExactOutputParams memory params =
            ISwapRouter.ExactOutputParams({
                path: abi.encodePacked(WETH9, poolFee, USDC, poolFee, DAI),
                recipient: msg.sender,
                deadline: block.timestamp,
                amountOut: amountOut,
                amountInMaximum: amountInMaximum
            });

        // 4. 执行交换，返回实际花费的 DAI 数量 (`amountIn`)。
        amountIn = swapRouter.exactOutput(params);

        // 5. 退款：如果未花费全部 `amountInMaximum` 来获得精确的 `amountOut`，则退还差额并重置授权。
        if (amountIn < amountInMaximum) {
            TransferHelper.safeApprove(DAI, address(swapRouter), 0);
            TransferHelper.safeTransferFrom(DAI, address(this), msg.sender, amountInMaximum - amountIn);
        }
    }
```
