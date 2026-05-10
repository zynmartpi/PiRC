# 8. Cross-App Utility & Interoperability

## 8.1 ZYN as a Cross-App Token

ZYN is designed to be usable across the entire Pi ecosystem, not just ZynMart. Any Pi app can integrate ZYN as a payment or reward mechanism.

## 8.2 Integration Tiers for Partner Apps

| Tier | Integration Level | Requirements |
|---|---|---|
| **Accept** | Accept ZYN as payment | Implement ZYN payment API, display ZYN prices |
| **Reward** | Distribute ZYN as rewards | Stake 1,000 ZYN, implement reward distribution API |
| **Govern** | Participate in governance | Stake 5,000 ZYN, appoint governance delegate |
| **Launch** | Launch own token via ZYN Launchpad | Stake 10,000 ZYN, pass community vote |

## 8.3 Cross-App Transfer Protocol

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│ ZynMart  │    │ Partner  │    │ Partner  │
│          │    │  App A   │    │  App B   │
└────┬─────┘    └────┬─────┘    └────┬─────┘
     │               │               │
     │    ZYN Token Contract (Soroban) │
     └───────────────┴───────────────┘
                     │
              ┌──────┴──────┐
              │  ZYN Ledger  │
              │  (Firestore  │
              │   + Chain)   │
              └─────────────┘
```

### Transfer Flow:
1. User earns ZYN in App A (e.g., completing a task)
2. App A calls `mint_reward(user, amount)` on ZYN contract
3. User's ZYN balance updates on-chain
4. User spends ZYN in App B (e.g., purchasing a service)
5. App B calls `transfer_from(user, merchant, amount)` on ZYN contract
6. 0.5% of transfer is burned automatically

## 8.4 Cross-App Balance Synchronization

ZYN balances are synchronized across apps using a dual-layer system:

- **Layer 1 (On-Chain)**: Soroban smart contract holds the canonical balance
- **Layer 2 (Off-Chain)**: Firestore caches balances for real-time UI updates

Synchronization rules:
- Off-chain reads are instant (Firestore)
- On-chain writes are confirmed within 5 seconds (Soroban)
- Discrepancies are resolved in favor of on-chain state
- Apps must check on-chain balance before processing withdrawals

## 8.5 Revenue Sharing

When ZYN is used in a partner app, transaction fees are distributed:

| Recipient | Share | Purpose |
|---|---|---|
| **Burn** | 0.5% | Deflationary mechanism |
| **Partner App** | 1.0% | Integration incentive |
| **ZYN Treasury** | 0.5% | Ecosystem development fund |
| **LP Fee** | 0.3% | Liquidity provider incentive |

This creates a **virtuous cycle**: more partner apps → more ZYN usage → more fee revenue → more ecosystem development → more partner apps.

## 8.6 Governance: Cross-App Council

ZYN governance is managed by a council of stakeholders:

- **ZynMart Team**: 2 seats
- **Top Merchants** (by stake): 2 seats
- **Community Delegates** (elected by ZYN holders): 3 seats
- **Partner App Representatives**: 1 seat

The council votes on:
- New partner app integrations
- Fee adjustments
- Treasury allocation
- Protocol upgrades
- Dispute resolution policies

Council decisions require 5/8 majority. Community delegates are elected quarterly by ZYN holders (1 ZYN = 1 vote).

---

*This concludes PiRC1: ZYN Ecosystem Token Design. For the enhanced subscription contract, see [PiRC2](../PiRC2/ReadMe.md).*
