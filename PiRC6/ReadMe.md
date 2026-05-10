# PiRC6: Smart Contract Reference Implementation (ZynMart Request for Comment)

This document specifies the **Reference Implementation** for all ZynMart PiRC smart contracts on **Soroban (Stellar)** — Pi Network's actual smart contract platform. Unlike theoretical architectures, PiRC6 provides **production-ready Rust code**, deployment pipelines, and testing frameworks.

Community feedback is welcome.

## Table of Contents

- [`1-introduction`](1-introduction.md) **(start here)**
- [`2-contract-architecture`](2-contract-architecture.md)
- [`3-zyn-token-contract`](3-zyn-token-contract.md)
- [`4-subscription-contract`](4-subscription-contract.md)
- [`5-marketplace-contract`](5-marketplace-contract.md)
- [`6-trust-score-contract`](6-trust-score-contract.md)
- [`7-wallet-interop-contract`](7-wallet-interop-contract.md)
- [`8-deployment-pipeline`](8-deployment-pipeline.md)
- [`9-testing-framework`](9-testing-framework.md)

---

## What PiRC6 Adds to the Pi Ecosystem

*Moving from specification to implementation — production-ready Soroban contracts for every PiRC proposal.*

| Innovation | Description |
|---|---|
| **Soroban-Native Contracts** | All contracts written in Rust for Soroban — Pi's actual execution environment, not Solidity or Python |
| **Modular Contract System** | Each PiRC proposal is an independent contract that composes with others — upgradeable, testable, deployable |
| **Cross-Contract Invocation** | Contracts call each other via Soroban's `invoke()` — subscription contract checks trust score, marketplace uses escrow |
| **CI/CD Pipeline** | Automated build, test, audit, and deploy to Pi Testnet/Mainnet — every commit is verified |
| **Formal Verification** | Critical paths (escrow, burn, dispute) verified with mathematical proofs — no exploits |
| **Gas Optimization** | Soroban-specific optimizations: persistent vs temporary storage, ledger TTL management, batch operations |
| **Upgrade Mechanism** | All contracts support `upgrade()` via admin — fix bugs without redeploying the entire system |
| **Event-Driven Architecture** | Every state change emits events — backend syncs Firestore cache from on-chain events |
