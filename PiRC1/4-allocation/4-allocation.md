# 4. Allocation Period

## 4.1 Hybrid Allocation: Fixed-Price + Engagement-Weighted + Merchant Reserve

ZYN uses a **three-phase allocation** combining the best elements of PiRC1's two designs:

### Phase 1: Fixed-Price Portion (60% of participant allocation)

All participants with ZYNPower can commit Pi to buy ZYN at a fixed list price:

$$
p_{list} = \frac{C_{commit}}{T_{fixed}}
$$

Where $C_{commit}$ = total Pi committed by participants, $T_{fixed}$ = tokens in the fixed-price portion.

**Oversubscription handling**: If total commitments exceed available tokens, allocation is proportional to ZYNPower.

### Phase 2: Engagement-Weighted Allocation (25% of participant allocation)

The remaining tokens are distributed based on engagement ranking:

- Top 10% engagement: receive 1.5× their proportional share
- Top 25%: receive 1.3×
- Top 50%: receive 1.1×
- Bottom 50%: receive 0.8× (excess redistributed to top engagers)

This ensures **active marketplace participants receive more tokens** than passive stakers.

### Phase 3: Merchant Reserve Allocation (15% of total supply)

Merchants receive ZYN tokens proportional to their stake and engagement:

$$
MerchantAllocation_j = T_{merchant} \times \left(\frac{MerchantStake_j}{\sum MerchantStake} \times 0.6 + \frac{MerchantScore_j}{\sum MerchantScore} \times 0.4\right)
$$

Merchant tokens are **locked for 6 months** with linear release, ensuring merchants remain committed.

## 4.2 LP Formation

The Escrow Wallet deposits into the Liquidity Pool:
- **All committed Pi** ($C$) from participants
- **Project's liquidity tokens** ($T_{liquidity} = 25\%$ of total supply)

The Escrow Wallet is **permanently locked** — no withdrawal capability, same as PiRC1.

## 4.3 Burn Mechanism (New)

Starting from TGE, **0.5% of every ZYN transaction is burned**:

$$
BurnAmount = 0.005 \times TransactionAmount
$$

This creates a **deflationary supply** that increases as marketplace activity grows. The burn is:
- Automatic (enforced by smart contract)
- Transparent (all burns are on-chain events)
- Capped at 50% of total supply (prevents over-deflation)

Next: [`5-TGE-State`](../5-tge-state.md)
