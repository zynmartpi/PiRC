# 6. Anti-Manipulation Measures

## 6.1 Sybil Attack Prevention

**Problem**: Creating many accounts to boost ratings or manipulate dispute juries.

**Solutions**:

| Attack Vector | Defense |
|---|---|
| **Mass Account Creation** | Account age + stake weight in review scoring |
| **Self-Review** | Only verified on-chain buyers can review (contract enforced) |
| **Review Swapping** | Cross-app detection: same users reviewing each other flagged |
| **Jury Packing** | Random jury selection from global ZYN staker pool |
| **Wash Trading** | Same buyer/seller patterns detected and flagged |

## 6.2 Review Manipulation Prevention

- **Verified Purchase Only**: Contract checks that `order_id` exists and buyer is the purchaser
- **One Review Per Order**: Each order can only receive one review per dimension
- **Review Window**: Reviews only accepted within 30 days of delivery confirmation
- **Weighted Impact**: Reviewer's trust score determines how much their review affects the merchant
- **Anomaly Detection**: Sudden spikes in reviews (positive or negative) trigger audit

## 6.3 Dispute Manipulation Prevention

- **Random Jury**: Jurors selected randomly from all eligible stakers
- **Jury Rotation**: No juror can serve on consecutive disputes for same merchant
- **Vote Privacy**: Juror votes are hidden until all votes cast (prevents herding)
- **Stake Penalty**: Jurors who vote against majority consistently lose stake

## 6.4 Stake-Based Weighting

All reputation actions are weighted by stake:

$$
ActionWeight = \min\left(1.0,\ \frac{UserStake}{1000}\right)
$$

- Users with ≥1000 ZYN stake have full weight (1.0)
- Users with 100 ZYN stake have 0.1 weight
- Users with 0 stake have 0.01 weight (can still participate, but minimally)

This ensures that **committed participants have more influence** than casual or malicious accounts.

## 6.5 Audit & Reporting

- **Weekly Audit**: Contract automatically flags suspicious patterns
- **Community Reporting**: Any user can report suspected manipulation (requires 50 ZYN deposit)
- **Investigation**: Governance council reviews flagged accounts
- **Penalties**: Confirmed manipulation results in trust score reset to 0.0 + stake slash

Next: [`7-Data-Types`](7-data-types.md)
