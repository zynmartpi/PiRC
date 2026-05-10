# 6. Merchant Integration

## 6.1 Merchant Staking Tiers

Merchants must stake ZYN to access marketplace features. This creates natural buy pressure and ensures merchant commitment.

| Tier | ZYN Stake | Benefits |
|---|---|---|
| **Bronze** | 100 ZYN | Basic listings, standard fees (3%) |
| **Silver** | 500 ZYN | Featured placement, reduced fees (2%), priority support |
| **Gold** | 1,000 ZYN | Premium placement, reduced fees (1%), analytics dashboard, dispute bond |
| **Platinum** | 5,000 ZYN | Top placement, 0.5% fees, API access, cross-app promotion, governance weight |

## 6.2 Stake-to-Sell Model

Merchants must maintain a ZYN stake proportional to their monthly sales volume:

$$
RequiredStake_j = \max(TierMinimum, \ MonthlySalesPi_j \times 0.1)
$$

If a merchant's sales exceed their stake tier, they must upgrade within 7 days or face temporary fee increases (2× standard rate) until compliance.

## 6.3 Merchant Engagement Score

Merchant quality is scored on-chain:

| Metric | Weight | Measurement |
|---|---|---|
| **Fulfillment Rate** | 30% | % of orders delivered on time |
| **Customer Rating** | 25% | Average review score (1-5) |
| **Response Time** | 20% | Average time to respond to inquiries |
| **Dispute Resolution** | 15% | % of disputes resolved in customer's favor |
| **Return Rate** | 10% | % of orders returned |

Merchants with scores below 3.0 face stake slashing (5% of stake burned per quarter).

## 6.4 Dispute Resolution Bond

Merchants staking at Gold tier or above have a **dispute bond** — a portion of their stake (10%) that can be awarded to buyers in dispute resolutions. This:
- Incentivizes quality service
- Provides buyer protection without centralized arbitration
- Creates trust in the marketplace

Next: [`7-Loyalty-Rewards`](7-loyalty-rewards.md)
