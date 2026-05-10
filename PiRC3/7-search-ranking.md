# 7. Search & Ranking Algorithm

## 7.1 Ranking Formula

Product search results are ranked using a transparent on-chain formula:

$$
RankScore = (TrustScore \times 0.3) + (StakeWeight \times 0.25) + (SalesVolume \times 0.2) + (ReviewScore \times 0.15) + (Recency \times 0.1)
$$

| Factor | Weight | Source |
|---|---|---|
| **Trust Score** | 30% | On-chain reputation (PiRC4) |
| **Stake Weight** | 25% | ZYN staked on listing |
| **Sales Volume** | 20% | Completed orders count |
| **Review Score** | 15% | Average buyer rating |
| **Recency** | 10% | How recently listing was active |

## 7.2 Anti-Manipulation

- **Sybil Resistance**: Trust scores are weighted by account age and transaction history
- **Review Authenticity**: Only verified buyers can leave reviews
- **Stake Cannot Override**: Even max stake can't push a 1-star product to top
- **Decay Factor**: Stake weight decays 1% per day, requiring periodic renewal

## 7.3 Category-Specific Boosts

Certain categories receive ranking boosts based on governance votes:

- **Essential Goods**: +20% boost (food, medicine, basics)
- **Digital Goods**: +10% boost (software, content)
- **Local Commerce**: +15% boost (services, delivery)
- **New Merchants**: +30% boost for first 30 days (onboarding incentive)

Next: [`8-Data-Types`](8-data-types.md)
