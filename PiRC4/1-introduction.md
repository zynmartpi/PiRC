# 1. Introduction

The Reputation & Trust Score System provides a decentralized, transparent, and manipulation-resistant trust framework for the Pi ecosystem. Every participant — buyer, seller, or app — earns trust through verifiable on-chain actions.

## 1.1 Core Principles

- **Verifiable**: Trust scores are derived from on-chain events only — no off-chain claims
- **Transparent**: The scoring formula is public and immutable
- **Portable**: Trust scores travel with users across all Pi apps
- **Recoverable**: Poor scores decay over time with good behavior
- **Stake-Weighted**: Higher stake = more impact on others' scores (prevents Sybil)

## 1.2 Trust Score Range

| Range | Label | Color | Implications |
|---|---|---|---|
| 4.5 - 5.0 | ⭐ Excellent | Green | Lowest fees, highest visibility, dispute advantage |
| 3.5 - 4.4 | ✅ Good | Blue | Standard fees, normal visibility |
| 2.5 - 3.4 | ⚠️ Fair | Yellow | Higher fees, reduced visibility |
| 1.5 - 2.4 | 🔴 Poor | Orange | Maximum fees, minimal visibility, escrow required |
| 0.0 - 1.4 | ❌ Untrusted | Red | Banned from marketplace, stake slashed |

## 1.3 Score Architecture

```
┌─────────────────────────────────────────┐
│           TRUST SCORE (0-5)              │
│                                         │
│  ┌───────────┐ ┌───────────┐ ┌────────┐│
│  │ Transaction│ │  Review   │ │Dispute ││
│  │  History   │ │  Score    │ │Record  ││
│  │  (40%)     │ │  (30%)    │ │(20%)   ││
│  └───────────┘ └───────────┘ └────────┘│
│                                         │
│  ┌─────────────────────────────────────┐│
│  │  Account Age & Stake Weight (10%)    ││
│  └─────────────────────────────────────┘│
└─────────────────────────────────────────┘
```

Next: [`2-Trust-Score-Calculation`](2-trust-score-calculation.md)
