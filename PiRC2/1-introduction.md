# 1. Introduction

This PiRC2 enhances the original Pi Network Subscription Contract with critical features for real-world marketplace use. Subscriptions are the backbone of modern commerce, but the original design lacked buyer protection, dispute resolution, and merchant accountability.

## 1.1 How It Works (Enhanced)

The enhanced subscription contract preserves PiRC2's core innovation — **non-custodial recurring payments via token allowances** — while adding:

1. **Grace Periods**: Failed payments don't immediately cancel subscriptions. Subscribers get 3 days to resolve payment issues.
2. **Dispute Resolution**: Built-in on-chain dispute mechanism with merchant bond staking.
3. **Merchant Staking**: Merchants must stake ZYN to register services, ensuring commitment.
4. **Dual Token Support**: Accepts both Pi and ZYN as subscription payment tokens.
5. **Tiered Pricing**: Long-term subscribers receive discount tiers.
6. **Pause/Resume**: Subscribers can temporarily pause subscriptions without cancellation.

## 1.2 Use Cases (Expanded)

- **SaaS on Pi**: Productivity tools, AI services, cloud storage
- **E-commerce Memberships**: ZynMart seller premium, buyer clubs, wholesale access
- **Digital Content**: Streaming, news subscriptions, educational platforms
- **Local Commerce**: Gym memberships, recurring deliveries, service contracts
- **Cross-App Services**: Subscribe once, use across multiple Pi apps

## 1.3 Technical Innovation

The enhanced contract introduces **layered payment authorization**:

```
Layer 1: Token Allowance (PiRC2 original)
  → Subscriber approves contract as spender

Layer 2: Grace Period Buffer (NEW)
  → Failed charges enter 3-day grace state
  → Subscriber can top up balance during grace period
  → Contract retries charge after balance replenishment

Layer 3: Dispute Escrow (NEW)
  → Disputed charges are held in escrow
  → Merchant bond covers potential refund
  → Resolution triggers release or refund
```

Next: [`2-Overview`](2-overview.md)
