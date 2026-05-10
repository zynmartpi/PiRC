# 6. Trust Score Contract

Implements PiRC4 on-chain reputation with cross-app portability.

## 6.1 Core Functions

```rust
// === SCORE MANAGEMENT ===
fn update_transaction_score(env: Env, user: Address, completed: bool, on_time: bool);
fn update_review_score(env: Env, reviewer: Address, merchant: Address, rating: ReviewRating);
fn update_dispute_score(env: Env, user: Address, won: bool);
fn recalculate_trust(env: Env, user: Address) -> f32;

// === QUERIES ===
fn get_trust_score(env: Env, user: Address) -> f32;
fn get_universal_trust(env: Env, user: Address) -> TrustInfo;
fn get_merchant_tier(env: Env, merchant: Address) -> MerchantTier;
fn get_reviewer_weight(env: Env, reviewer: Address) -> f32;

// === CROSS-APP ===
fn register_app_score(env: Env, app_id: String, user: Address, score: f32, tx_count: u64);
fn get_app_scores(env: Env, user: Address) -> Vec<AppScore>;

// === ANTI-MANIPULATION ===
fn report_manipulation(env: Env, reporter: Address, target: Address, reason: FlagReason,
                       evidence_hash: Bytes) -> u64;
fn investigate_flag(env: Env, flag_id: u64, confirmed: bool);
```

## 6.2 Trust Score Calculation (On-Chain)

```rust
fn recalculate_trust(env: Env, user: Address) -> f32 {
    let profile = read_profile(&env, &user);

    // Transaction Score (40%)
    let completion_rate = profile.completed_orders as f32 / profile.total_orders as f32;
    let on_time_rate = profile.on_time_deliveries as f32 / profile.completed_orders as f32;
    let transaction_score = 5.0 * (completion_rate * 0.6 + on_time_rate * 0.4);

    // Review Score (30%)
    let review_score = calculate_weighted_review_score(&env, &user);

    // Dispute Score (20%)
    let dispute_score = 5.0 - (profile.disputes_lost as f32 * 0.5)
                            + (profile.disputes_won as f32 * 0.1);
    let dispute_score = dispute_score.max(0.0);
    let dispute_score = apply_time_decay(dispute_score, &profile);

    // Account Score (10%)
    let age_years = (env.ledger().timestamp() - profile.created_at) as f32 / (365.25 * 86400.0);
    let stake_log = (profile.stake_amount as f64 + 1.0).log10() as f32;
    let account_score = (age_years * 0.5 + stake_log * 1.0).min(5.0);

    // Weighted sum
    let total = transaction_score * 0.4
              + review_score * 0.3
              + dispute_score * 0.2
              + account_score * 0.1;

    total.min(5.0)
}
```

## 6.3 Reviewer Weight (Anti-Manipulation)

```rust
fn get_reviewer_weight(env: Env, reviewer: Address) -> f32 {
    let trust = get_trust_score(&env, reviewer.clone());
    let stake = get_stake(&env, &reviewer);
    let stake_factor = (stake as f32 / 1000.0).min(1.0);

    (trust / 5.0 * 0.5 + stake_factor * 0.5).min(1.0)
}
```

Next: [`7-Wallet-Interop-Contract`](7-wallet-interop-contract.md)
