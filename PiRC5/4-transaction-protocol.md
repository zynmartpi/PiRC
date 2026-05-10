# 4. Cross-App Transaction Protocol

## 4.1 Transaction Types

| Type | Description | On-Chain | Firestore |
|---|---|---|---|
| **In-App Purchase** | Buy within same app | ✅ | ✅ |
| **Cross-App Payment** | Pay in App B with ZYN from App A | ✅ | ✅ |
| **Cross-App Transfer** | Move ZYN between own accounts | ✅ | ✅ |
| **Reward Mint** | App rewards user in ZYN | ✅ | ✅ |
| **Swap** | Pi ↔ ZYN via LP | ✅ | ✅ |
| **Burn** | 0.5% transaction burn | ✅ | ✅ |

## 4.2 Cross-App Payment Flow

```
User in App A wants to buy service in App B:

1. App B creates order (price: 50 ZYN)
2. App B calls: approve_app(user, app_b, 50)
3. User confirms in App A's wallet UI
4. App B calls: transfer_cross_app(user, app_b_merchant, 50)
5. Smart contract:
   a. Check user's ZYN balance (on-chain)
   b. Check app_b allowance
   c. Transfer 49.75 ZYN to merchant
   d. Burn 0.25 ZYN (0.5%)
   e. Emit event: cross_app_transfer
6. Backend updates Firestore for both apps
7. Both app UIs update in real-time
```

## 4.3 Transaction Record Format

Every transaction creates a unified record:

| Field | Type | Description |
|---|---|---|
| `tx_id` | `String` | Unique transaction ID |
| `type` | `TxType` | Purchase/Transfer/Reward/Swap/Burn |
| `from` | `Address` | Sender |
| `to` | `Address` | Receiver |
| `amount` | `i128` | Transaction amount |
| `fee` | `i128` | Fee amount |
| `burn` | `i128` | Burned amount |
| `token` | `TokenAddress` | Pi or ZYN |
| `source_app` | `String` | App that initiated |
| `dest_app` | `String` | App that received |
| `order_id` | `u64` | Optional linked order |
| `created_at` | `u64` | Ledger timestamp |

## 4.4 App Allowance System

Users must approve each app to spend their ZYN:

#### `approve_app(user, app_id, allowance, expiration_ledger)`

| Param | Type | Description |
|---|---|---|
| `user` | `Address` | User's address |
| `app_id` | `String` | App identifier |
| `allowance` | `i128` | Max ZYN the app can spend |
| `expiration_ledger` | `u64` | When approval expires |

Users can:
- Set per-app allowances (e.g., 100 ZYN for App B, 500 ZYN for App C)
- Revoke allowances at any time
- Set expiration for auto-revocation
- View all active allowances in wallet UI

## 4.5 Atomic Transactions

Cross-app transactions are **atomic** — either fully completed or fully rolled back:

1. **Check**: Verify balance + allowance
2. **Hold**: Reserve amount from balance
3. **Transfer**: Move to destination
4. **Burn**: Deduct 0.5%
5. **Confirm**: Emit success event

If any step fails, the entire transaction is rolled back and no balance changes occur.

Next: [`5-Security-Model`](5-security-model.md)
