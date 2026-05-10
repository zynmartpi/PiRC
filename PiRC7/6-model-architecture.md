# 6. Model Architecture & Deployment

## 6.1 Infrastructure

```
┌──────────────────────────────────────────────────┐
│              AI INFRASTRUCTURE                     │
│                                                    │
│  ┌────────────┐    ┌────────────┐    ┌──────────┐│
│  │  Feature   │    │  Model     │    │Prediction ││
│  │  Store     │    │  Registry  │    │  Logger   ││
│  │ (Firestore)│    │ (MLflow)   │    │ (Soroban) ││
│  └─────┬──────┘    └─────┬──────┘    └─────┬─────┘│
│        │                 │                 │      │
│        ▼                 ▼                 ▼      │
│  ┌──────────────────────────────────────────────┐ │
│  │           TRAINING PIPELINE                   │ │
│  │  On-chain events → ETL → Features → Train    │ │
│  │  → Validate → Deploy (shadow mode) → Promote │ │
│  └──────────────────────────────────────────────┘ │
│                                                    │
│  ┌──────────────────────────────────────────────┐ │
│  │           SERVING PIPELINE                    │ │
│  │  Event → Feature extraction → Inference      │ │
│  │  → Log prediction on-chain → Action (if any) │ │
│  └──────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────┘
```

## 6.2 Model Lifecycle

| Stage | Description | Duration |
|---|---|---|
| **Development** | Train on historical data, test on held-out set | 1-2 weeks |
| **Shadow Mode** | Deploy alongside current model, compare predictions (no real actions) | 2 weeks |
| **Canary** | Serve 10% of traffic, monitor for regressions | 1 week |
| **Production** | Full deployment with monitoring | Ongoing |
| **Retirement** | Replace with new model version, archive old version | 1 day |

## 6.3 On-Chain Prediction Logging

Every AI prediction is logged on-chain for auditability:

```rust
fn log_prediction(env: Env, model_id: String, input_hash: BytesN<32>,
                  prediction: String, confidence: f32, action_taken: String) {
    let log = PredictionLog {
        timestamp: env.ledger().timestamp(),
        model_id,
        input_hash,
        prediction,
        confidence,
        action_taken,
    };
    write_prediction_log(&env, &log);

    env.events().publish(
        (Symbol::new("zyn"), Symbol::new("ai"), Symbol::new("prediction")),
        (log.timestamp.to_val(), log.model_id.to_val(), log.confidence.to_val())
    );
}
```

This ensures **every AI decision is auditable** — anyone can verify what the model predicted and what action was taken.

## 6.4 Model Performance Monitoring

| Metric | Alert Threshold | Action |
|---|---|---|
| **Accuracy Drop** | >5% below baseline | Auto-rollback to previous model |
| **Latency Increase** | >2× normal | Scale up serving infrastructure |
| **Prediction Drift** | Distribution shift >10% | Trigger emergency retrain |
| **False Positive Spike** | >2× normal | Reduce model sensitivity + investigate |

Next: [`7-Data-Pipeline`](7-data-pipeline.md)
