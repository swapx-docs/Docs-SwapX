# 🥭SwapX-V3 Liquidity Pool

#### 💰 Providing Liquidity

Liquidity is the core mechanism that drives all trading on SwapX. By providing liquidity, you stake a token pair into a liquidity pool, which facilitates token swaps and allows you to **earn a portion of the trading fees** for that pair.

***

#### 💧 Liquidity Fee Structure

The default trading fee for token swaps within the **SwapX-V3 Liquidity Pool Protocol** is **0.3%**. This fee is entirely distributed to the Liquidity Providers (LPs) based on their contribution to the liquidity reserves.

LPs can choose from the following three fee tiers when adding liquidity:

| Fee Tier  | Use Case Recommendation                                 |
| --------- | ------------------------------------------------------- |
| **0.05%** | Stablecoin pairs (e.g., USDC/USDT)                      |
| **0.3%**  | **Default** - Standard pairs (e.g., ETH/USDC, XOC/USDH) |
| **1%**    | Exotic or non-correlated pairs                          |

> **💡 Best Practice:** It is always recommended to provide liquidity in the most popular fee tier for the chosen token pair to **maximize trading fee returns**.

***

#### ➕ How to Provide Liquidity

To provide liquidity, you need to stake a certain amount of a token pair.

* The **minimum (USD value) of the two tokens** will determine the maximum liquidity you can provide.
* _Example:_ We will use **XOC** and **USDH** to demonstrate the process.

**Step-by-Step Guide**

**1. Access the Pools Page 🌊**

* Navigate to the **Pools** page on SwapX.
* Click the **"Connect Wallet"** button in the top right corner.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure></div>

**2. Start Adding Liquidity 🛠️**

* Click the **"Create"** button (or "Add Liquidity") to start the process.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure></div>

**3. Select the Token Pair 🪙**

* Use the input fields to select the two tokens for which you wish to provide liquidity.
* _Example:_ Select **XOC** and **USDT**.

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

**4. Confirm/Customize Fee Tier 💲**

* If needed, select the desired fee percentage from the available options: **0.05%**, **0.3%**, or **1%**.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure></div>

**5. Enter Deposit Amounts and Set Price Range 📐**

* Under **"Deposit Amount,"** enter the quantity for one of the tokens. The amount for the other token will be **automatically calculated** to maintain the current market ratio.
*   **Set Price Range:** For the concentrated liquidity model (SwapX-V3), you must set a **"Price Range."** This range defines the specific bounds within which your liquidity is active.

    * **Error Check:** If the current market price of the pair falls outside the range you set, the system will prevent submission and display an error.

    <div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure></div>

> **❗ Insufficient Balance:** If you have an insufficient balance of one token, you will see an **"Insufficient Balance"** error. Reduce the input amount or use the **"MAX"** button to populate the maximum available value.

**6. Confirm Creation ✅**

* Review all the entered information, including deposit amounts and the price range.
* Once verified, click the **"Create"** button.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure></div>

**7. Final Wallet Confirmation 📲**

* Your wallet will prompt you for a final transaction confirmation.
* Please confirm the transaction within your wallet interface.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure></div>

**8. View Your Holdings 📊**

* Shortly after the transaction is confirmed, you will see your new liquidity position on the **"My Holdings"** tab of the Pools page.
* Click on the position entry to view its detailed information.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure></div>
