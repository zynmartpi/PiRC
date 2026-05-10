# 4. Subscription Contract

Implements PiRC2 enhanced subscription with grace periods, disputes, and tiered pricing.

## 4.1 Core Functions

```rust
// === SERVICE MANAGEMENT ===
fn register_service(env: Env, merchant: Address, params: ServiceParams) -> u64;
fn deactivate_service(env: Env, merchant: Address, service_id: u64);

// === SUBSCRIPTION LIFECYCLE ===
fn subscribe(env: Env, subscriber: Address, service_id: u64, payment_token: Address,
             committed_periods: u64) -> u64;
fn cancel(env: Env, subscriber: Address, sub_id: u64) -> RefundResult;
fn pause(env: Env, subscriber: Address, sub_id: u64);
fn resume(env: Env, subscriber: Address, sub_id: u64);
fn toggle_pay_upfront(env: Env, subscriber: Address, sub_id: u64) -> bool;
fn extend_subscription(env: Env, subscriber: Address, sub_id: u64);

// === BATCH PROCESSING ===
fn process(env: Env, merchant: Address, service_id: u64) -> ProcessResult;

// === DISPUTE RESOLUTION ===
fn dispute(env: Env, subscriber: Address, sub_id: u64, charge_ts: u64, reason: String) -> u64;
fn resolve_dispute_merchant(env: Env, merchant: Address, dispute_id: u64, accept: bool);
fn resolve_dispute_council(env: Env, dispute_id: u64, vote_for_subscriber: bool);

// === QUERIES ===
fn get_subscription(env: Env, caller: Address, sub_id: u64) -> Subscription;
fn get_merchant_stake(env: Env, merchant: Address) -> StakeInfo;
```

## 4.2 Grace Period Implementation

```rust
fn process_subscription(env: Env, sub: &mut Subscription) -> ChargeResult {
    if sub.is_paused || !sub.pay_upfront {
        return ChargeResult::Skipped;
    }

    if env.ledger().timestamp() < sub.next_charge_ts {
        return ChargeResult::Skipped;
    }

    // Check if in grace period
    if sub.grace_start_ts > 0 {
        let grace_elapsed = env.ledger().timestamp() - sub.grace_start_ts;
        if grace_elapsed > GRACE_PERIOD_SECS {
            // Grace expired — auto-cancel
            sub.pay_upfront = false;
            return ChargeResult::GraceExpired;
        }
        // Still in grace — retry charge
    }

    // Attempt charge
    let result = try_transfer_from(&env, &sub.subscriber, &sub.merchant, sub.price, &sub.payment_token);

    match result {
        Ok(()) => {
            sub.next_charge_ts += sub.period_secs; // No drift
            sub.service_end_ts = sub.next_charge_ts;
            sub.grace_start_ts = 0; // Clear grace
            ChargeResult::Charged
        }
        Err(_) => {
            // Enter grace period
            if sub.grace_start_ts == 0 {
                sub.grace_start_ts = env.ledger().timestamp();
            }
            ChargeResult::GraceStarted
        }
    }
}
```

## 4.3 Pro-Rata Refund

```rust
fn cancel(env: Env, subscriber: Address, sub_id: u64) -> RefundResult {
    subscriber.require_auth();
    let mut sub = get_subscription_mut(&env, sub_id);

    let remaining_secs = sub.service_end_ts - env.ledger().timestamp();
    let refund_ratio = remaining_secs as f64 / sub.period_secs as f64;
    let refund_amount = (sub.price as f64 * refund_ratio * 0.9) as i128; // 90% refund
    let burn_amount = (sub.price as f64 * refund_ratio * 0.1) as i128;   // 10% burn

    // Transfer refund to subscriber
    internal_transfer(&env, &escrow, &subscriber, refund_amount);
    // Burn cancellation fee
    internal_burn(&env, &escrow, burn_amount);

    sub.pay_upfront = false;

    RefundResult { refund_amount, burn_amount, active_until_ts: sub.service_end_ts }
}
```

Next: [`5-Marketplace-Contract`](5-marketplace-contract.md)
