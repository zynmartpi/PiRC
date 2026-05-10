# 7. Data Types

## 7.1 WalletProfile

| Field | Type | Description |
|---|---|---|
| `user` | `Address` | User's on-chain address |
| `username` | `String` | Pi username (document ID) |
| `pi_balance` | `i128` | Pi balance (on-chain) |
| `zyn_balance` | `i128` | ZYN balance (on-chain) |
| `locked_zyn` | `i128` | ZYN locked in stakes/escrow |
| `total_earned` | `i128` | Lifetime ZYN earned |
| `total_spent` | `i128` | Lifetime ZYN spent |
| `total_burned` | `i128` | Lifetime ZYN burned |
| `app_allowances` | `Vec<AppAllowance>` | Active app allowances |
| `created_at` | `u64` | Wallet creation timestamp |

## 7.2 AppAllowance

| Field | Type | Description |
|---|---|---|
| `app_id` | `String` | App identifier |
| `allowance` | `i128` | Max ZYN the app can spend |
| `spent` | `i128` | Amount already spent |
| `expiration_ledger` | `u64` | When allowance expires |
| `created_at` | `u64` | When allowance was set |

## 7.3 CrossAppTransaction

| Field | Type | Description |
|---|---|---|
| `tx_id` | `String` | Unique transaction ID |
| `type` | `TxType` | Purchase/Transfer/Reward/Swap/Burn |
| `from_user` | `Address` | Sender |
| `to_user` | `Address` | Receiver |
| `amount` | `i128` | Transaction amount |
| `fee` | `i128` | Fee amount |
| `burn` | `i128` | Burned amount (0.5%) |
| `token` | `TokenAddress` | Pi or ZYN |
| `source_app` | `String` | Initiating app |
| `dest_app` | `String` | Destination app |
| `order_id` | `u64` | Optional linked order |
| `status` | `TxStatus` | Pending/Confirmed/Failed |
| `created_at` | `u64` | Ledger timestamp |

## 7.4 AppIntegration

| Field | Type | Description |
|---|---|---|
| `app_id` | `String` | App identifier |
| `name` | `String` | App display name |
| `tier` | `IntegrationTier` | Accept/Reward/Govern/Launch |
| `stake` | `i128` | ZYN staked by app |
| `is_active` | `bool` | Whether integration is active |
| `total_transactions` | `u64` | Transactions processed |
| `total_volume` | `i128` | ZYN volume processed |
| `approved_at` | `u64` | Council approval timestamp |

---

*This concludes PiRC5: Cross-App Wallet Interoperability Protocol. Together with PiRC1-4, these five proposals form a comprehensive framework for a decentralized, trustless, and interoperable Pi ecosystem powered by ZYN.*
