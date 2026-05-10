# 2. Architecture

## 2.1 Three-Layer Architecture

```
┌─────────────────────────────────────────────────┐
│                  APP LAYER                        │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐         │
│  │ ZynMart │  │ App B   │  │ App C   │         │
│  │ React/  │  │ Vue/    │  │ Native/ │         │
│  │ Vanilla │  │ Next.js │  │ Flutter │         │
│  └────┬────┘  └────┬────┘  └────┬────┘         │
└───────┼─────────────┼─────────────┼──────────────┘
        │             │             │
┌───────▼─────────────▼─────────────▼──────────────┐
│               SYNC LAYER                          │
│  ┌──────────────────────────────────────────┐    │
│  │           Firestore Cache                 │    │
│  │  • Real-time balance reads               │    │
│  │  • Transaction history cache              │    │
│  │  • User profile cache                    │    │
│  │  • Trust score cache                     │    │
│  └──────────────────────────────────────────┘    │
└──────────────────────┬───────────────────────────┘
                       │
┌──────────────────────▼───────────────────────────┐
│              ON-CHAIN LAYER                       │
│  ┌──────────────────────────────────────────┐    │
│  │        Soroban Smart Contract             │    │
│  │  • Canonical ZYN balance                 │    │
│  │  • Token transfers (Pi ↔ ZYN)            │    │
│  │  • Burn mechanism (0.5%)                  │    │
│  │  • Allowance management                  │    │
│  │  • Cross-app transfer authorization      │    │
│  └──────────────────────────────────────────┘    │
└──────────────────────────────────────────────────┘
```

## 2.2 Data Flow

### Read Flow (Balance Check)
```
App → Firestore Cache → Display to User
         (instant)
```

### Write Flow (Transfer/Spend)
```
App → Soroban Contract → Update Balance → Firestore Sync → Display
      (5 sec confirm)    (on-chain)        (real-time)
```

### Discrepancy Resolution
```
Firestore Balance ≠ On-Chain Balance
         │
         ▼
On-Chain is ALWAYS the source of truth
         │
         ▼
Firestore updated to match on-chain
```

## 2.3 Firestore Document Structure

```
users/
  {username}/
    balance: { pi: 100, zyn: 500 }
    transactions: [ ... ]
    trust_score: 4.2
    apps: { zynmart: true, app_b: true }
    updated_at: timestamp

apps/
  {app_id}/
    name: "ZynMart"
    stake: 1000
    is_active: true
    integration_tier: "govern"
```

## 2.4 Smart Contract Endpoints

| Method | Description |
|---|---|
| `balance_of(user)` | Get canonical ZYN balance |
| `transfer(from, to, amount)` | Transfer ZYN between users |
| `transfer_cross_app(user, app, amount)` | Spend ZYN in another app |
| `approve_app(user, app, allowance)` | Authorize app to spend ZYN |
| `burn(amount)` | Burn 0.5% of transaction |
| `get_transaction_history(user)` | Get on-chain transaction log |

Next: [`3-Balance-Sync`](3-balance-sync.md)
