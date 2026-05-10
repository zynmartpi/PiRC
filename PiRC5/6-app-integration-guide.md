# 6. App Integration Guide

## 6.1 Integration Steps

### Step 1: Apply for Integration
- Submit application to ZYN Governance Council
- Describe app, expected ZYN usage, security measures
- Stake required ZYN amount

### Step 2: Implement ZYN SDK
```javascript
// Initialize ZYN wallet in your app
import { ZynWallet } from '@zynmart/zyn-sdk';

const wallet = new ZynWallet({
  appId: 'your-app-id',
  sorobanRpc: 'https://rpc.mainnet.minepi.com',
  firebaseConfig: { /* ... */ }
});

// Check user balance (from cache, instant)
const balance = await wallet.getBalance(username);
// { pi: 100, zyn: 500 }

// Check on-chain balance (authoritative, ~5 sec)
const onChainBalance = await wallet.getOnChainBalance(username);

// Request allowance from user
await wallet.requestAllowance({
  amount: 100,  // ZYN
  purpose: 'Premium subscription'
});

// Process payment
const tx = await wallet.transfer({
  from: username,
  to: merchantUsername,
  amount: 50,
  token: 'ZYN'
});
// tx = { txId: '...', amount: 49.75, fee: 0.25, burn: 0.125 }
```

### Step 3: Test on Testnet
- Use Pi Testnet for integration testing
- Verify balance sync, cross-app transfers, allowances
- Submit test results to council

### Step 4: Council Review & Approval
- Council reviews integration code
- Security audit (for Govern tier and above)
- Community vote (for Launch tier)

### Step 5: Mainnet Launch
- Deploy to mainnet
- Monitor first 48 hours closely
- Report any issues to council

## 6.2 Integration Tiers

| Tier | Stake | Capabilities | Approval |
|---|---|---|---|
| **Accept** | 100 ZYN | Read balances, accept ZYN payments | Auto-approved |
| **Reward** | 1,000 ZYN | Distribute ZYN rewards | Council review |
| **Govern** | 5,000 ZYN | Governance participation | Council + vote |
| **Launch** | 10,000 ZYN | Launch tokens via ZYN Launchpad | Community vote |

## 6.3 Best Practices

1. **Always check on-chain balance** before processing withdrawals
2. **Use Firestore cache** for display only, never for transaction decisions
3. **Implement rate limiting** on your app's ZYN operations
4. **Show transaction confirmation** to user before every ZYN spend
5. **Handle network errors** gracefully — ZYN operations may take 5-10 seconds
6. **Cache transaction history** locally but validate against on-chain periodically

Next: [`7-Data-Types`](7-data-types.md)
