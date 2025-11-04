# 🥭提供流动性-SwapX-V3

#### 💰 提供流动性

流动性是驱动 SwapX 交易的核心机制。通过提供流动性，您将代币对质押到流动性池中，从而促成兑换（Swap），并为该代币对**赚取一部分交易费用**。

***

#### 💧 流动性费用结构

**SwapX-V3 流动性池**代币兑换的默认交易费用为 **0.3%**。此费用将全部按照流动性提供者（LPs）在流动性储备中的贡献比例进行分配。

LPs 在添加流动性时，可以选择以下三种费用等级：

| 费用等级      | 推荐使用场景                                |
| --------- | ------------------------------------- |
| **0.05%** | 稳定币对（例如：USDC/USDT）                    |
| **0.3%**  | **默认** - 标准代币对（例如：ETH/USDC, XOC/USDH） |
| **1%**    | 特色或低相关性代币对                            |

> **💡 最佳实践：** 始终建议在所选代币对中**最受欢迎的费用等级**下提供流动性，以**最大化交易费用收益**。

***

#### ➕ 如何提供流动性

要提供流动性，您需要质押一定数量的**代币对**。

* 两个代币中**最低（以美元计）的价值**将决定您可以提供的最大流动性。
* _示例：_ 在本示例中，我们将使用 **XOC** 和 **USDH** 来演示该过程。

**分步指南**

1. **访问资金池页面**
   * 访问 SwapX 上的 **资金池（Pools）** 页面。
   *   点击右上角连接您的钱包，推荐使用MetaMask。

       <div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure></div>
2.  **开始添加流动性**

    * 点击 **“创建 Create”** 按钮。

    <div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure></div>

**3. 选择代币对 🪙**

* 使用输入框选择您要添加流动性的两个代币。
* _示例：_ 选择 **XOC** 和 **USDT**。

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure></div>

**4. 确认/自定义费用等级 💲**

* 如需自定义费用等级，请选择所需的百分比：**0.05%**、**0.3%** 或 **1%**。

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure></div>

**5. 输入存入金额并设置价格范围 📐**

* 在 **“存入金额（Deposit Amount）”** 下，输入其中一个代币的数量。另一个代币的数量将**自动计算并填充**，以维持当前的市场比例。
*   **设置价格范围：** （针对集中流动性模型）您必须通过 **“设置价格范围（Price Range）”** 自由设置价格区间。

    * **错误提示：** 如果超出您所提供代币的当前价格范围，系统将报错且无法提交。

    <div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure></div>

> **❗ 余额不足：** 如果您其中一个代币余额不足，您将看到 **“余额不足（Insufficient Balance）”** 错误提示。请降低输入金额，或使用 **“MAX”** 按钮填充最大可用值。

**6. 确认创建 ✅**

* 确认所有信息，包括存入金额和价格范围。
* 确认无误后点击 **“创建（Create）”**。

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure></div>

**7. 最终钱包确认 📲**

* 您的钱包将要求您进行最终交易确认。
* 请在钱包中确认您的交易。

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure></div>

**8. 查看您的持仓 📊**

* 交易确认后不久，您将在资金池的 **“我的持仓（My Holdings）”** 页面看到新的流动性状态。
* 点击该状态即可查看其详细信息。

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure></div>
