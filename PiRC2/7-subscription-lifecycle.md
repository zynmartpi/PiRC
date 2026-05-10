# 7. Subscription Lifecycle (Enhanced)

## 7.1 Subscribe

#### `subscribe(subscriber, service_id, pay_upfront, payment_token, committed_periods) -> Subscription`

Creates a subscription with enhanced options.

| Param | Type | Description |
|---|---|---|
| `subscriber` | `Address` | Subscriber's address |
| `service_id` | `u64` | Service to subscribe to |
| `pay_upfront` | `bool` | Auto-renewal flag |
| `payment_token` | `TokenAddress` | Pi or ZYN (NEW) |
| `committed_periods` | `u64` | Discount tier commitment (0 = monthly) (NEW) |

### Subscribe Behavior Matrix

|  | `pay_upfront = true` | `pay_upfront = false` |
|---|---|---|
| **With trial** | No immediate payment. Approve for `approve_periods`. After trial, `process()` charges each cycle. | Trial only. No approval, no payment. Expires at trial end. |
| **Without trial, monthly** | Transfer 1st period price. Approve for `approve_periods`. | Transfer 1st period. Approve for 1 period. Expires after. |
| **Without trial, committed** (NEW) | Transfer `committed_periods × discounted_price`. Approve for `approve_periods`. Monthly release to merchant from escrow. | Not allowed (must be `pay_upfront` for commitments). |

**Errors:** `ServiceNotFound`, `AlreadySubscribed`, `TimestampOverflow`, `InvalidPaymentToken`, `InvalidDiscountTier`, `TrialCooldownActive`

**Events:** `sub`, `approve`, `low_bal`

## 7.2 Cancel (Enhanced with Pro-Rata Refund)

#### `cancel(subscriber, sub_id) -> RefundResult`

Cancels subscription with pro-rata refund for unused time.

$$
RefundAmount = \left\lfloor\frac{RemainingSecs}{PeriodSecs} \times PaidPrice \times 0.9\right\rfloor
$$

10% cancellation fee is **burned** (not given to merchant).

**RefundResult:**

| Field | Type | Description |
|---|---|---|
| `refund_amount` | `i128` | Amount refunded to subscriber |
| `burn_amount` | `i128` | Amount burned as cancellation fee |
| `active_until_ts` | `u64` | When subscription actually ends |

**Errors:** `SubscriptionNotFound`, `Unauthorized`, `AlreadyCancelled`, `GracePeriodActive`

**Events:** `cancel`, `refund`, `burn`

## 7.3 Pause (NEW)

#### `pause(subscriber, sub_id) -> Subscription`

Pauses subscription. Billing halted, remaining time preserved.

- Max pause: 30 days per year per subscription
- Service access suspended during pause
- Auto-resume after 30 days if not manually resumed

**Errors:** `SubscriptionNotFound`, `Unauthorized`, `PauseLimitExceeded`, `AlreadyPaused`

**Events:** `pause`

## 7.4 Resume (NEW)

#### `resume(subscriber, sub_id) -> Subscription`

Resumes a paused subscription. Billing resumes from pause point.

**Errors:** `SubscriptionNotFound`, `Unauthorized`, `NotPaused`

**Events:** `resume`

## 7.5 Toggle Pay Upfront

#### `toggle_pay_upfront(subscriber, sub_id) -> bool`

Same as PiRC2. Toggles auto-renewal. Refreshes token approval when enabling.

**Errors:** `SubscriptionNotFound`, `Unauthorized`, `SubscriptionExpired`

**Events:** `renew`, `approve`

## 7.6 Extend Subscription

#### `extend_subscription(subscriber, sub_id) -> Subscription`

Same as PiRC2. Refreshes approval and sets `pay_upfront = true`.

**Errors:** `SubscriptionNotFound`, `Unauthorized`, `SubscriptionExpired`, `ServiceNotFound`

**Events:** `extend`, `approve`

## 7.7 Process (Enhanced Batch Charge)

#### `process(merchant, service_id) -> ProcessResult`

Enhanced batch charge with grace period awareness and retry logic.

For each subscription:
- **Paused**: Skip (don't charge, don't expire)
- **In Grace**: Retry charge if within 3-day window
- **Grace Expired**: Auto-cancel, emit `grace_expired`
- **Due & Active**: Attempt charge via `transfer_from`
  - **Success**: Advance `next_charge_ts`, emit `charge`
  - **Failure**: Enter grace state, emit `grace_start`

**Errors:** `ServiceNotFound`, `NotServiceOwner`, `TimestampOverflow`

**Events per subscription:**
- `charge` — successful payment
- `trl_end` — first charge after trial
- `grace_start` — payment failed, entered grace (NEW)
- `grace_expired` — grace period ended, auto-cancelled (NEW)
- `low_alw` — remaining allowance < price
- `low_bal` — subscriber balance < price
- `chg_fail` — payment failed (legacy, same as grace_start)

Next: [`8-Dispute-Resolution`](8-dispute-resolution.md)
