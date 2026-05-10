# 2. Real-Time Fraud Detection (FraudGuard)

## 2.1 Problem

The Pi ecosystem faces fraud patterns that manual review cannot scale to handle:
- **Wash Trading**: Same users buying/selling to inflate volume
- **Review Farming**: Coordinated fake positive reviews
- **Sybil Attacks**: Mass account creation to manipulate trust scores or jury selection
- **Refund Abuse**: Repeated purchase-cancel-refund cycles

## 2.2 Model: XGBoost Classifier

**Input Features** (per user, rolling 30-day window):

| Feature | Type | Description |
|---|---|---|
| `trade_partner_concentration` | float | % of trades with top 3 partners (high = suspicious) |
| `review_velocity` | float | Reviews per day (spike = suspicious) |
| `review_sentiment_variance` | float | Variance in review text sentiment (low = bot-like) |
| `account_age_days` | int | Days since first on-chain action |
| `stake_amount` | int | ZYN staked (low stake + high activity = suspicious) |
| `dispute_filing_rate` | float | % of orders disputed (high = potential abuse) |
| `cancellation_rate` | float | % of orders cancelled after purchase |
| `cross_app_velocity` | float | Rate of cross-app transfers (unusual patterns) |
| `time_between_actions_std` | float | Std dev of time between actions (low = bot-like) |
| `ip_diversity_score` | float | Number of distinct sessions (low = single device) |

**Output**:
- `fraud_score`: 0.0 - 1.0 (probability of fraudulent behavior)
- `fraud_type`: Enum (WashTrading / ReviewFarming / Sybil / RefundAbuse / None)
- `confidence`: 0.0 - 1.0
- `flagged_features`: List of features that triggered the score

## 2.3 Detection Flow

```
Transaction Event → Feature Extraction → FraudGuard Model → Score
       │                                                    │
       │                                                    ▼
       │                                          ┌────────────────┐
       │                                          │  score < 0.3   │ → Pass (no action)
       │                                          │ 0.3 ≤ score < 0.7│ → Flag for review
       │                                          │  score ≥ 0.7   │ → Auto-freeze + alert
       │                                          └────────────────┘
       │                                                    │
       ▼                                                    ▼
  On-Chain Log                                    Council Investigation
  (model_id, input_hash,                          (human confirms or dismisses)
   fraud_score, fraud_type,
   confidence, timestamp)
```

## 2.4 Auto-Freeze Rules

When `fraud_score >= 0.7`:
1. User's ZYN allowances are **temporarily frozen** (cannot spend)
2. Active subscriptions enter **monitoring mode** (still charged, but flagged)
3. Marketplace listings **hidden** from search
4. Council receives alert with evidence
5. Council has **48 hours** to confirm or dismiss
6. If confirmed: trust score reset + stake slash
7. If dismissed: freeze lifted + false positive logged for model improvement

## 2.5 Model Performance Targets

| Metric | Target | Measurement |
|---|---|---|
| **Precision** | ≥ 95% | Of flagged accounts, % that are actually fraudulent |
| **Recall** | ≥ 80% | Of actual fraud, % that is detected |
| **False Positive Rate** | ≤ 2% | Of legitimate users, % incorrectly flagged |
| **Latency** | ≤ 100ms | Time from transaction to fraud score |
| **Retrain Time** | ≤ 30 min | Daily model retrain duration |

Next: [`3-Dynamic-Pricing`](3-dynamic-pricing.md)
