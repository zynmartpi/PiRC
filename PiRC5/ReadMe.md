# PiRC5: Cross-App Wallet Interoperability Protocol (ZynMart Request for Comment)

This document specifies the **Cross-App Wallet Interoperability Protocol** — a standard for seamless token and data transfer between Pi ecosystem applications. Currently, each Pi app maintains isolated wallet balances. PiRC5 enables users to carry their ZYN balance, transaction history, and trust score across all integrated apps.

Community feedback is welcome.

## Table of Contents

- [`1-introduction`](1-introduction.md) **(start here)**
- [`2-architecture`](2-architecture.md)
- [`3-balance-sync`](3-balance-sync.md)
- [`4-transaction-protocol`](4-transaction-protocol.md)
- [`5-security-model`](5-security-model.md)
- [`6-app-integration-guide`](6-app-integration-guide.md)
- [`7-data-types`](7-data-types.md)

---

## Why PiRC5 Is Needed

*Building on the ecosystem infrastructure established by [PiNetwork/PiRC](https://github.com/PiNetwork/PiRC) to enable seamless cross-app interoperability.*

### What PiRC5 Adds

| Innovation | Description |
|---|---|
| **Unified On-Chain Balance** | One ZYN balance accessible from any integrated app — no more isolated wallets |
| **Cross-App Payments** | Pay in App B with ZYN earned in App A — seamless token transfers via smart contract |
| **Dual-Layer Sync** | On-chain (Soroban) for authority + Firestore cache for real-time UI — best of both worlds |
| **App Allowance System** | Users approve per-app spending limits — full control, no app can drain balance |
| **Atomic Transactions** | Cross-app transfers are all-or-nothing — either fully complete or fully rolled back |
| **Shared Identity** | Pi SDK authentication works across all apps — no duplicate verification needed |
| **Unified Transaction Ledger** | Full history follows the user across apps — transparent and portable |
| **Integration Tiers** | Accept → Reward → Govern → Launch — progressive app integration with stake requirements |
| **Security Model** | Rate limits, emergency freeze, anomaly detection — user funds always protected |
