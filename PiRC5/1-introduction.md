# 1. Introduction

The Cross-App Wallet Interoperability Protocol solves one of the biggest friction points in the Pi ecosystem: **wallet isolation**. Users who earn ZYN in one app cannot easily use it in another. PiRC5 creates a unified wallet layer that makes ZYN truly portable.

## 1.1 Core Principles

- **One Balance, Many Apps**: A user's ZYN balance is stored on-chain and accessible from any integrated app
- **Instant Transfers**: Moving ZYN between apps is as simple as switching tabs — no withdrawal/deposit cycle
- **Transaction Continuity**: Full transaction history follows the user across apps
- **Security First**: Dual-layer security (on-chain + off-chain cache) with on-chain as source of truth
- **Privacy Preserved**: Apps only see what they need — no unnecessary data sharing

## 1.2 How It Works

```
┌──────────┐  ┌──────────┐  ┌──────────┐
│ ZynMart  │  │ App B    │  │ App C    │
│ (Wallet) │  │ (Wallet) │  │ (Wallet) │
└────┬─────┘  └────┬─────┘  └────┬─────┘
     │             │             │
     │    ┌────────▼────────┐    │
     └───►│  ZYN Token      │◄───┘
          │  Contract       │
          │  (Soroban)      │
          └────────┬────────┘
                   │
          ┌────────▼────────┐
          │  Sync Layer     │
          │  (Firestore +   │
          │   Chain Cache)  │
          └─────────────────┘
```

1. **On-Chain Layer**: Soroban smart contract holds the canonical ZYN balance
2. **Sync Layer**: Firestore caches balances for real-time UI updates
3. **App Layer**: Each app reads from sync layer, writes through smart contract

## 1.3 Benefits

- **For Users**: No more withdrawing from one app to deposit in another
- **For Apps**: Access to a larger user base with existing ZYN balances
- **For Ecosystem**: Network effects — more apps = more ZYN utility = more value
- **For Merchants**: Accept ZYN from any app's user base, not just their own

Next: [`2-Architecture`](2-architecture.md)
