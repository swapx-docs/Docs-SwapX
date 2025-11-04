# ⚙️ SwapX V2 Smart Contracts

## ⚙️ SwapX V2 Smart Contracts

SwapX V2 operates as a **binary smart contract system**. The **Core** contracts provide fundamental security guarantees, while the **Periphery** contracts facilitate user-friendly interaction.

### 🧠 Core

The Core is intentionally designed to be minimal—or even **"brutalist"**—to ensure maximum security and elegance. This simplicity:

* Makes the contracts easier to reason about and less error-prone.
* Allows key system properties to be directly declared in the code, leaving little room for error.

> **⚠️ Note:** The Core contracts are somewhat user-unfriendly. It is generally **not recommended** to interact with them directly. Instead, users should utilize the Periphery contracts.

#### Factory

The `Factory` is a **singleton** contract responsible for:

* Holding the generic bytecode that powers all trading pairs.
* Creating and indexing one unique smart contract for every unique token pair.
* Containing the logic to activate the protocol-wide fee.

#### Pairs

A `Pair` contract serves three primary functions:

1. **Automated Market Maker (AMM):** Executes trades based on the constant product formula.
2. **Token Balance Tracking:** Maintains and tracks the pool token balances.
3. **Price Oracle Data:** Exposes data that can be used to construct decentralized price oracles.

### 📦 Periphery

The Periphery is a collection of smart contracts built to support **domain-specific interactions** with the Core. Due to SwapX's permissionless nature, the contracts described here are just a small fraction of what's possible, serving as helpful examples for safe and effective interaction with SwapX V2.

#### Library

The `Library` provides a collection of utility functions, primarily for **fetching data and calculating prices**.

#### Router

The `Router`, which utilizes the `Library`, supports all essential requirements for a frontend to provide trading and liquidity management functionality.

* **Native Support** for multi-pair trades (e.g., `Token X` to `Y` to `Z`).
* Provides **meta-transactions** for liquidity removal.

### 💡 Design Decisions

This section details key design choices made within SwapX V2. You can safely skip this section unless you are interested in V2's deep inner workings or are writing a smart contract integration.

#### Sending Tokens

V2 pairs accept tokens via a unique balance-checking mechanism, unlike typical contracts requiring `approve`/`transferFrom`:

* The pair checks its token balance at the **end** of every interaction.
* At the **start** of the next interaction, the difference between the current and stored balance determines the amount of tokens sent.
* **Crucially:** Tokens **must be transferred** to the pair _before_ calling any method that requires them. (Refer to the Whitepaper for a detailed explanation.)

#### WXOC

SwapX V2 pairs do not natively support **ETH** (Ethereum's native coin). `ETH⇄ERC-20` pairs must be simulated using **WXOC** (Wrapped XOC).

* **Motivation:** To remove XOC-specific code from the Core, resulting in a cleaner codebase.
* **User Experience:** End-users can remain entirely unaware, as the Periphery handles the wrapping/unwrapping of XOC.
* **Router Support:** The Router fully supports interacting with any WXOC pair using XOC.

#### Minimum Liquidity

To mitigate rounding errors and boost the theoretical minimum quote unit for liquidity provision, pairs **burn** the first `MINIMUM_LIQUIDITY` pool tokens.

* This amount is **negligible** for the vast majority of pairs.
* The burn occurs automatically during the **first liquidity supply**, permanently capping the total supply thereafter.
