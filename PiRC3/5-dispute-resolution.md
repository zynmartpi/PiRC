# 5. Dispute Resolution (Marketplace)

## 5.1 Dispute Filing

#### `file_dispute(buyer, order_id, reason, evidence_hash) -> Dispute`

Buyer files dispute within 7 days of delivery confirmation (or auto-confirmation).

| Param | Type | Description |
|---|---|---|
| `order_id` | `u64` | Disputed order |
| `reason` | `DisputeReason` | NotReceived / NotAsDescribed / Damaged / Other |
| `evidence_hash` | `Bytes` | Hash of evidence (photos, messages) |

## 5.2 Community Jury System

Disputes are resolved by a **community jury** of 5 randomly selected ZYN stakers:

1. **Jury Selection**: 5 stakers with ≥100 ZYN stake, randomly selected
2. **Evidence Review**: Both parties submit evidence (3-day window)
3. **Voting**: Jurors vote (2-day window)
4. **Resolution**: Majority wins (3/5 required)

### Juror Incentives

- **Participation Reward**: 5 ZYN per dispute resolved
- **Accuracy Bonus**: +3 ZYN if vote matches majority (incentivizes honest voting)
- **Penalty**: -10 ZYN stake slash for not voting

## 5.3 Dispute Outcomes

| Outcome | Buyer Gets | Merchant Gets | Stake Effect |
|---|---|---|---|
| **Buyer Wins (Full)** | 100% refund | 0 | Merchant: -5% listing stake |
| **Buyer Wins (Partial)** | 50% refund | 50% payment | Merchant: -2% listing stake |
| **Merchant Wins** | 0 | Full payment | Buyer: -0.5 trust score |
| **Mutual Agreement** | Negotiated | Negotiated | No penalty |

## 5.4 Appeal Process

Either party can appeal within 3 days of resolution:
- Appeal requires 50 ZYN deposit
- 7 new jurors selected (higher stake requirement: ≥500 ZYN)
- Appeal decision is final
- Loser's deposit is burned

Next: [`6-Fee-Structure`](6-fee-structure.md)
