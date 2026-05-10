# 1. Introduction

PiRC6 moves from specification to **production-ready implementation**. Every PiRC1-5 proposal has a corresponding Soroban smart contract written in Rust, with full test coverage, deployment automation, and formal verification.

## 1.1 Why Rust + Soroban

| Platform | Assessment | Verdict |
|---|---|---|
| **Python** | Great for tooling, not for on-chain execution | ❌ Cannot run on Pi blockchain |
| **Solidity** | Ethereum's language, not compatible with Stellar | ❌ Pi uses Soroban, not EVM |
| **Rust + Soroban** | Pi Network's official smart contract platform | ✅ Native, production-ready, gas-efficient |

Pi Network's blockchain is built on **Stellar** with the **Soroban** smart contract runtime. Writing contracts in anything else is theoretical — Rust/Soroban is what actually runs on Pi.

## 1.2 Contract Map

| Contract | Implements | Soroban Module |
|---|---|---|
| **zyn_token** | PiRC1 (Token Design) | SPL-style token with burn, staking, governance |
| **zyn_subscription** | PiRC2 (Subscriptions) | Recurring payments with grace, disputes, tiers |
| **zyn_marketplace** | PiRC3 (Marketplace) | Escrow, listings, delivery proof, jury disputes |
| **zyn_trust** | PiRC4 (Reputation) | Trust score calculation, cross-app portability |
| **zyn_wallet** | PiRC5 (Wallet Interop) | Cross-app allowances, balance sync, atomic transfers |

## 1.3 Cross-Contract Architecture

```
┌─────────────────────────────────────────────────────┐
│                  SOROBAN RUNTIME                      │
│                                                       │
│  ┌───────────┐  ┌───────────┐  ┌──────────────────┐ │
│  │ zyn_token │◄─┤ zyn_wallet│──┤ zyn_subscription │ │
│  │ (burn,    │  │ (allowance│  │ (grace, dispute, │ │
│  │  stake,   │  │  cross-app│  │  tiered pricing) │ │
│  │  govern)  │  │  transfer)│  └──────────────────┘ │
│  └─────┬─────┘  └─────┬─────┘                       │
│        │               │                             │
│        │    ┌───────────▼───────────┐                 │
│        │    │    zyn_marketplace    │                 │
│        │    │  (escrow, listing,    │                 │
│        │    │   delivery, jury)     │                 │
│        │    └───────────┬───────────┘                 │
│        │                │                             │
│        │    ┌───────────▼───────────┐                 │
│        └───►│      zyn_trust         │                │
│             │  (score, portability,  │                │
│             │   anti-manipulation)   │                │
│             └───────────────────────┘                │
└─────────────────────────────────────────────────────┘
```

## 1.4 Development Principles

- **Test-First**: Every contract function has unit tests + integration tests + scenario tests
- **Formal Verify Critical Paths**: Escrow release, burn mechanism, dispute resolution
- **Gas-Efficient**: Use Soroban's `persistent` vs `temporary` storage strategically
- **Upgradeable**: All contracts implement `upgrade()` via admin key
- **Event-Rich**: Every state change emits events for off-chain sync
- **Minimal Trust**: No admin override for user funds — admin can only upgrade code

Next: [`2-Contract-Architecture`](2-contract-architecture.md)
