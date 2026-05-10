# 8. Data Types

## 8.1 Listing

| Field | Type | Description |
|---|---|---|
| `listing_id` | `u64` | Auto-incremented ID |
| `merchant` | `Address` | Seller's address |
| `title` | `String` | Product title |
| `description_hash` | `Bytes` | SHA-256 of full description |
| `price_pi` | `i128` | Price in Pi |
| `price_zyn` | `i128` | Price in ZYN (0 = Pi only) |
| `category` | `String` | Product category |
| `stock` | `u32` | Available quantity |
| `stake_amount` | `i128` | ZYN staked |
| `is_active` | `bool` | Visibility flag |
| `trust_score` | `f32` | Merchant's trust score |
| `created_at` | `u64` | Ledger timestamp |
| `expires_at` | `u64` | Auto-expiry timestamp |

## 8.2 Order

| Field | Type | Description |
|---|---|---|
| `order_id` | `u64` | Auto-incremented ID |
| `buyer` | `Address` | Buyer's address |
| `merchant` | `Address` | Seller's address |
| `listing_id` | `u64` | Product listing |
| `quantity` | `u32` | Quantity |
| `amount` | `i128` | Total payment |
| `payment_token` | `TokenAddress` | Pi or ZYN |
| `status` | `OrderStatus` | Pending/Shipped/Delivered/Complete/Cancelled/Disputed |
| `escrow_amount` | `i128` | Amount in escrow |
| `shipping_proof` | `Bytes` | Hash of delivery evidence |
| `created_at` | `u64` | Ledger timestamp |
| `timeout_at` | `u64` | Auto-confirm deadline |

## 8.3 MarketplaceDispute

| Field | Type | Description |
|---|---|---|
| `dispute_id` | `u64` | Auto-incremented ID |
| `order_id` | `u64` | Associated order |
| `buyer` | `Address` | Disputer |
| `merchant` | `Address` | Respondent |
| `reason` | `DisputeReason` | NotReceived/NotAsDescribed/Damaged/Other |
| `evidence_hash` | `Bytes` | Buyer evidence |
| `merchant_evidence` | `Bytes` | Merchant evidence |
| `jurors` | `Vec<Address>` | 5 selected jurors |
| `votes` | `Vec<bool>` | Juror votes (true = buyer wins) |
| `status` | `DisputeStatus` | Open/Voting/Resolved/Appealed |
| `created_at` | `u64` | Ledger timestamp |

---

*This concludes PiRC3: Decentralized Marketplace Protocol. For the reputation system, see [PiRC4](../PiRC4/ReadMe.md).*
