# 5. Predictive Analytics

## 5.1 Churn Prediction (ChurnPredictor)

### Problem
Subscribers who cancel could often be retained with a targeted offer. Manual retention doesn't scale.

### Model: Logistic Regression

Simple, interpretable, fast — ideal for binary prediction (will cancel / won't cancel).

**Features**:

| Feature | Description |
|---|---|
| `tenure_days` | Days since subscription started |
| `payment_failures_last_30d` | Failed charges in last 30 days |
| `support_tickets_last_30d` | Support interactions |
| `app_login_frequency` | Logins per week (declining = churn signal) |
| `feature_usage_rate` | % of subscribed features actually used |
| `grace_period_count` | Times entered grace period |
| `alternative_services_available` | Competing services in same category |

**Output**: `churn_probability` (0.0 - 1.0)

### Retention Actions

| Churn Probability | Action | Cost |
|---|---|---|
| < 0.3 | None | 0 ZYN |
| 0.3 - 0.5 | In-app reminder of value | 0 ZYN |
| 0.5 - 0.7 | 10% discount offer for next period | 10% of subscription price |
| 0.7 - 0.9 | 20% discount + 1 free month | 20% + 1 month |
| > 0.9 | Personalized retention offer (merchant decides) | Variable |

All retention offers are **opt-in** — subscriber chooses whether to accept.

## 5.2 Dispute Triage (DisputeTriage)

### Problem
Jurors spend too much time reviewing evidence. AI can pre-score and summarize to speed up decisions.

### Model: BERT-based NLP

Analyzes dispute reason text + evidence descriptions to predict likely outcome.

**Input**: Dispute reason text + evidence hash metadata
**Output**: 
- `predicted_outcome`: BuyerWins / MerchantWins / Partial
- `confidence`: 0.0 - 1.0
- `key_evidence_points`: Extracted key claims from text
- `similar_historical_disputes`: IDs of similar past disputes with outcomes

Jurors still vote independently — triage is advisory only.

## 5.3 Demand Forecasting (DemandForecaster)

### Problem
Merchants don't know what to stock. Overstocking wastes capital; understocking loses sales.

### Model: Prophet (Time Series)

Facebook's Prophet model forecasts demand by category and region.

**Input**: Daily sales volume by category (90-day history minimum)
**Output**: 
- `forecast_7d`: Predicted demand for next 7 days
- `forecast_30d`: Predicted demand for next 30 days
- `confidence_interval`: Lower and upper bounds
- `trend`: Growing / Stable / Declining
- `seasonality`: Day-of-week and monthly patterns

**Merchants see**:
```
Category: Electronics
7-day forecast: 120-150 orders (↑ 12% vs last week)
30-day forecast: 500-600 orders (stable)
Best listing time: Thursday 2-4pm UTC
Recommended stock: 150 units
```

Next: [`6-Model-Architecture`](6-model-architecture.md)
