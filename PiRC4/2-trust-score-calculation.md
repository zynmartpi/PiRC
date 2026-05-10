# 2. Trust Score Calculation

## 2.1 Master Formula

$$
TrustScore = \min\left(5.0,\ TransactionScore \times 0.4 + ReviewScore \times 0.3 + DisputeScore \times 0.2 + AccountScore \times 0.1\right)
$$

## 2.2 Transaction Score (40%)

Based on order completion and fulfillment metrics:

$$
TransactionScore = 5.0 \times \left(\frac{CompletedOrders}{TotalOrders} \times 0.6 + \frac{OnTimeDeliveries}{CompletedOrders} \times 0.4\right)
$$

| Metric | Weight | Description |
|---|---|---|
| **Completion Rate** | 60% | % of orders that complete without cancellation/dispute |
| **On-Time Rate** | 40% | % of completed orders delivered within stated timeframe |

## 2.3 Review Score (30%)

Based on verified buyer reviews:

$$
ReviewScore = \frac{\sum_{i}(Rating_i \times ReviewerWeight_i)}{\sum ReviewerWeight_i}
$$

**Reviewer Weight** prevents fake reviews:

$$
ReviewerWeight_i = \min(1.0,\ BuyerTrustScore_i \times 0.5 + BuyerStake_i / 1000 \times 0.5)
$$

- A review from a trusted, staked buyer carries more weight
- A review from a new, unstaked account carries minimal weight
- Only verified buyers (on-chain order exists) can review

## 2.4 Dispute Score (20%)

Based on dispute history:

$$
DisputeScore = 5.0 - (DisputesLost \times 0.5) + (DisputesWon \times 0.1)
$$

- Losing a dispute: -0.5 points
- Winning a dispute: +0.1 points
- Minimum dispute score: 0.0
- **Time decay**: Dispute impact decays 10% per quarter

## 2.5 Account Score (10%)

Based on account age and stake:

$$
AccountScore = \min(5.0,\ AccountAgeYears \times 0.5 + \log_{10}(Stake + 1) \times 1.0)
$$

- Older accounts get a trust bonus (max 2.5 from age)
- Higher stake gets a trust bonus (max 2.5 from stake)
- New accounts with no stake start at ~0.5

## 2.6 Time Decay & Rehabilitation

All negative events decay over time:

| Event | Initial Impact | Half-Life |
|---|---|---|
| **Lost Dispute** | -0.5 | 6 months |
| **Cancelled Order** | -0.1 | 3 months |
| **Late Delivery** | -0.2 | 4 months |
| **Low Review** | Weighted | 12 months |

**Rehabilitation**: After 6 consecutive months of positive activity (no disputes, >90% completion rate), all negative impacts are reduced by 50%.

Next: [`3-Merchant-Reputation`](3-merchant-reputation.md)
