# 7. Wallet Interop Contract

Implements PiRC5 cross-app wallet with allowances, atomic transfers, and balance sync events.

## 7.1 Core Functions

```rust
// === ALLOWANCE MANAGEMENT ===
fn approve_app(env: Env, user: Address, app_id: String, allowance: i128, expiration_ledger: u64);
fn revoke_app(env: Env, user: Address, app_id: String);
fn get_app_allowance(env: Env, user: Address, app_id: String) -> AppAllowance;

// === CROSS-APP TRANSFERS ===
fn transfer_cross_app(env: Env, from: Address, to: Address, amount: i128,
                      source_app: String, dest_app: String, order_id: Option<u64>) -> String;

// === BALANCE QUERIES ===
fn get_on_chain_balance(env: Env, user: Address) -> i128;
fn get_wallet_profile(env: Env, user: Address) -> WalletProfile;

// === APP INTEGRATION ===
fn register_app(env: Env, app_id: String, name: String, tier: IntegrationTier, stake: i128);
fn verify_app_access(env: Env, app_id: String, user: Address) -> bool;

// === EMERGENCY ===
fn emergency_freeze(env: Env, user: Address); // Freeze all allowances
fn unfreeze(env: Env, user: Address);
```

## 7.2 Atomic Cross-App Transfer

```rust
fn transfer_cross_app(env: Env, from: Address, to: Address, amount: i128,
                      source_app: String, dest_app: String, order_id: Option<u64>) -> String {
    from.require_auth();

    // 1. Check on-chain balance
    let balance = get_on_chain_balance(&env, &from);
    assert!(balance >= amount, ZynError::InsufficientBalance);

    // 2. Check app allowance
    let allowance = get_app_allowance(&env, &from, &dest_app);
    assert!(allowance.allowance - allowance.spent >= amount, ZynError::InsufficientAllowance);

    // 3. Calculate burn
    let burn_amount = amount / 200; // 0.5%
    let transfer_amount = amount - burn_amount;

    // 4. Atomic transfer
    internal_transfer(&env, &from, &to, transfer_amount);
    internal_burn(&env, &from, burn_amount);

    // 5. Update allowance spent
    update_allowance_spent(&env, &from, &dest_app, amount);

    // 6. Generate tx_id
    let tx_id = generate_tx_id(&env);

    // 7. Emit events for Firestore sync
    env.events().publish(
        (Symbol::new("zyn"), Symbol::new("wallet"), Symbol::new("cross_app_transfer")),
        (tx_id.clone().to_val(), from.to_val(), to.to_val(), amount.to_val(),
         source_app.to_val(), dest_app.to_val())
    );

    tx_id
}
```

## 7.3 Event-Driven Sync

Every contract emits events that the backend consumes:

```rust
// Event format: (namespace, contract, action, data...)
env.events().publish(
    (Symbol::new("zyn"), Symbol::new("wallet"), Symbol::new("balance_changed")),
    (user.to_val(), new_balance.to_val())
);
```

Backend listener:
```javascript
// Node.js backend listens to Soroban events and updates Firestore
soroban.onEvent('zyn:wallet:balance_changed', (event) => {
  const [user, newBalance] = event.data;
  firestore.collection('users').doc(user).update({
    'balance.zyn': newBalance,
    updated_at: Date.now()
  });
});
```

Next: [`8-Deployment-Pipeline`](8-deployment-pipeline.md)
