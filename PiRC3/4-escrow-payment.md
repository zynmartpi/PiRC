# 4. Escrow Payment System

## 4.1 Purchase Flow

#### `initiate_purchase(buyer, listing_id, quantity, payment_token) -> Order`

Creates an order with payment in escrow.

| Field | Type | Description |
|---|---|---|
| `order_id` | `u64` | Auto-incremented ID |
| `buyer` | `Address` | Buyer's address |
| `merchant` | `Address` | Seller's address |
| `listing_id` | `u64` | Product listing |
| `quantity` | `u32` | Quantity purchased |
| `amount` | `i128` | Total payment amount |
| `payment_token` | `TokenAddress` | Pi or ZYN |
| `status` | `OrderStatus` | Current order state |
| `escrow_amount` | `i128` | Amount held in escrow |
| `created_at` | `u64` | Ledger timestamp |
| `timeout_at` | `u64` | Auto-confirm deadline |

## 4.2 Order Status Flow

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  PENDING │───▶│  SHIPPED │───▶│ DELIVERED│───▶│ COMPLETE │
│  (Escrow)│    │  (Proof) │    │(Confirmed│    │(Released)│
└────┬─────┘    └────┬─────┘    └──────────┘    └──────────┘
     │               │
     │               │
     ▼               ▼
┌──────────┐    ┌──────────┐
│ CANCELLED│    │ DISPUTED │
│(Refunded)│    │(On Hold) │
└──────────┘    └──────────┘
```

## 4.3 Delivery Confirmation

#### `confirm_delivery(buyer, order_id)`

Buyer confirms receipt. Escrow released to merchant (minus fees).

#### `submit_shipping_proof(merchant, order_id, proof_hash, carrier)`

Merchant submits delivery proof. Starts 7-day confirmation window.

| Param | Type | Description |
|---|---|---|
| `proof_hash` | `Bytes` | Hash of tracking/delivery evidence |
| `carrier` | `String` | Shipping carrier name |

## 4.4 Auto-Confirmation

If buyer doesn't confirm or dispute within 7 days of shipping proof:
- Order auto-confirms
- Escrow released to merchant
- Buyer's trust score slightly decreased (passive behavior penalty)

## 4.5 Cancellation

- **Before shipping**: Full refund, no fee
- **After shipping**: Must go through dispute process

Next: [`5-Dispute-Resolution`](5-dispute-resolution.md)
