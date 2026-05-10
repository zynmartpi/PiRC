# 3. Dynamic Marketplace Pricing (PriceEngine)

## 3.1 Problem

Fixed listing boost prices don't reflect actual demand — popular categories are underpriced, niche categories overpriced. Dynamic pricing creates fair market conditions.

## 3.2 Model: Reinforcement Learning Agent

The PriceEngine agent learns optimal boost pricing through interaction with the marketplace:

- **State**: Current demand per category, merchant density, time of day, recent transaction volume
- **Action**: Set boost price for each tier (Basic/Featured/Premium/Spotlight)
- **Reward**: Revenue + merchant satisfaction + buyer conversion rate

## 3.3 Pricing Factors

| Factor | Weight | Source |
|---|---|---|
| **Category Demand** | 30% | Search volume + purchase volume per category |
| **Merchant Density** | 20% | Number of merchants in category (more = higher competition = higher price) |
| **Time of Day** | 15% | Peak hours cost more, off-peak discounts |
| **Trust Score** | 15% | Higher trust merchants get 10-20% discount on boosts |
| **Historical CTR** | 10% | Click-through rate of boosted listings in this category |
| **Conversion Rate** | 10% | Purchase rate of boosted listings |

## 3.4 Price Bounds

Dynamic pricing has hard bounds to prevent exploitation:

| Tier | Min Price (ZYN) | Max Price (ZYN) | Default |
|---|---|---|---|
| **Basic** | 5 | 20 | 10 |
| **Featured** | 25 | 100 | 50 |
| **Premium** | 100 | 400 | 200 |
| **Spotlight** | 250 | 1000 | 500 |

Prices adjust within these bounds based on the RL agent's recommendations, updated every 6 hours.

## 3.5 Merchant Transparency

Merchants always see:
- **Current boost price** for their category
- **Price trend** (rising/falling/stable)
- **Expected visibility** at each price tier
- **Historical performance** of their boosted listings

No hidden algorithm — pricing logic and factors are public.

Next: [`4-Anomaly-Detection`](4-anomaly-detection.md)
