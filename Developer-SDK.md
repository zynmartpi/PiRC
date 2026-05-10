# ZYN Developer SDK

## Overview

The ZYN SDK provides TypeScript/JavaScript libraries for integrating ZYN ecosystem features into Pi applications.

## 1. Package Structure

```
@zyn/sdk
├── core          # Wallet connection, auth, config
├── token         # ZYN balance, transfer, approve
├── subscription  # Subscribe, cancel, pause, resume
├── marketplace   # Listings, orders, escrow
├── trust         # Trust score queries, reviews
└── wallet        # Cross-app transfers, allowances
```

## 2. Installation

```bash
npm install @zyn/sdk
```

## 3. Quick Start

```typescript
import { ZynClient } from '@zyn/sdk';

const client = new ZynClient({
  network: 'testnet',
  appId: 'zynmart',
  sorobanRpcUrl: 'https://api.testnet.soroban.pi.network',
});

// Connect wallet
const wallet = await client.connectWallet();

// Check ZYN balance
const balance = await client.token.balance(wallet.address);
console.log(`ZYN Balance: ${balance}`);

// Subscribe to a service
const subscription = await client.subscription.subscribe({
  serviceId: 1,
  paymentToken: 'ZYN',
  committedPeriods: 12, // Annual = 15% discount
});

// Create marketplace listing
const listing = await client.marketplace.createListing({
  title: 'Premium Widget',
  price: 50, // 50 ZYN
  category: 'electronics',
  stakeAmount: 200, // 200 ZYN stake
});
```

## 4. Core API

### 4.1 Wallet Connection

```typescript
// Connect via Pi SDK authentication
const wallet = await client.connectWallet();

// Listen for balance changes (real-time via Firestore)
client.token.onBalanceChange(wallet.address, (newBalance) => {
  console.log(`Balance updated: ${newBalance}`);
});
```

### 4.2 Token Operations

```typescript
// Transfer ZYN
await client.token.transfer({
  to: 'GBCP...X3Y2',
  amount: 100,
});

// Approve spending for subscription
await client.token.approve({
  spender: subscriptionContractAddress,
  amount: 600, // 12 months × 50 ZYN
  expirationLedger: 100000,
});

// Stake ZYN (merchant)
await client.token.stake({
  amount: 500,
});
```

### 4.3 Subscription Operations

```typescript
// Subscribe
const sub = await client.subscription.subscribe({
  serviceId: 1,
  paymentToken: 'ZYN',
  committedPeriods: 3, // Quarterly = 10% discount
});

// Pause subscription
await client.subscription.pause(sub.id);

// Resume subscription
await client.subscription.resume(sub.id);

// Cancel with pro-rata refund
const refund = await client.subscription.cancel(sub.id);
console.log(`Refunded: ${refund.amount} ZYN, Burned: ${refund.burned} ZYN`);

// File dispute
const dispute = await client.subscription.dispute({
  subscriptionId: sub.id,
  reason: 'Service not delivered as described',
  evidenceHash: 'Qm...ipfs',
});
```

### 4.4 Marketplace Operations

```typescript
// Create listing (merchant)
const listing = await client.marketplace.createListing({
  title: 'Premium Widget',
  description: 'High-quality widget',
  price: 50,
  category: 'electronics',
  stakeAmount: 200,
});

// Purchase (buyer)
const order = await client.marketplace.initiatePurchase({
  listingId: listing.id,
  quantity: 2,
  paymentToken: 'ZYN',
});

// Submit shipping proof (merchant)
await client.marketplace.submitShippingProof({
  orderId: order.id,
  proofHash: 'Qm...ipfs',
  carrier: 'DHL',
  trackingNumber: '1234567890',
});

// Confirm delivery (buyer)
await client.marketplace.confirmDelivery(order.id);

// File dispute
await client.marketplace.fileDispute({
  orderId: order.id,
  reason: 'Item not as described',
  evidenceHash: 'Qm...ipfs',
});
```

### 4.5 Trust Score Queries

```typescript
// Get trust score
const trust = await client.trust.getScore(wallet.address);
console.log(`Trust: ${trust.score}/5.0, Tier: ${trust.tier}`);

// Get merchant tier benefits
const tier = await client.trust.getMerchantTier(merchantAddress);
console.log(`Fee discount: ${tier.feeDiscount}%, Boost discount: ${tier.boostDiscount}%`);

// Submit review (verified buyer only)
await client.trust.submitReview({
  merchantAddress: 'GBCP...X3Y2',
  orderId: order.id,
  rating: 4, // 1-5
  comment: 'Great product, fast shipping',
});
```

### 4.6 Cross-App Wallet

```typescript
// Approve app allowance
await client.wallet.approveApp({
  appId: 'app-b',
  allowance: 100, // Max 100 ZYN spendable by App B
  expirationLedger: 200000,
});

// Cross-app transfer
await client.wallet.transferCrossApp({
  to: 'GBCP...X3Y2',
  amount: 25,
  sourceApp: 'zynmart',
  destApp: 'app-b',
});

// Check allowances
const allowances = await client.wallet.getAppAllowances(wallet.address);
```

## 5. Event Listening

```typescript
// Listen to on-chain events (via Firestore sync)
client.events.on('subscription:charged', (data) => {
  console.log(`Charged: ${data.amount} ZYN for sub #${data.subId}`);
});

client.events.on('marketplace:order_created', (data) => {
  console.log(`New order: #${data.orderId} for ${data.amount} ZYN`);
});

client.events.on('trust:score_updated', (data) => {
  console.log(`Trust updated: ${data.newScore}/5.0`);
});

client.events.on('token:burn', (data) => {
  console.log(`Burned: ${data.amount} ZYN — total supply decreasing`);
});
```

## 6. Error Handling

```typescript
try {
  await client.subscription.subscribe({ serviceId: 1, paymentToken: 'ZYN', committedPeriods: 0 });
} catch (error) {
  if (error.code === ZynError.InsufficientBalance) {
    console.log('Not enough ZYN — need to top up');
  } else if (error.code === ZynError.InsufficientMerchantStake) {
    console.log('Merchant stake too low — service unavailable');
  } else if (error.code === ZynError.GracePeriodActive) {
    console.log('Subscription in grace period — top up to keep active');
  }
}
```

## 7. Integration Tiers

| Tier | Capabilities | Stake Required |
|---|---|---|
| **Accept** | Accept ZYN as payment | 100 ZYN |
| **Reward** | Reward users with ZYN (cashback, loyalty) | 500 ZYN |
| **Govern** | Participate in governance votes | 1,000 ZYN |
| **Launch** | Launch custom token via ZYN Launchpad | 10,000 ZYN |
