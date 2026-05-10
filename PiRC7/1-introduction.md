# 1. Introduction

PiRC7 brings **practical AI** to the ZYN ecosystem. Every model is designed for a specific problem, trained on on-chain data, and deployed with measurable outcomes.

## 1.1 Why Practical AI

PiRC7 takes a **problem-first** approach to AI integration:

| Principle | Implementation |
|---|---|
| **Specific Models** | Each AI model solves one specific problem — no vague "intelligence layer" |
| **On-Chain Data** | All models train on verifiable on-chain events + Firestore analytics |
| **Auditable Predictions** | Every prediction is logged on-chain with confidence scores |
| **Privacy-First** | Anonymized, aggregated data only — no PII in the pipeline |
| **Human Override** | AI advises, humans decide — no autonomous fund freezing or slashing |
| **Production Pipeline** | Shadow → Canary → Production deployment with A/B testing |

## 1.2 AI Service Architecture

```
┌─────────────────────────────────────────────────────┐
│                  AI SERVICE LAYER                      │
│                                                       │
│  ┌─────────────┐  ┌─────────────┐  ┌──────────────┐ │
│  │   Fraud     │  │  Dynamic    │  │   Anomaly    │ │
│  │  Detection  │  │  Pricing    │  │  Detection   │ │
│  │  (XGBoost)  │  │  (RL Agent) │  │  (Isolation  │ │
│  │             │  │             │  │   Forest)    │ │
│  └──────┬──────┘  └──────┬──────┘  └──────┬───────┘ │
│         │                │                │           │
│  ┌──────▼────────────────▼────────────────▼───────┐ │
│  │            PREDICTION LOG (On-Chain)             │ │
│  │  Every prediction: model_id + input_hash +      │ │
│  │  confidence + timestamp → immutable audit trail  │ │
│  └──────────────────────┬──────────────────────────┘ │
│                         │                             │
│  ┌──────────────────────▼──────────────────────────┐ │
│  │            DATA PIPELINE                          │ │
│  │  On-chain events → ETL → Feature Store → Models  │ │
│  └──────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

## 1.3 Model Registry

| Model | Type | Purpose | Training Data | Update Frequency |
|---|---|---|---|---|
| **FraudGuard** | XGBoost Classifier | Detect wash trading, review farming, Sybil patterns | Transaction + review + dispute history | Daily retrain |
| **PriceEngine** | Reinforcement Learning | Optimize listing boost pricing by category/demand | Marketplace transaction volume | Continuous learning |
| **AnomalyScout** | Isolation Forest | Detect unusual patterns in real-time | Rolling 30-day transaction windows | Hourly retrain |
| **ChurnPredictor** | Logistic Regression | Predict subscription cancellation | Subscription lifecycle data | Weekly retrain |
| **DisputeTriage** | BERT-based NLP | Pre-score dispute evidence and reason text | Historical dispute outcomes | Monthly retrain |
| **DemandForecaster** | Prophet (Time Series) | Predict category demand per region | Daily sales volume by category | Daily retrain |

## 1.4 Key Principle: AI Assists, Humans Decide

AI models in PiRC7 are **advisory, not authoritative**:

- Fraud detection flags suspicious activity → **human review confirms**
- Dynamic pricing suggests boost costs → **merchant chooses**
- Churn prediction identifies at-risk subscribers → **grace period offer sent, subscriber decides**
- Dispute triage scores evidence → **jurors still vote**
- Anomaly detection alerts → **council investigates**

No AI model can autonomously freeze funds, cancel subscriptions, or slash stakes.

Next: [`2-Fraud-Detection`](2-fraud-detection.md)
