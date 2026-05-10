# 3. Balance Synchronization

## 3.1 Dual-Layer Sync Protocol

### Firestore → UI (Read Path)
- **Latency**: <100ms (real-time)
- **Purpose**: Display balance in app UI
- **Method**: Firestore `onSnapshot()` listener
- **Accuracy**: Eventually consistent (may be slightly stale)

### Soroban → Firestore (Write Path)
- **Latency**: ~5 seconds (block confirmation)
- **Purpose**: Update Firestore cache after on-chain write
- **Method**: Backend listener on Soroban events → Firestore update
- **Accuracy**: Authoritative (on-chain is source of truth)

## 3.2 Sync Rules

| Rule | Description |
|---|---|
| **On-Chain First** | All writes go through smart contract first |
| **Cache Last** | Firestore is always updated after on-chain confirmation |
| **Reconcile Periodically** | Every 15 minutes, full balance reconciliation |
| **Discrepancy Alert** | If cache differs from chain by >1 ZYN, flag for review |
| **Never Trust Cache for Withdrawals** | Withdrawals always check on-chain balance first |

## 3.3 Conflict Resolution

When Firestore and on-chain disagree:

```
1. On-chain balance: 500 ZYN
2. Firestore cache: 498 ZYN
3. Resolution: Firestore updated to 500 ZYN
4. Alert logged for investigation
5. If discrepancy >10 ZYN, freeze cache reads until resolved
```

## 3.4 Offline Support

When user is offline:
- App shows last known Firestore balance with "⚠️ May not be current" indicator
- On reconnection, app fetches on-chain balance and updates cache
- Any pending transactions are queued and submitted on reconnection

## 3.5 Real-Time Updates

Using Firestore real-time listeners:

```javascript
// App subscribes to balance changes
firebase.firestore()
  .collection('users')
  .doc(username)
  .onSnapshot((doc) => {
    const balance = doc.data().balance;
    updateUI(balance);
  });
```

When on-chain balance changes:
1. Backend detects Soroban event
2. Backend updates Firestore document
3. Firestore triggers `onSnapshot` in all connected apps
4. UI updates in real-time across all apps simultaneously

Next: [`4-Transaction-Protocol`](4-transaction-protocol.md)
