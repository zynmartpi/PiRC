# 2. Overview

Enhanced Soroban smart contract managing **recurring payment subscriptions** with grace periods, disputes, and merchant staking.

```
flowchart LR
  subgraph A["Merchant"]
    M[Merchant] -->|stake ZYN| Stake[Stake Contract]
    M -->|register_service| S[Service]
  end

  subgraph B["Subscriber"]
    Sub[Subscriber] -->|subscribe| Subscription
    Sub -->|pause/resume| Subscription
    Sub -->|dispute| Escrow[Dispute Escrow]
  end

  subgraph C["Billing Cycle"]
    M -->|process| Charge[Batch Charge]
    Charge -->|try_transfer_from| Token[Token Contract]
    Charge -->|failed| Grace[Grace Period]
    Grace -->|retry| Token
    Grace -->|expired| Cancel[Auto-Cancel]
  end

  Subscription -->|linked to| S
  Subscription -->|token approve| Token
  Stake -->|bond| Escrow
```

## Key Concepts

- **Services** — Merchants define services with pricing, billing cycles, trial periods, and discount tiers.
- **Subscriptions** — Subscribers choose services with auto-renewal, pause/resume, and dispute options.
- **Grace Periods** — Failed payments enter a 3-day grace state before cancellation.
- **Dispute Resolution** — Built-in escrow mechanism with merchant bond coverage.
- **Merchant Staking** — Merchants stake ZYN proportional to their subscriber count.
- **Tiered Pricing** — Discounts for long-term commitments (3mo: 5%, 6mo: 10%, 12mo: 15%).
- **Batch Processing** — Enhanced with retry logic, priority queuing, and grace period awareness.

Next: [`3-Enhanced-Features`](3-enhanced-features.md)
