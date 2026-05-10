# 2. Core Design

## 2.1 Launch Phases

Each ZYN token launch follows these phases:

1. **Participation Window (Stake + Commerce Engagement)** — Users stake Pi to receive ZYNPower (analogous to PiPower). During this window, users are incentivized to transact on ZynMart and partner apps. Commerce engagement is measured by: purchases completed, reviews written, referrals made, and merchant registrations. At window close, both staking and engagement are snapshotted.

2. **Merchant Staking Period** — Merchants must stake ZYN proportional to their expected monthly sales volume. This creates:
   - Natural buy pressure before TGE
   - Skin-in-the-game for merchants
   - Enhanced liquidity depth

3. **Allocation Period: Liquidity Formation & Distribution** — The Launchpad forms permanent liquidity and distributes tokens. ZYN uses a **hybrid allocation** combining the best of PiRC1 Design 1 & 2:
   - Fixed-price portion (from Design 2) for predictability
   - Engagement-weighted allocation (from Design 1) for fairness
   - Merchant reserve allocation (new) for commerce utility

4. **Official TGE / Market Opening** — LP/trading market opens for all.

5. **Post-Launch: Continuous Utility Phase** — ZYN enters its permanent utility cycle:
   - 0.5% burn on every transaction
   - Merchant staking requirements increase with sales
   - Governance voting begins
   - Cross-app integrations activate

## 2.2 High-Level Token Flows

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Pioneers   │     │  Merchants   │     │   Project    │
│              │     │              │     │   (ZynMart)  │
└──────┬───────┘     └──────┬───────┘     └──────┬───────┘
       │                    │                    │
       │ stake Pi           │ stake Pi           │ tokens
       ▼                    ▼                    ▼
┌──────────────────────────────────────────────────────┐
│                    Escrow Wallet                      │
│  ┌─────────┐  ┌──────────┐  ┌───────────────────┐   │
│  │Pioneer  │  │Merchant  │  │  Project Tokens   │   │
│  │Pi Stake │  │Pi Stake  │  │  (Liquidity +     │   │
│  │         │  │          │  │   Merchant Reserve│   │
│  └────┬────┘  └────┬─────┘  └────────┬──────────┘   │
└───────┼─────────────┼────────────────┼──────────────┘
        │             │                │
        ▼             ▼                ▼
┌──────────────────────────────────────────────────────┐
│                  Liquidity Pool                       │
│         Pi ↔ ZYN swaps (permanent, immutable)        │
│                                                      │
│  • Escrow Wallet LOCKED — cannot withdraw LP         │
│  • 0.5% of each swap burned automatically            │
│  • Additional deposits welcome (fee-earning LP)      │
└──────────────────────────────────────────────────────┘
```

## 2.3 Token Distribution

| Allocation | Percentage | Purpose |
|---|---|---|
| **Launchpad Participants** | 40% | Staking + engagement rewards |
| **Liquidity Pool** | 25% | Permanent LP seed (Escrow-locked) |
| **Merchant Reserve** | 15% | Merchant staking rewards & fee subsidies |
| **Ecosystem Development** | 10% | Cross-app integrations, partnerships, grants |
| **Team (ZynMart)** | 10% | 4-year vesting, 1-year cliff |

### Key Differences from PiRC1:
- **Merchant Reserve (15%)** — New. Ensures merchants have ZYN to stake, creating buy pressure and utility demand.
- **Ecosystem Development (10%)** — New. Funds cross-app integrations so ZYN isn't limited to ZynMart.
- **Team tokens have vesting** — PiRC1 didn't specify team vesting; we enforce 4-year vesting with 1-year cliff.

Next: [`3-Participation`](3-participation.md)
