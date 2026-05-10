# 5. Marketplace Contract

Implements PiRC3 on-chain marketplace with escrow, delivery proof, and jury disputes.

## 5.1 Core Functions

```rust
// === LISTINGS ===
fn create_listing(env: Env, merchant: Address, product: ProductData, stake_amount: i128) -> u64;
fn update_listing(env: Env, merchant: Address, listing_id: u64, updates: ListingUpdates);
fn deactivate_listing(env: Env, merchant: Address, listing_id: u64);

// === ORDERS & ESCROW ===
fn initiate_purchase(env: Env, buyer: Address, listing_id: u64, quantity: u32,
                     payment_token: Address) -> u64;
fn submit_shipping_proof(env: Env, merchant: Address, order_id: u64, proof_hash: Bytes, carrier: String);
fn confirm_delivery(env: Env, buyer: Address, order_id: u64);
fn auto_confirm_expired(env: Env, order_id: u64); // Called by cron

// === DISPUTES (JURY) ===
fn file_dispute(env: Env, buyer: Address, order_id: u64, reason: DisputeReason,
                evidence_hash: Bytes) -> u64;
fn submit_juror_vote(env: Env, juror: Address, dispute_id: u64, vote: bool);
fn resolve_dispute(env: Env, dispute_id: u64) -> DisputeResult;
fn appeal_dispute(env: Env, caller: Address, dispute_id: u64);

// === FEE DISTRIBUTION ===
fn distribute_fees(env: Env, order_id: u64);
```

## 5.2 Escrow Flow

```rust
fn initiate_purchase(env: Env, buyer: Address, listing_id: u64, quantity: u32,
                     payment_token: Address) -> u64 {
    buyer.require_auth();

    let listing = read_listing(&env, listing_id);
    let total_amount = listing.price * quantity as i128;

    // Transfer payment to escrow (contract address)
    transfer_to_escrow(&env, &buyer, &env.current_contract_address(), total_amount, &payment_token);

    // Create order with timeout
    let order = Order {
        order_id: next_id(&env),
        buyer: buyer.clone(),
        merchant: listing.merchant.clone(),
        listing_id,
        quantity,
        amount: total_amount,
        payment_token,
        status: OrderStatus::Pending,
        escrow_amount: total_amount,
        shipping_proof: Bytes::new(&env),
        created_at: env.ledger().timestamp(),
        timeout_at: env.ledger().timestamp() + AUTO_CONFIRM_SECS,
    };

    write_order(&env, &order);

    env.events().publish(
        (Symbol::new("zyn"), Symbol::new("marketplace"), Symbol::new("order_created")),
        (buyer.to_val(), order.order_id.to_val(), total_amount.to_val())
    );

    order.order_id
}
```

## 5.3 Fee Distribution

```rust
fn distribute_fees(env: Env, order_id: u64) {
    let order = read_order(&env, order_id);
    assert!(order.status == OrderStatus::Delivered);

    let total = order.escrow_amount;
    let burn_amount = total / 200;       // 0.5% burn
    let lp_amount = total / 100;         // 1.0% LP
    let treasury_amount = total / 200;   // 0.5% treasury
    let merchant_amount = total - burn_amount - lp_amount - treasury_amount;

    // Release to merchant
    release_from_escrow(&env, &order.merchant, merchant_amount, &order.payment_token);
    // Burn
    burn_from_escrow(&env, burn_amount, &order.payment_token);
    // LP deposit
    deposit_to_lp(&env, lp_amount, &order.payment_token);
    // Treasury
    deposit_to_treasury(&env, treasury_amount, &order.payment_token);
}
```

## 5.4 Jury Selection Algorithm

```rust
fn select_jurors(env: Env, dispute_id: u64) -> Vec<Address> {
    // Get all ZYN stakers with >= 100 ZYN stake
    let eligible = get_eligible_jurors(&env);

    // Random selection using ledger randomness
    let seed = env.ledger().sequence();
    let mut jurors = Vec::new(&env);

    for i in 0..5 {
        let index = (seed + i as u32) as usize % eligible.len();
        jurors.push(eligible[index].clone());
    }

    jurors
}
```

Next: [`6-Trust-Score-Contract`](6-trust-score-contract.md)
