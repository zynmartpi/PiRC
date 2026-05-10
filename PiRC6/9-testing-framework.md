# 9. Testing Framework

## 9.1 Test Pyramid

```
        ┌─────────────┐
        │   Scenario   │  ← Full ecosystem simulation (5 contracts)
        │     Tests    │     10 tests
        ├─────────────┤
        │ Integration  │  ← Cross-contract interactions
        │    Tests     │     50 tests
        ├─────────────┤
        │    Unit      │  ← Individual contract functions
        │    Tests     │     200+ tests
        └─────────────┘
```

## 9.2 Unit Test Example

```rust
#[test]
fn test_burn_on_transfer() {
    let env = Env::default();
    let admin = Address::generate(&env);
    let user_a = Address::generate(&env);
    let user_b = Address::generate(&env);

    let contract_id = env.register_contract(None, ZynToken);
    let client = ZynTokenClient::new(&env, &contract_id);

    // Initialize and mint
    client.initialize(&admin, &7, &"ZYN Token", &"ZYN");
    client.mint(&admin, &user_a, &1000_0000000); // 1000 ZYN

    // Transfer 100 ZYN
    client.transfer(&user_a, &user_b, &100_0000000);

    // Verify: user_b gets 99.5 ZYN (0.5% burned)
    assert_eq!(client.balance_of(&user_b), 99_5000000);
    // Verify: total supply decreased by 0.5 ZYN
    assert_eq!(client.total_supply(), 999_5000000);
}
```

## 9.3 Integration Test Example

```rust
#[test]
fn test_subscription_with_grace_period() {
    let env = Env::default();

    // Deploy token + subscription contracts
    let token_id = env.register_contract(None, ZynToken);
    let sub_id = env.register_contract(None, ZynSubscription);

    let token = ZynTokenClient::new(&env, &token_id);
    let sub = ZynSubscriptionClient::new(&env, &sub_id);

    // Setup: merchant registers service, subscriber subscribes
    let merchant = Address::generate(&env);
    let subscriber = Address::generate(&env);

    token.mint(&admin, &subscriber, &1000_0000000);
    sub.register_service(&merchant, &"Premium Plan", &50_0000000, &2592000, &0, &12);

    // Subscribe with auto-renewal
    let sub_result = sub.subscribe(&subscriber, &1, &token_id, &0);

    // Fast-forward past charge date with insufficient balance
    env.ledger().set_timestamp(env.ledger().timestamp() + 2592000);
    token.burn(&subscriber, &950_0000000); // Drain balance

    // Process — should enter grace period
    let result = sub.process(&merchant, &1);
    assert_eq!(result.grace_started, 1);

    // Top up balance
    token.mint(&admin, &subscriber, &100_0000000);

    // Process again — should succeed
    let result = sub.process(&merchant, &1);
    assert_eq!(result.charged, 1);
}
```

## 9.4 Scenario Test Example

```rust
#[test]
fn test_full_marketplace_flow() {
    let env = Env::default();

    // Deploy all 5 contracts
    let token = deploy_token(&env);
    let trust = deploy_trust(&env);
    let marketplace = deploy_marketplace(&env);
    let subscription = deploy_subscription(&env);
    let wallet = deploy_wallet(&env);

    // Scenario: Merchant lists product, buyer purchases, delivery confirmed
    let merchant = Address::generate(&env);
    let buyer = Address::generate(&env);

    // 1. Merchant stakes ZYN and creates listing
    token.mint(&admin, &merchant, &500_0000000);
    token.stake(&merchant, &200_0000000);
    marketplace.create_listing(&merchant, &product_data(), &200_0000000);

    // 2. Buyer purchases with escrow
    token.mint(&admin, &buyer, &100_0000000);
    let order_id = marketplace.initiate_purchase(&buyer, &1, &1, &token.contract_id);

    // 3. Merchant ships
    marketplace.submit_shipping_proof(&merchant, &order_id, &proof_hash(), &"DHL");

    // 4. Buyer confirms
    marketplace.confirm_delivery(&buyer, &order_id);

    // 5. Fees distributed
    marketplace.distribute_fees(&order_id);

    // 6. Trust scores updated
    let merchant_trust = trust.get_trust_score(&merchant);
    assert!(merchant_trust > 0.0);

    // 7. Verify burn happened
    let total_supply = token.total_supply();
    assert!(total_supply < initial_supply); // Supply decreased
}
```

## 9.5 Gas Benchmarks

| Operation | Estimated Gas | Cost (Pi) |
|---|---|---|
| **Transfer ZYN** | ~50,000 | ~0.001 Pi |
| **Subscribe** | ~120,000 | ~0.002 Pi |
| **Process (batch 100)** | ~2,000,000 | ~0.04 Pi |
| **Create Listing** | ~80,000 | ~0.0016 Pi |
| **Initiate Purchase** | ~100,000 | ~0.002 Pi |
| **File Dispute** | ~150,000 | ~0.003 Pi |
| **Update Trust Score** | ~60,000 | ~0.0012 Pi |

---

*This concludes PiRC6: Smart Contract Reference Implementation. For AI-enhanced ecosystem intelligence, see [PiRC7](../PiRC7/ReadMe.md).*
