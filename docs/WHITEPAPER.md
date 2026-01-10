# Bethna AI Trader: Whitepaper v1.1

## **Abstract**
DeFi options markets offer high yields and institutional-grade hedging capabilities but suffer from extreme complexity. Retail users struggle with strike selection, greek management, and expiry timings. **Bethna AI Trader** democratizes this by introducing "Sentient Agents"—autonomous AI entities capable of analyzing market structure and executing non-custodial trades via the **SentientTrader** protocol on Thetanuts Finance.

---

## **1. The Problem Space**
While Decentralized Finance (DeFi) has matured, the **Derivatives** sector remains inaccessible to 99% of users.
*   **Cognitive Overload**: Understanding "Delta", "Theta", and "Implied Volatility" is a high barrier to entry.
*   **Execution Risk**: Manual trading on AMMs often leads to high slippage and front-running.
*   **Time Intensity**: optimal options strategies require 24/7 monitoring, which lies beyond human capacity.

## **2. The Bethna Solution**
Bethna acts as an **Intelligence Layer** on top of Thetanuts Finance. It replaces manual vault management with **Active AI Management**.

### **2.1 The Sentient Agents Architecture**
Bethna decomposes the complex trading lifecycle into distinct roles handled by specialized AI agents working in concert:

#### **Agent Alpha (The Analyst)**
*   **Core Function**: Market perception and signal generation.
*   **Inputs**: Aggregates real-time price feeds (Pyth), on-chain volume data, funding rates, and social sentiment analysis.
*   **Output**: Semantic signals (e.g., *"Bullish Divergence on ETH, Volatility compressing - Recommend Long Call"*).

#### **Agent Beta (The Executor)**
*   **Core Function**: Trade execution and slippage verification.
*   **Mechanism**: Listens for validated signals from Agent Alpha. It interacts directly with the `SentientTrader` smart contract.
*   **Safety**: verifies liquidity depth before execution and enforces strict slippage parameters to protect user capital.

#### **Agent Gamma (The Risk Manager)**
*   **Core Function**: Portfolio health and exit discipline.
*   **Logic**: Monitors open positions for **Theta Decay** (time value loss). If a position becomes unprofitable or reaches a profit target, Agent Gamma automatically triggers a `closePosition` transaction to lock in results.

#### **Agent Delta (The Hedger)**
*   **Core Function**: Neutralizing market direction risk.
*   **Strategy**: Executes delta-neutral strategies (e.g., Long Call + Short Perp) to harvest volatility without directional exposure. *(Coming in Phase 2)*

---

## **3. Technical Architecture**

### **3.1 The SentientTrader Protocol**
The on-chain backbone is the `SentientTrader.sol` smart contract. It acts as a secure, non-custodial proxy between the user and the Thetanuts V4 Router.

*   **Non-Custodial Design**: The contract executes swaps using standard ERC-20 allowances. It **never** takes custody of user funds for longer than the atomic transaction. All Option Tokens are returned immediately to the user.
*   **Access Control**: Utilizes a strict `onlyAuthorized` modifier, ensuring that only verified Bethna AI Agents can trigger trade functions. This prevents malicious third-party execution.
*   **Slippage Protection**: Enforces a hard-coded maximum slippage tolerance (`MAX_SLIPPAGE_BPS = 10%`), protecting users from volatile market swings during execution.
*   **Emergency Pause**: Includes OpenZeppelin `Pausable` functionality, allowing the protocol DAO to freeze operations instantly in the event of an upstream vulnerability.

### **3.2 The Application Layer**
*   **Performance Engine**: Built on **Next.js 15 (Turbo)** for instant page loads.
*   **Web3 Optimization**: Implements **Lazy Loading** for wallet providers (`Web3SmartProvider`), ensuring the UI is interactive immediately, even before the wallet connects.
*   **Visual Design**: The **"Bespoke Radiant"** design system uses GPU-accelerated animations and glassmorphism to present complex financial data in a simple, intuitive dashboard.

---

## **4. Tokenomics: $BETH**
The **Bethna Governance Token ($BETH)** is designed to align the incentives of users, the DAO, and the AI protocol.

| Utility | Description |
| :--- | :--- |
| **Governance** | Holders vote on new Agent strategies (e.g., "Add BTC Straddle Strategy") and risk parameters. |
| **Fee Discount** | Staking $BETH reduces the performance fee charged on profitable AI trades (Standard: 20% -> Staked: 10%). |
| **Pro Access** | Unlocks access to **Agent Delta** (Hedging) and **Agent Gamma** (Advanced Risk Management) for sophisticated users. |
| **Revenue Share** | A portion of protocol revenue is used to buy back and burn $BETH, creating deflationary pressure. |

---

## **5. Roadmap**

### **Phase 1: Genesis (Q1 2026)**
*   [x] **Protocol Launch**: Deployment of `SentientTrader.sol` on Base Mainnet.
*   [x] **Core Agents**: Implementation of Agent Alpha (Analyst) and Agent Beta (Executor).
*   [x] **Dashboard V1**: Launch of the "Bespoke Radiant" user interface.

### **Phase 2: Decentralization (Q3 2026)**
*   [ ] **Account Abstraction**: Implementation of **Session Keys (EIP-4337)**. This removes the need for "Authorized Trader" whitelisting, allowing users to granularly approve Agent actions for a set time window.
*   [ ] **Strategy Vaults**: Public vaults where users can deposit USDC for pooled AI management, reducing gas costs for smaller portfolios.

### **Phase 3: The Sentient Network (2027)**
*   [ ] **Agent Marketplace**: A platform for community developers to train and upload their own trading agents (e.g., "Momentum Bot", "Mean Reversion Bot").
*   [ ] **Cross-Chain Expansion**: Deploy SentientTrader to Arbitrum and Optimism to capture liquidity across the Superchain.
