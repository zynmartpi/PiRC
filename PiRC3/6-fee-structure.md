# 6. Fee Structure

## 6.1 Transaction Fees

Every completed order incurs a marketplace fee:

| Component | Rate | Destination |
|---|---|---|
| **Burn** | 0.5% | Permanently burned (deflationary) |
| **Liquidity Pool** | 1.0% | Added to ZYN/Pi LP |
| **Treasury** | 0.5% | Ecosystem development fund |
| **Total** | 2.0% | — |

## 6.2 Fee Discounts by Reputation

Merchants with higher trust scores pay lower fees:

| Trust Score | Fee Rate | Savings |
|---|---|---|
| 0-2.0 | 2.0% | 0% |
| 2.0-3.5 | 1.8% | 10% |
| 3.5-4.5 | 1.5% | 25% |
| 4.5-5.0 | 1.2% | 40% |

This incentivizes quality service — better merchants pay less.

## 6.3 ZYN Payment Discount

Buyers paying with ZYN receive a 5% discount on the marketplace fee (1.9% instead of 2.0%), encouraging ZYN usage.

## 6.4 Fee Distribution Flow

```
Order Payment (100 Pi)
        │
        ▼
┌──────────────┐
│    Escrow     │
└──────┬───────┘
       │
       ▼ (delivery confirmed)
┌──────────────────────────────────┐
│         Fee Distribution          │
│                                   │
│  Merchant: 98.0 Pi               │
│  Burn:     0.5 Pi (0.5%)         │
│  LP:       1.0 Pi (1.0%)         │
│  Treasury: 0.5 Pi (0.5%)         │
└──────────────────────────────────┘
```

Next: [`7-Search-Ranking`](7-search-ranking.md)
