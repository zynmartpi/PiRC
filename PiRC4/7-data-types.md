# 7. Data Types

## 7.1 TrustProfile

| Field | Type | Description |
|---|---|---|
| `user` | `Address` | User's address |
| `universal_score` | `f32` | Weighted trust score (0-5) |
| `role` | `UserRole` | Buyer / Merchant / Both |
| `total_transactions` | `u64` | Total on-chain transactions |
| `total_reviews_given` | `u32` | Reviews written |
| `total_reviews_received` | `u32` | Reviews received |
| `disputes_filed` | `u32` | Disputes filed as buyer |
| `disputes_won` | `u32` | Disputes won |
| `disputes_lost` | `u32` | Disputes lost |
| `stake_amount` | `i128` | Total ZYN staked |
| `account_created_at` | `u64` | First on-chain action timestamp |
| `last_updated` | `u64` | Last score update timestamp |

## 7.2 AppScore

| Field | Type | Description |
|---|---|---|
| `app_id` | `String` | App identifier |
| `score` | `f32` | App-specific trust score |
| `transaction_count` | `u64` | Transactions in this app |
| `review_avg` | `f32` | Average review in this app |
| `dispute_count` | `u32` | Disputes in this app |

## 7.3 Review

| Field | Type | Description |
|---|---|---|
| `review_id` | `u64` | Auto-incremented ID |
| `reviewer` | `Address` | Buyer's address |
| `merchant` | `Address` | Seller's address |
| `order_id` | `u64` | Verified order |
| `quality` | `u8` | Product quality (1-5) |
| `shipping` | `u8` | Shipping speed (1-5) |
| `accuracy` | `u8` | As described (1-5) |
| `communication` | `u8` | Communication (1-5) |
| `reviewer_weight` | `f32` | Weight based on reviewer trust |
| `created_at` | `u64` | Ledger timestamp |

## 7.4 AuditFlag

| Field | Type | Description |
|---|---|---|
| `flag_id` | `u64` | Auto-incremented ID |
| `reporter` | `Address` | Who reported |
| `target` | `Address` | Who is flagged |
| `reason` | `FlagReason` | Sybil/WashTrading/ReviewSwap/Other |
| `evidence_hash` | `Bytes` | Evidence hash |
| `status` | `FlagStatus` | Open/Investigating/Resolved/Dismissed |
| `deposit` | `i128` | Reporter's 50 ZYN deposit |
| `created_at` | `u64` | Ledger timestamp |

---

*This concludes PiRC4: Reputation & Trust Score System. For the cross-app wallet interoperability protocol, see [PiRC5](../PiRC5/ReadMe.md).*
