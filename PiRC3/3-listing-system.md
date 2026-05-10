# 3. Listing System

## 3.1 Product Listing

Merchants create on-chain product listings by staking ZYN:

#### `create_listing(merchant, product_data, stake_amount) -> Listing`

| Field | Type | Description |
|---|---|---|
| `listing_id` | `u64` | Auto-incremented ID |
| `merchant` | `Address` | Seller's address |
| `title` | `String` | Product title |
| `description_hash` | `Bytes` | SHA-256 of full description (stored off-chain) |
| `price_pi` | `i128` | Price in Pi |
| `price_zyn` | `i128` | Price in ZYN (optional, 0 = Pi only) |
| `category` | `String` | Product category |
| `stock` | `u32` | Available quantity (0 = unlimited digital) |
| `stake_amount` | `i128` | ZYN staked for this listing |
| `is_active` | `bool` | Whether listing is visible |
| `created_at` | `u64` | Ledger timestamp |

## 3.2 Stake Requirements

Listing stake determines visibility tier:

| Tier | Min Stake | Visibility Boost |
|---|---|---|
| **Basic** | 10 ZYN | Standard search ranking |
| **Featured** | 50 ZYN | 2× search ranking weight |
| **Premium** | 200 ZYN | 5× search ranking weight + category featured |
| **Spotlight** | 500 ZYN | 10× weight + homepage featured + badge |

## 3.3 Listing Lifecycle

```
Created → Active → (Sold Out / Deactivated / Expired)
              │
              └── Merchant can update price, stock, or deactivate
```

- **Update**: Merchant can update price, stock, description hash
- **Deactivate**: Merchant deactivates, stake returned after 7 days (pending disputes)
- **Expire**: Listings auto-expire after 90 days unless renewed

Next: [`4-Escrow-Payment`](4-escrow-payment.md)
