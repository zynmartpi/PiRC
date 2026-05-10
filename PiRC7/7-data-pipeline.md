# 7. Data Pipeline

## 7.1 ETL Architecture

```
┌────────────┐    ┌────────────┐    ┌────────────┐    ┌────────────┐
│  Soroban   │    │  Event     │    │  Feature   │    │   Model    │
│  Events    │───▶│  Processor │───▶│  Store     │───▶│  Training  │
│  (On-Chain)│    │  (Node.js) │    │ (Firestore)│    │  (Python)  │
└────────────┘    └────────────┘    └────────────┘    └────────────┘
       │                │                  │                  │
       │                ▼                  ▼                  ▼
       │          Parse & Validate    Aggregate & Window    Train & Validate
       │          Classify Event      Compute Features     Deploy Shadow
       │          Store Raw Event     Update Rolling       Monitor Performance
       │                               Windows
```

## 7.2 Event Processing

```javascript
// Event processor: listens to Soroban events and builds feature vectors
const eventHandlers = {
  'zyn:token:transfer': async (event) => {
    await updateTradeFeatures(event.data.from, event.data.to, event.data.amount);
    await updateVolumeFeatures(event.data.amount, event.timestamp);
  },
  'zyn:subscription:charge': async (event) => {
    await updateSubscriptionFeatures(event.data.subscriber, 'charge_success');
  },
  'zyn:subscription:grace_started': async (event) => {
    await updateSubscriptionFeatures(event.data.subscriber, 'grace_entered');
    await incrementChurnRisk(event.data.subscriber);
  },
  'zyn:marketplace:order_created': async (event) => {
    await updateDemandFeatures(event.data.listing_id, event.timestamp);
  },
  'zyn:trust:score_updated': async (event) => {
    await updateReputationFeatures(event.data.user, event.data.new_score);
  }
};
```

## 7.3 Feature Store Schema

| Feature Group | Key | Update Frequency | Retention |
|---|---|---|---|
| `user_trading_30d` | `{user_id}` | Per transaction | 90 days |
| `user_reviews_30d` | `{user_id}` | Per review | 90 days |
| `category_demand_7d` | `{category_id}` | Per order | 180 days |
| `subscription_health` | `{subscriber_id}` | Per charge event | 365 days |
| `marketplace_volume_1h` | `{category_id}` | Per order | 30 days |
| `global_anomaly_metrics` | `global` | Per event | 30 days |

## 7.4 Data Quality Checks

| Check | Frequency | Action on Failure |
|---|---|---|
| **Null/Empty Features** | Per event | Skip event, log warning |
| **Outlier Detection** | Per batch | Cap at 99th percentile |
| **Feature Drift** | Daily | Alert + trigger retrain |
| **Label Accuracy** | Weekly | Manual audit of 100 samples |
| **Pipeline Latency** | Per event | Alert if >5 seconds |

Next: [`8-Privacy-Model`](8-privacy-model.md)
