# 🌉  RainLink  Bridging

### Prerequisites

***

Before you start, please make sure you have prepared the following:

1. A crypto wallet that supports Ethereum (or other source chains) (such as [MetaMask ](https://metamask.io/), [OKX Wallet ](https://www.okx.com/web3/wallet), [TokenPocket ](https://www.tokenpocket.pro/), [TronLink ](https://www.tronlink.org/), [TokenUp ](https://www.tokenup.org/)…)
2. A certain amount of native assets (such as ETH, BNB, TRX, XOC…) for cross-chain gas fees.
3. [Supported ](https://docs.xone.org/developers/token) cross-chain assets.

### Let’s get started

***

#### 1、Visit RainLink

Go to the [RainLink ](https://rainlink.co/) website.

#### 2、Connect a wallet

On the RainLink website, click the **Connect Wallet** button, select your wallet type and connect.

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Connect.mp4" %}

#### 3、Select the source and target chains

Select the source and target chains you want to cross-chain from the operation bar displayed on the page.

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Select.mp4" %}

* What are the source and target chains?

The source and target chains are the two endpoints of the cross-chain bridge. The source chain is the chain where the assets you want to cross-chain are located, and the target chain is the chain to which you want to transfer the assets.

* How to select the source and target chains?

In the RainLink website operation interface, the supported source and target chains are listed, and you can click the source and target chains to select.

* Which source and target chains are supported?

If you need to know the supported source and target chains, please go [here](https://docs.rainlink.co/docs/network) to check.

#### 4、Enter the cross-chain amount

After completing the above steps, enter the number of assets you need to cross-chain in the input box. At this time, you need to confirm the number of cross-chain assets and the type of cross-chain assets.

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Amount.mp4" %}

* Which assets are supported?

If you need to know the supported assets, please go [here ](https://docs.xone.org/developers/token).

* Is there a limit on the number of cross-chain assets?

There is no limit on the number of cross-chain assets, as long as the basic cross-chain asset accuracy is met. But at this time, you should understand the [cross-chain handling fee](https://docs.rainlink.co/docs/fee) in detail, because this is unavoidable. If you do not meet the basic cross-chain handling fee, this cross-chain transaction will not be able to proceed.

#### 5、Confirm the receiving address

Enter the address where you receive cross-chain assets in the **Receive Address** input box.

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Address.mp4" %}

**Please note:**

1. Before this, your wallet address will be automatically obtained and filled after you successfully link your wallet, but you should confirm the information here again; because once confirmed and authorized successfully, it will be irreversible.
2. The cross-chain addresses can be inconsistent. For example, if the address you successfully linked is A, you have two options: A > A or A > B (here B represents other addresses).

#### 6、Start bridging

Click the “Transfer” button, and the wallet plug-in or operation pop-up window of your terminal will be awakened. Follow the page prompts to confirm the information authorization.

{% embed url="https://rainlink-docs.s3.ap-southeast-1.amazonaws.com/video/Transfer.mp4" %}

{% hint style="danger" %}
NOTE

**Please note:**

1. When you authorize the bridge for the first time, you will be prompted to authorize the bridge asset permissions of the wallet address. This is because you need to obtain authorization for the asset operation of your wallet, because you need to lock the assets of the source chain into the [RainLink contract ](https://docs.xone.org/developers/contracts) during the bridge process. If you are not sure whether the information authorized at this time is risky, you can go to the public contract information page to confirm.
2. After the authorization is successful, you need to confirm the number and type of cross-chain assets again.
{% endhint %}

#### 7、Wait for the cross-chain to complete

After the authorization is completed, the source chain assets will be locked into the [RainLink contract ](https://docs.xone.org/developers/contracts). At this time, you need to wait for the cross-chain to complete. After completion, the cross-chain assets will be received on the target chain.

#### Congratulations on completing a bridge🎉 <a href="#congratulations-on-completing-a-bridge" id="congratulations-on-completing-a-bridge"></a>
