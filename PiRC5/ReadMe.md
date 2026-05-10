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

| Problem | Current State | PiRC5 Solution |
|---|---|---|
| **Isolated Balances** | Each app has its own ZYN balance | Unified on-chain balance accessible from any app |
| **No Cross-App Payments** | Can't pay in App B with ZYN earned in App A | Seamless cross-app token transfers |
| **Duplicate KYC** | Each app verifies identity separately | Shared identity verification via Pi SDK |
| **No Transaction Portability** | Transaction history locked per app | Unified transaction ledger across apps |
| **Manual Withdrawals** | Must withdraw from one app to use in another | Direct inter-app transfers via smart contract |
