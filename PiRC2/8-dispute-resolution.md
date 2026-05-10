# 8. Dispute Resolution (NEW)

## 8.1 Overview

The enhanced subscription contract includes an on-chain dispute resolution mechanism to protect subscribers from unfair charges while ensuring merchants can respond to complaints.

## 8.2 Dispute Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│ Subscriber│     │ Contract │     │ Merchant │     │ Council  │
│  Files    │────▶│  Escrow  │     │ Responds │     │  Votes   │
│  Dispute  │     │  Amount  │────▶│ (5 days) │     │ (>1K ZYN)│
└──────────┘     └──────────┘     └────┬─────┘     └────┬─────┘
                                        │                 │
                                   ┌────▼─────┐     ┌────▼─────┐
                                   │ Resolved │     │ Resolved │
                                   │ Mutual/  │     │ Council  │
                                   │ Default  │     │ Vote     │
                                   └──────────┘     └──────────┘
```

## 8.3 Dispute Methods

### `dispute(subscriber, sub_id, charge_ts, reason) -> Dispute`

Opens a dispute for a specific charge. Must be within 7 days of the charge.

| Param | Type | Description |
|---|---|---|
| `subscriber` | `Address` | Disputer's address |
| `sub_id` | `u64` | Subscription ID |
| `charge_ts` | `u64` | Timestamp of the disputed charge |
| `reason` | `String` | Dispute reason |

**Pre-conditions:**
- Charge must have occurred within last 7 days
- No existing open dispute for same charge
- Subscriber must be the subscription owner

**Actions:**
- Disputed amount transferred from merchant to escrow
- Subscription enters `disputed` state (not charged during dispute)

**Errors:** `SubscriptionNotFound`, `Unauthorized`, `DisputeAlreadyOpen`, `NotDisputeParticipant`

**Events:** `dispute_open`, `escrow_hold`

### `resolve_dispute_merchant(merchant, dispute_id, accept) -> DisputeResult`

Merchant responds to dispute. If `accept = true`, subscriber wins automatically.

| Param | Type | Description |
|---|---|---|
| `merchant` | `Address` | Merchant's address |
| `dispute_id` | `u64` | Dispute ID |
| `accept` | `bool` | Whether merchant accepts the dispute |

**If merchant accepts:**
- Full refund to subscriber from escrow
- 10% of merchant bond burned as penalty
- Subscription cancelled

**If merchant rejects:**
- Dispute enters 5-day response window
- If no further action, automatic resolution in subscriber's favor

**Errors:** `DisputeNotFound`, `NotDisputeParticipant`

**Events:** `dispute_accept`, `dispute_reject`, `refund`, `burn`

### `resolve_dispute_council(dispute_id, vote_for_subscriber) -> DisputeResult`

Council resolution for high-value disputes (> 1000 ZYN).

| Param | Type | Description |
|---|---|---|
| `dispute_id` | `u64` | Dispute ID |
| `vote_for_subscriber` | `bool` | Council's decision |

**Errors:** `DisputeNotFound`, `Unauthorized` (only council can call)

**Events:** `dispute_council`, `refund` or `release`

## 8.4 Dispute Outcomes

| Outcome | Subscriber Gets | Merchant Gets | Burn |
|---|---|---|---|
| **Subscriber Wins** | Full refund + 10% bond | 0 | 0 |
| **Merchant Wins** | 0 | Escrow released | 0 |
| **Partial (50/50)** | 50% refund | 50% released | 0 |
| **No Response (5 days)** | Full refund | 0 | 5% of escrow |

## 8.5 Anti-Abuse Measures

- **Dispute Limit**: Max 3 disputes per subscriber per 30 days
- **Frivolous Dispute Penalty**: If council rules against subscriber 3 times, subscriber loses dispute rights for 90 days
- **Merchant Bond Slash**: Merchants with >20% dispute loss rate have bond slashed 5% per quarter

Next: [`9-Query-Methods`](9-query-methods.md)
