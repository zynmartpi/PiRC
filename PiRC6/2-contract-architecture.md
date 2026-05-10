# 2. Contract Architecture

## 2.1 Soroban Storage Strategy

Soroban has three storage types with different costs and lifetimes:

| Storage Type | Lifetime | Cost | Use In ZYN |
|---|---|---|---|
| **Persistent** | Permanent (until removed) | High | Token balances, trust scores, service definitions |
| **Temporary** | Until ledger TTL expires | Low | Order escrows, grace period state, dispute votes |
| **Instance** | Contract-level | Medium | Admin, token address, config parameters |

### TTL Management

```rust
// Temporary storage auto-expires — we set TTLs strategically
const GRACE_PERIOD_LEDGERS: u32 = 720;    // ~3 days (5 sec/ledger)
const DISPUTE_VOTE_LEDGERS: u32 = 4800;  // ~7 days
const ESCROW_LEDGERS: u32 = 12000;       // ~17 days (max escrow hold)
const ORDER_LEDGERS: u32 = 12000;         // ~17 days (auto-confirm window)
```

## 2.2 Contract Communication

Contracts communicate via Soroban's cross-contract invocation:

```rust
// Marketplace contract checks trust score before listing
let trust_score = env.invoke_contract(
    &trust_contract,
    &Symbol::new("get_trust_score"),
    vec![merchant.to_val()]
);

// Subscription contract burns ZYN on cancellation
env.invoke_contract(
    &token_contract,
    &Symbol::new("burn"),
    vec![burn_amount.to_val()]
);
```

## 2.3 Error Handling Pattern

```rust
#[contracterror]
#[derive(Copy, Clone, Debug, Eq, PartialEq, PartialOrd, Ord)]
#[repr(u32)]
pub enum ZynError {
    InsufficientBalance = 1,
    InsufficientAllowance = 2,
    Unauthorized = 3,
    GracePeriodActive = 4,
    DisputeAlreadyOpen = 5,
    InsufficientMerchantStake = 6,
    SubscriptionExpired = 7,
    PauseLimitExceeded = 8,
    InvalidPaymentToken = 9,
    // ... all errors from PiRC2-5 mapped here
}
```

## 2.4 Event Emission Pattern

```rust
// Every state change emits an event for Firestore sync
env.events().publish(
    (Symbol::new("zyn"), Symbol::new("subscription"), Symbol::new("charge")),
    (subscriber.to_val(), sub_id.to_val(), amount.to_val())
);
```

Events are consumed by the backend sync layer (PiRC5) to update Firestore cache.

## 2.5 Upgrade Mechanism

```rust
pub fn upgrade(env: Env, new_wasm_hash: BytesN<32>) {
    let admin = read_admin(&env);
    admin.require_auth();
    env.deployer().update_current_contract_wasm(new_wasm_hash);
    env.events().publish(
        (Symbol::new("zyn"), Symbol::new("contract"), Symbol::new("upgrade")),
        (new_wasm_hash,)
    );
}
```

All contracts implement this pattern — admin can fix bugs without migrating data.

Next: [`3-ZYN-Token-Contract`](3-zyn-token-contract.md)
