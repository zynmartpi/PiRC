# 3. Merchant Reputation

## 3.1 Merchant-Specific Metrics

Merchants have additional reputation dimensions beyond the base trust score:

| Metric | Source | Impact |
|---|---|---|
| **Fulfillment Rate** | % of orders shipped on time | ±0.3 trust score |
| **Response Time** | Average time to respond to inquiries | ±0.1 trust score |
| **Return Rate** | % of orders returned | ±0.2 trust score |
| **Repeat Customer Rate** | % of buyers who purchase again | +0.2 trust score |
| **Product Accuracy** | % of "as described" ratings | ±0.3 trust score |

## 3.2 Merchant Tiers

| Tier | Trust Score | Benefits | Requirements |
|---|---|---|---|
| **New Merchant** | 0-2.0 | Basic listing, 3% fee, escrow required | 10 ZYN stake |
| **Rising Star** | 2.0-3.5 | Standard listing, 2.5% fee | 50 ZYN stake, 10+ orders |
| **Trusted Seller** | 3.5-4.5 | Featured listing, 1.5% fee, dispute bond | 200 ZYN stake, 50+ orders |
| **Top Merchant** | 4.5-5.0 | Premium listing, 1% fee, priority support | 500 ZYN stake, 200+ orders |

## 3.3 Merchant Score Display

On-chain merchant scores are displayed with:
- **Overall Trust Score** (0-5.0)
- **Transaction Count** (total completed orders)
- **Review Summary** (average + count)
- **Badge System**: 🥉 Bronze / 🥈 Silver / 🥇 Gold / 💎 Diamond

## 3.4 Merchant Penalties

| Violation | Penalty | Recovery |
|---|---|---|
| **Failed Delivery** | -0.3 trust, 5% stake slash | 3 months good behavior |
| **Misleading Listing** | -0.5 trust, 10% stake slash | 6 months good behavior |
| **Repeated Disputes Lost** | -1.0 trust, 20% stake slash | 12 months good behavior |
| **Fraud (proven)** | -5.0 trust, 100% stake slash, permanent ban | Irreversible |

Next: [`4-Buyer-Reputation`](4-buyer-reputation.md)
