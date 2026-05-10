# 5. Cross-App Trust Portability

## 5.1 Universal Trust Score

A user's trust score is **portable across all Pi apps** that integrate the PiRC4 protocol. This means:

- A trusted seller on ZynMart carries their reputation to any other marketplace
- A trusted buyer on one app gets benefits on all apps
- Bad behavior on one app affects trust across the ecosystem

## 5.2 Trust Score Composition

```
┌───────────────────────────────────────────┐
│           UNIVERSAL TRUST SCORE            │
│                                           │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐    │
│  │ ZynMart │ │ App B   │ │ App C   │    │
│  │ Score   │ │ Score   │ │ Score   │    │
│  │ (0-5)   │ │ (0-5)   │ │ (0-5)   │    │
│  └────┬────┘ └────┬────┘ └────┬────┘    │
│       │           │           │          │
│       └───────────┴───────────┘          │
│                   │                      │
│         Weighted Average                 │
│         (by transaction volume)          │
│                   │                      │
│         ┌───────▼────────┐               │
│         │ Universal Score │              │
│         │    (0 - 5.0)    │              │
│         └────────────────┘               │
└───────────────────────────────────────────┘
```

## 5.3 App-Specific vs Universal

$$
UniversalScore = \frac{\sum_{app}(AppScore_{app} \times TransactionVolume_{app})}{\sum TransactionVolume_{app}}
$$

- Apps with more transaction history carry more weight
- New apps start with equal weight until they accumulate volume
- Universal score is always between the highest and lowest app-specific scores

## 5.4 Integration Requirements

For an app to participate in the universal trust system:

1. **Implement PiRC4 Contract**: Deploy or connect to the trust score contract
2. **Report Events**: Submit on-chain events for transactions, reviews, disputes
3. **Verify Users**: Ensure users are authenticated via Pi SDK
4. **Stake ZYN**: Minimum 100 ZYN stake to participate (prevents spam apps)

## 5.5 Trust Score API

Apps can query the universal trust score:

#### `get_universal_trust(user) -> TrustInfo`

| Field | Type | Description |
|---|---|---|
| `universal_score` | `f32` | Weighted average across apps |
| `app_scores` | `Vec<AppScore>` | Per-app breakdown |
| `total_transactions` | `u64` | Total on-chain transactions |
| `account_age_days` | `u32` | Days since first on-chain action |
| `stake_amount` | `i128` | Total ZYN staked |

Next: [`6-Anti-Manipulation`](6-anti-manipulation.md)
