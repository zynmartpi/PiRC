# 5. Official TGE / Market Opening

When the allocation rollout ends, the LP/market opens for unrestricted access. This event constitutes the official Token Generation Event (TGE).

## 5.1 LP Situation at TGE

The Escrow Wallet seeds the LP using the **deposit** operation:

- LP at TGE contains **all committed Pi** ($C$) and the project's liquidity bucket $T_{liquidity} = 0.25T$.
- The Escrow Wallet is **permanently locked** — cannot withdraw initial liquidity.
- At TGE, the LP contains **~25% of the project's circulating supply** as permanent liquidity.

### Enhanced Price Floor Protection

Unlike PiRC1 where the floor was ~23.8% of list price, ZYN introduces two additional floor-support mechanisms:

**1. Merchant Staking Buy Pressure**

Merchants are required to stake ZYN proportional to their sales volume. This creates continuous buy pressure:

$$
MerchantStakeRequirement = MonthlySalesPi \times 0.1 \times p_{ZYN}
$$

**2. Transaction Burn Deflation**

0.5% of every transaction is burned, permanently reducing supply:

$$
Supply_{t} = Supply_{TGE} - \sum_{i=1}^{t} Burn_i
$$

**3. Combined Floor Analysis**

With merchant staking and burn mechanism, the effective floor is strengthened:

$$
p_{floor}^{ZYN} \approx 0.30 \times p_{list}
$$

This is **~26% higher** than PiRC1 Design 1's floor of $0.238 \times p_{list}$, because:
- Merchant staking absorbs sell pressure (they're buying, not selling)
- Burn reduces circulating supply over time
- Commerce utility creates natural demand floor

## 5.2 Post-TGE Utility Activation

Immediately after TGE, ZYN activates its full utility stack:

| Utility | Activation | Description |
|---|---|---|
| **Marketplace Discounts** | TGE + 0 | 5-15% discount on ZynMart purchases paid in ZYN |
| **Seller Fee Reduction** | TGE + 0 | Merchants staking ZYN pay 50% lower listing fees |
| **Governance Voting** | TGE + 30 days | ZYN holders vote on marketplace policies |
| **Cross-App Payments** | TGE + 60 days | ZYN accepted in partner Pi apps |
| **Staking Rewards** | TGE + 90 days | ZYN stakers earn yield from transaction fees |
| **Advanced Governance** | TGE + 180 days | On-chain proposals, treasury allocation votes |

## 5.3 Unlock Schedule

| Group | TGE | 3 months | 6 months | 1 year | 2 years | 4 years |
|---|---|---|---|---|---|---|
| **Launchpad Participants** | 25% | 50% | 75% | 100% | — | — |
| **Merchant Reserve** | 0% | 0% | 17% | 50% | 75% | 100% |
| **Ecosystem Development** | 0% | 10% | 25% | 50% | 75% | 100% |
| **Team** | 0% | 0% | 0% | 0% | 25% | 100% (1yr cliff + 3yr vest) |
| **Liquidity Pool** | 100% | — | — | — | — | — |

**Key improvement over PiRC1**: Team tokens have a 1-year cliff (nothing for first year) + 3-year vesting, ensuring long-term alignment.

Next: [`6-Merchant-Integration`](6-merchant-integration.md)
