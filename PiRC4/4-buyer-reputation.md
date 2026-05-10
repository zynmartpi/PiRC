# 4. Buyer Reputation

## 4.1 Buyer-Specific Metrics

Buyers also have reputation scores that affect their marketplace experience:

| Metric | Source | Impact |
|---|---|---|
| **Confirmation Rate** | % of orders confirmed on time | ±0.2 trust score |
| **Dispute Frequency** | % of orders disputed | ±0.3 trust score |
| **Dispute Win Rate** | % of disputes won | ±0.1 trust score |
| **Payment Reliability** | % of initiated purchases completed | ±0.2 trust score |
| **Review Quality** | Average review usefulness rating | ±0.1 trust score |

## 4.2 Buyer Benefits by Trust Score

| Trust Score | Benefits |
|---|---|
| **4.5-5.0** | Instant escrow release, priority support, 5% ZYN cashback |
| **3.5-4.4** | Standard escrow (7-day auto-confirm), 3% ZYN cashback |
| **2.5-3.4** | Extended escrow (14-day), 1% ZYN cashback |
| **1.5-2.4** | Mandatory escrow (21-day), no cashback, limited purchases |
| **0.0-1.4** | Marketplace restricted, must rebuild trust |

## 4.3 Buyer Penalties

| Behavior | Penalty |
|---|---|
| **Excessive Disputes** (>30% of orders) | -0.5 trust, dispute rights suspended 30 days |
| **Frivolous Disputes** (council rules against) | -1.0 trust, 90-day dispute ban |
| **Non-Confirmation** (auto-confirm habit) | -0.1 per auto-confirm |
| **Payment Abandonment** (initiate but don't pay) | -0.2 per abandoned order |

## 4.4 Buyer Review System

Buyers can rate purchases on multiple dimensions:

| Dimension | Scale | Weight |
|---|---|---|
| **Product Quality** | 1-5 | 30% |
| **Shipping Speed** | 1-5 | 20% |
| **As Described** | 1-5 | 30% |
| **Communication** | 1-5 | 20% |

Reviews are weighted by buyer trust score — a trusted buyer's review impacts the merchant more than a new buyer's.

Next: [`5-Cross-App-Portability`](5-cross-app-portability.md)
