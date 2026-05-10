# 3. Enhanced Features

## 3.1 Grace Period

When a payment fails (insufficient balance or allowance), the subscription enters a **grace state** instead of immediate cancellation:

| State | Duration | Subscriber Action | Auto-Action |
|---|---|---|---|
| **Active** | Normal | — | Charge on schedule |
| **Grace** | 3 days | Top up balance or fix allowance | Retry charge daily |
| **Expired** | After grace | Must re-subscribe | Auto-cancel |

During grace period:
- Service remains accessible to subscriber
- Contract retries charge every 24 hours
- Subscriber receives `low_bal` and `grace_start` events
- If balance replenished, charge succeeds and subscription returns to Active

## 3.2 Dispute Resolution

Subscribers can dispute a charge within 7 days of processing:

1. **Dispute Filed**: Subscriber calls `dispute(sub_id, reason)`. Disputed amount is held in escrow.
2. **Merchant Response**: Merchant has 5 days to respond with evidence.
3. **Resolution**:
   - **Mutual Agreement**: Either party can resolve in the other's favor
   - **Automatic**: If merchant doesn't respond in 5 days, subscriber wins
   - **Council Vote**: For disputes > 1000 ZYN, the ZYN Governance Council votes

Dispute outcomes:
- **Subscriber Wins**: Escrowed amount returned + 10% of merchant bond as compensation
- **Merchant Wins**: Escrowed amount released to merchant
- **Partial**: Pro-rata split based on service usage evidence

## 3.3 Merchant Staking

Merchants must stake ZYN to register services:

$$
RequiredStake = BaseStake + (ActiveSubscribers \times StakePerSub)
$$

| Parameter | Value |
|---|---|
| BaseStake | 100 ZYN |
| StakePerSub | 2 ZYN |
| MaxStake | 10,000 ZYN |

If a merchant's stake falls below required, new subscriptions are blocked until restaked.

## 3.4 Tiered Pricing

Services can offer discount tiers for long-term commitments:

| Commitment | Discount | Payment |
|---|---|---|
| Monthly | 0% | Pay per month |
| Quarterly | 5% | Pay 3 months upfront |
| Semi-Annual | 10% | Pay 6 months upfront |
| Annual | 15% | Pay 12 months upfront |

Upfront payments are held in escrow and released to merchant monthly as service is delivered.

## 3.5 Pause/Resume

Subscribers can pause subscriptions without cancellation:

- **Pause**: Service access suspended, billing halted, remaining time preserved
- **Resume**: Service access restored, billing resumes from pause point
- **Max Pause Duration**: 30 days per subscription per year
- **Auto-Resume**: After 30 days, subscription auto-resumes

## 3.6 Pro-Rata Refund

When a subscriber cancels with remaining paid time:

$$
RefundAmount = (RemainingDays / TotalDays) \times PaidAmount \times 0.9
$$

10% is retained as cancellation fee (burned, not given to merchant).

Next: [`4-Data-Types`](4-data-types.md)
