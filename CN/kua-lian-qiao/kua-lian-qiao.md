# 🌉  跨链桥

### 准备工作

在您开始之前，请确保您已准备好以下物品：

* 一个支持以太坊（或其他源链）的加密钱包（例如 MetaMask 、OKX Wallet 、TokenPocket 、TronLink 、TokenUp …）
* 一定数量的原生资产（例如 ETH, BNB, TRX, XOC…）用于支付跨链 Gas 费。
* 支持的  跨链资产。

### 让我们开始吧

### 1、访问 RainLink

前往 [RainLink  ](https://rainlink.co/)网站。

### 2、连接钱包

在 [RainLink](https://rainlink.co/) 网站上，点击“连接钱包”按钮，选择您的钱包类型并连接。

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Connect.mp4" %}

### 3、选择源链和目标链

从页面显示的操作栏中选择您想要跨链的源链和目标链。

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Select.mp4" %}

> **什么是源链和目标链？** 源链和目标链是跨链桥的两个端点。源链是您要跨链的资产所在的链，目标链是您要将资产转移到的链。
>
> **如何选择源链和目标链？** 在 RainLink 网站操作界面中，会列出支持的源链和目标链，您可以点击源链和目标链进行选择。
>
> **支持哪些源链和目标链？** 如果您需要了解支持的源链和目标链，请到 这里 查看。

### 4、输入跨链金额

完成上述步骤后，在输入框中输入您需要跨链的资产数量。此时，您需要确认跨链资产的数量和跨链资产的类型。

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Amount.mp4" %}

支持哪些资产？ 如果您需要了解支持的资产，请到 这里  查看。

跨链资产的数量有限制吗？ 跨链资产数量没有限制，只要满足基本的跨链资产精度即可。但此时，您应该详细了解 跨链手续费 ，因为这是不可避免的。如果您不满足基本的跨链手续费，这笔跨链交易将无法进行。

### 5、确认接收地址

在“接收地址”输入框中输入您接收跨链资产的地址。

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Address.mp4" %}

> 请注意：
>
> * 在此之前，您成功链接钱包后，您的钱包地址将被自动获取并填写，但您应在此处再次确认信息；因为一旦确认并授权成功，将不可逆转。
> * 跨链地址可以不一致。例如，如果您成功链接的地址是 A，您有两种选择：A > A 或 A > B（这里 B 代表其他地址）。

6、开始跨链

点击“Transfer”（转账）按钮，您终端的钱包插件或操作弹窗将被唤醒。按照页面提示确认信息授权。

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Transfer.mp4" %}

> <mark style="color:$primary;">请注意：</mark>
>
> * 当您第一次授权跨链时，系统会提示您授权钱包地址的跨链资产权限。这是因为需要获取您钱包的资产操作授权，因为在跨链过程中需要将源链的资产锁定到 RainLink 合约  中。如果您不确定此时授权的信息是否有风险，可以到公开的合约信息页面进行确认。
> * 授权成功后，您需要再次确认跨链资产的数量和类型。

### 7、等待跨链完成

授权完成后，源链资产将被锁定到 RainLink 合约  中。此时，您需要等待跨链完成。完成后，跨链资产将在目标链上收到。

恭喜您完成了一次跨链🎉
