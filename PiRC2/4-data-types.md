# 4. Data Types

## 4.1 Service (Enhanced)

| Field | Type | Description |
|---|---|---|
| `service_id` | `u64` | Auto-incremented unique identifier |
| `merchant` | `Address` | Service owner's address |
| `name` | `String` | Display name of the service |
| `price` | `i128` | Amount charged per period |
| `period_secs` | `u64` | Billing cycle duration in seconds |
| `trial_period_secs` | `u64` | Free trial duration (0 = no trial) |
| `approve_periods` | `u64` | Periods to pre-approve for recurring |
| `is_active` | `bool` | Whether service accepts new subscriptions |
| `accepts_zyn` | `bool` | Whether ZYN is accepted as payment (NEW) |
| `discount_tiers` | `Vec<DiscountTier>` | Available discount tiers (NEW) |
| `merchant_stake` | `i128` | Required ZYN stake amount (NEW) |
| `created_at` | `u64` | Ledger timestamp at creation |

## 4.2 DiscountTier (NEW)

| Field | Type | Description |
|---|---|---|
| `periods` | `u64` | Number of periods for commitment |
| `discount_bps` | `u32` | Discount in basis points (500 = 5%) |

## 4.3 Subscription (Enhanced)

| Field | Type | Description |
|---|---|---|
| `sub_id` | `u64` | Auto-incremented unique identifier |
| `subscriber` | `Address` | Subscriber's address |
| `service_id` | `u64` | Associated service ID |
| `price` | `i128` | Price locked at subscription time |
| `period_secs` | `u64` | Billing cycle duration |
| `trial_period_secs` | `u64` | Trial duration |
| `trial_end_ts` | `u64` | Timestamp when trial ends |
| `pay_upfront` | `bool` | Whether process() charges this subscription |
| `service_end_ts` | `u64` | When current active period expires |
| `next_charge_ts` | `u64` | Next time process() will attempt charge |
| `grace_start_ts` | `u64` | When grace period started (0 = not in grace) (NEW) |
| `is_paused` | `bool` | Whether subscription is paused (NEW) |
| `pause_remaining_secs` | `u64` | Remaining pause time allowed (NEW) |
| `payment_token` | `TokenAddress` | Pi or ZYN (NEW) |
| `committed_periods` | `u64` | Discount tier commitment (NEW) |
| `created_at` | `u64` | Ledger timestamp at creation |

## 4.4 Dispute (NEW)

| Field | Type | Description |
|---|---|---|
| `dispute_id` | `u64` | Auto-incremented unique identifier |
| `sub_id` | `u64` | Associated subscription |
| `subscriber` | `Address` | Disputer's address |
| `merchant` | `Address` | Respondent's address |
| `amount` | `i128` | Disputed amount |
| `reason` | `String` | Dispute reason |
| `status` | `DisputeStatus` | Open/Resolved/Mutual/Council |
| `resolution_ts` | `u64` | When dispute was resolved |
| `created_at` | `u64` | Ledger timestamp at creation |

## 4.5 ProcessResult (Enhanced)

| Field | Type | Description |
|---|---|---|
| `charged` | `u32` | Successfully charged |
| `failed` | `u32` | Payment failed (entered grace) |
| `skipped` | `u32` | Not due or paused |
| `grace_expired` | `u32` | Grace period expired, auto-cancelled (NEW) |
| `disputed` | `u32` | Charges disputed (NEW) |

Next: [`5-Error-Codes`](5-error-codes.md)
