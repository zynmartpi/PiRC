# 3. Participation Window (Stake → Commerce Engagement → ZYNPower)

During the participation window, users stake Pi to receive **ZYNPower**, which determines the maximum number of ZYN tokens they can commit to buy. Unlike PiRC1's generic engagement, ZYN measures **commerce-specific engagement**.

## 3.1 ZYNPower Calculation

$$
ZYNPower_i = T_{available} \times \left(\frac{StakedPi_i}{\sum StakedPi} + ZYNPower_{Baseline} + CommerceBonus_i\right)
$$

Where:
- $StakedPi_i$ = amount of Pi user $i$ has staked
- $T_{available}$ = total ZYN tokens available for participants
- $ZYNPower_{Baseline}$ = baseline power for long-term lockers (same as PiRC1)
- $CommerceBonus_i$ = bonus power from commerce engagement (NEW)

### 3.1.1 Commerce Bonus (New)

Commerce engagement is scored across multiple dimensions:

| Action | Points | Cap |
|---|---|---|
| Purchase completed on ZynMart | 10 pts | 500 pts |
| Product review written | 5 pts | 100 pts |
| Referral that converts | 25 pts | 250 pts |
| Merchant registration | 50 pts | 50 pts |
| Spin wheel participation | 2 pts | 20 pts |
| Cross-app ZYN usage | 15 pts | 150 pts |

$$
CommerceBonus_i = \frac{EngagementPoints_i}{\sum EngagementPoints} \times 0.15 \times T_{available}
$$

This means **commerce engagement can boost your ZYNPower by up to 15%** of available tokens — rewarding real marketplace activity over passive staking.

## 3.2 Merchant Participation (New)

Merchants have a separate participation track:

- **Merchant Stake Requirement**: Merchants must stake Pi equal to **2× their average monthly sales** in Pi.
- **Merchant ZYNPower**: Calculated separately, ensuring merchants receive ZYN for staking and operational engagement.
- **Merchant Engagement Score**: Based on order fulfillment rate, customer satisfaction, response time, and dispute resolution.

## 3.3 Long-Term Locker Baseline

Same as PiRC1:
- Users with active Pi lockup of ≥90% of mined tokens for ≥3 years (launches before Feb 20, 2026) receive $ZYNPower_{Baseline}$.
- This prevents future account exploitation while rewarding long-term believers.

## 3.4 Engagement Ranking & Price Effect

At window close, participants are ranked by engagement score. This ranking affects the **effective price** of ZYN tokens:

| Engagement Rank | Price Discount |
|---|---|
| Top 10% | 5% discount |
| Top 25% | 3% discount |
| Top 50% | 1% discount |
| Bottom 50% | List price |

This incentivizes genuine marketplace participation rather than passive staking.

Next: [`4-Allocation`](4-allocation/4-allocation.md)
