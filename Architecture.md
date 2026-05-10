# ZYN Ecosystem Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         ZYN ECOSYSTEM                                │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │                     USER LAYER                                  │ │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐      │ │
│  │  │  ZynMart │  │  App B   │  │  App C   │  │  App D   │      │ │
│  │  │  (Web)   │  │ (Mobile) │  │ (Mobile) │  │ (Mobile) │      │ │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘      │ │
│  └───────┼──────────────┼──────────────┼──────────────┼──────────┘ │
│          │              │              │              │              │
│  ┌───────▼──────────────▼──────────────▼──────────────▼──────────┐ │
│  │                    SDK LAYER (JS/TS)                            │ │
│  │  zyn-sdk: wallet connect, transaction builder, event listener │ │
│  └──────────────────────────┬────────────────────────────────────┘ │
│                             │                                        │
│  ┌──────────────────────────▼────────────────────────────────────┐ │
│  │                 SYNC LAYER (Firestore Cache)                    │ │
│  │  Balance cache, transaction history, trust score mirror        │ │
│  └──────────────────────────┬────────────────────────────────────┘ │
│                             │                                        │
│  ┌──────────────────────────▼────────────────────────────────────┐ │
│  │               SMART CONTRACT LAYER (Soroban)                    │ │
│  │                                                                │ │
│  │  ┌───────────┐  ┌───────────┐  ┌──────────────────┐          │ │
│  │  │ zyn_token │◄─┤ zyn_wallet│──┤ zyn_subscription │          │ │
│  │  └─────┬─────┘  └─────┬─────┘  └──────────────────┘          │ │
│  │        │               │                                       │ │
│  │        │    ┌───────────▼───────────┐                          │ │
│  │        │    │    zyn_marketplace    │                           │ │
│  │        │    └───────────┬───────────┘                          │ │
│  │        │                │                                       │ │
│  │        │    ┌───────────▼───────────┐                          │ │
│  │        └───►│      zyn_trust         │                          │ │
│  │             └───────────────────────┘                          │ │
│  └────────────────────────────────────────────────────────────────┘ │
│                             │                                        │
│  ┌──────────────────────────▼────────────────────────────────────┐ │
│  │                   AI LAYER (PiRC7)                              │ │
│  │  FraudGuard · PriceEngine · AnomalyScout · ChurnPredictor     │ │
│  │  DisputeTriage · DemandForecaster                             │ │
│  └────────────────────────────────────────────────────────────────┘ │
│                                                                      │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │                  GOVERNANCE LAYER                               │ │
│  │  ZYN holder voting · Council · Proposal execution              │ │
│  └────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

## Data Flow

### Purchase Flow

```
Buyer                    SDK                   Soroban               Firestore
  │                       │                       │                      │
  │──Click "Buy"────────▶│                       │                      │
  │                       │──initiate_purchase()──▶│                      │
  │                       │                       │──escrow ZYN────────▶│ (balance -X)
  │                       │                       │                      │
  │                       │◀──order_id────────────│                      │
  │◀──Order confirmed─────│                       │                      │
  │                       │                       │                      │
  │                       │◀──event:order_created─│──────────────────────│
  │                       │                       │                      │
  │                       │                       │   [Merchant ships]  │
  │                       │                       │                      │
  │──Confirm delivery────▶│──confirm_delivery()──▶│                      │
  │                       │                       │──distribute_fees──▶│ (merchant +X)
  │                       │                       │──burn 0.5%────────▶│ (supply -Y)
  │                       │◀──event:delivered─────│──────────────────────│
  │◀──Receipt─────────────│                       │                      │
```

### Subscription Flow

```
Subscriber              SDK                   Soroban               Firestore
  │                       │                       │                      │
  │──Subscribe───────────▶│──subscribe()─────────▶│                      │
  │                       │                       │──approve allowance──▶│
  │                       │                       │──charge first period▶│
  │                       │◀──sub_id──────────────│──────────────────────│
  │◀──Subscription active─│                       │                      │
  │                       │                       │                      │
  │   [30 days later]     │                       │                      │
  │                       │◀──cron:process()──────│                      │
  │                       │                       │──charge next period─▶│
  │                       │                       │                      │
  │   [Payment fails]     │                       │                      │
  │                       │                       │──enter grace (3d)──▶│ (flag: grace)
  │◀──"Top up" alert──────│                       │                      │
  │                       │                       │                      │
  │──Top up balance──────▶│                       │──retry charge──────▶│ (grace cleared)
  │◀──Subscription saved──│                       │                      │
```

### Dispute Flow

```
Buyer               Merchant            Jury (5)            Council
  │                     │                   │                   │
  │──file_dispute()────▶│                   │                   │
  │                     │                   │                   │
  │                     │◀──respond()───────│                   │
  │                     │                   │                   │
  │                     │   [5 jurors selected randomly]       │
  │                     │                   │                   │
  │                     │                   │──vote()──────────▶│
  │                     │                   │──vote()──────────▶│
  │                     │                   │──vote()──────────▶│
  │                     │                   │                   │
  │                     │                   │   [3/5 majority]  │
  │◀──Refund + burn─────│──stake slashed───│──jurors rewarded──│
  │                     │                   │                   │
  │   [Either party can appeal with 50 ZYN deposit]           │
  │                     │                   │                   │
  │                     │                   │   [7 new jurors]  │
  │                     │                   │──final vote──────▶│
  │◀──Final decision────│◀──Final decision─│◀──Executed────────│
```

## PiRC Coverage Map

| Layer | PiRC1 | PiRC2 | PiRC3 | PiRC4 | PiRC5 | PiRC6 | PiRC7 |
|---|---|---|---|---|---|---|---|
| **Token Design** | ✅ | | | | | ✅ | |
| **Subscriptions** | | ✅ | | | | ✅ | ✅ |
| **Marketplace** | | | ✅ | | | ✅ | ✅ |
| **Reputation** | | | | ✅ | | ✅ | ✅ |
| **Wallet Interop** | | | | | ✅ | ✅ | |
| **Implementation** | | | | | | ✅ | |
| **AI Intelligence** | | | | | | | ✅ |

## Technology Stack

| Component | Technology | Purpose |
|---|---|---|
| **Smart Contracts** | Rust + Soroban SDK 21.x | On-chain logic |
| **Backend Sync** | Node.js + Firestore SDK | Event processing + cache |
| **AI Models** | Python (XGBoost, PyTorch, Prophet) | Fraud, pricing, prediction |
| **Frontend SDK** | TypeScript + Pi SDK | App integration |
| **CI/CD** | GitHub Actions + Soroban CLI | Build, test, deploy |
| **Monitoring** | Grafana + Prometheus | System health + anomaly alerts |
| **Model Registry** | MLflow | AI model versioning + deployment |
| **Feature Store** | Firestore + BigQuery | ML feature storage + analytics |
