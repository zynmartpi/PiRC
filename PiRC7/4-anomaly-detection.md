# 4. Anomaly Detection (AnomalyScout)

## 4.1 Problem

Even without specific fraud, unusual patterns can indicate emerging threats: coordinated attacks, market manipulation, or system exploits.

## 4.2 Model: Isolation Forest

Isolation Forest detects anomalies by isolating outliers — anomalous data points require fewer splits to isolate.

**Monitored Metrics** (per category, per app, per time window):

| Metric | Window | Normal Range | Alert Threshold |
|---|---|---|---|
| **Transaction Volume** | 1 hour | ±2σ from 7-day average | >3σ |
| **New Accounts** | 1 hour | ±2σ | >4σ |
| **Review Count** | 1 hour | ±2σ | >3σ |
| **Dispute Filing Rate** | 1 hour | ±2σ | >3σ |
| **Cross-App Transfer Volume** | 1 hour | ±2σ | >4σ |
| **Token Burn Rate** | 1 hour | ±1.5σ | >3σ |
| **Staking Changes** | 1 hour | ±2σ | >4σ |
| **Subscription Signups** | 1 hour | ±2σ | >3σ |

## 4.3 Alert Levels

| Level | Condition | Response |
|---|---|---|
| **Green** | All metrics within 2σ | Normal — no action |
| **Yellow** | 1-2 metrics exceed 3σ | Log + notify council |
| **Orange** | 3+ metrics exceed 3σ OR 1 metric exceeds 4σ | Temporary rate limits + council review |
| **Red** | 5+ metrics exceed 3σ OR 3+ metrics exceed 4σ | Emergency pause + full investigation |

## 4.4 Response Automation

| Alert Level | Auto-Response | Human Response |
|---|---|---|
| **Green** | None | None |
| **Yellow** | Increase monitoring frequency to 15 min | Council reviews within 24 hours |
| **Orange** | Rate limit: 50% reduction in transaction limits | Council reviews within 4 hours |
| **Red** | Pause all non-essential contract operations | Council emergency session within 1 hour |

## 4.5 Self-Healing

After an anomaly is resolved:
1. Root cause is documented
2. If new pattern, added to model training data
3. Detection rules updated if needed
4. Rate limits gradually relaxed over 24 hours
5. Post-mortem published to community

Next: [`5-Predictive-Analytics`](5-predictive-analytics.md)
