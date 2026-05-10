# 2. Overview

```
flowchart TD
  subgraph Merchant["Merchant Actions"]
    M1[List Product] --> M2[Stake ZYN]
    M2 --> M3[Submit Delivery Proof]
  end

  subgraph Buyer["Buyer Actions"]
    B1[Browse & Search] --> B2[Initiate Purchase]
    B2 --> B3[Confirm Receipt / Dispute]
  end

  subgraph Contract["Smart Contract"]
    E[Escrow] --> R[Release]
    E --> D[Dispute Hold]
    R --> F[Fee Distribution]
    F --> B1_[Burn 0.5%]
    F --> LP_[LP 1%]
    F --> T_[Treasury 0.5%]
  end

  subgraph Reputation["Reputation System"]
    S[Trust Score Update]
    S --> V[Visibility Weight]
    S --> FR[Fee Rate Adjustment]
  end

  M2 -->|stake| E
  B2 -->|payment| E
  M3 -->|proof| R
  B3 -->|confirm| R
  B3 -->|dispute| D
  R --> S
```

## Key Concepts

- **Product Listings**: On-chain product data with merchant stake commitment
- **Escrow Payments**: Smart contract holds funds until delivery confirmed
- **Delivery Proof**: Merchant submits on-chain proof (tracking hash, photo hash)
- **Auto-Confirmation**: 7-day timeout protects merchants from buyer silence
- **Dispute Resolution**: Community jury system with stake-weighted voting
- **Reputation System**: Trust scores affect visibility, fees, and dispute outcomes
- **Fee Distribution**: Transparent on-chain fee split (burn + LP + treasury)

Next: [`3-Listing-System`](3-listing-system.md)
