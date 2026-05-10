# PiRC2: Enhanced Subscription Contract API (ZynMart Request for Comment)

This document specifies the **Enhanced Subscription Contract API** — an improved version of PiRC2's Soroban smart contract for recurring payments, adding **grace periods**, **dispute resolution**, **merchant staking requirements**, and **ZYN token integration**.

Community feedback is welcome. Pioneers, developers, and merchants are encouraged to review, comment, and share specific suggestions through GitHub Issues, Discussions, or Pull Requests.

## Table of Contents

- [`1-introduction`](1-introduction.md) **(start here)**
- [`2-overview`](2-overview.md)
- [`3-enhanced-features`](3-enhanced-features.md)
- [`4-data-types`](4-data-types.md)
- [`5-error-codes`](5-error-codes.md)
- [`6-constructor-and-service-management`](6-constructor-and-service-management.md)
- [`7-subscription-lifecycle`](7-subscription-lifecycle.md)
- [`8-dispute-resolution`](8-dispute-resolution.md)
- [`9-query-methods`](9-query-methods.md)
- [`10-admin-methods`](10-admin-methods.md)

---

## What PiRC2+ Adds to the Pi Ecosystem

*Extending the subscription contract foundation of [PiNetwork/PiRC2](https://github.com/PiNetwork/PiRC/tree/main/PiRC2) with buyer protection and merchant accountability.*

| Innovation | Description |
|---|---|
| **Grace Periods** | 3-day buffer before cancellation on failed payment — subscribers can top up and keep their subscription |
| **Dispute Resolution** | On-chain dispute mechanism with merchant bond — fair, transparent, decentralized |
| **Merchant Staking** | Merchants must stake ZYN to register services — ensures commitment and accountability |
| **Dual Token Support** | Accepts both Pi and ZYN as subscription payment — more flexibility for users |
| **Tiered Pricing** | 5-15% discounts for quarterly/annual commitments — rewards loyal subscribers |
| **Enhanced Trial Guard** | Trial abuse prevention with cooldown period — protects merchants from exploitation |
| **Pro-Rata Refunds** | Fair refund on cancellation — 90% returned, 10% burned (not kept by merchant) |
| **Pause & Resume** | Subscribers can pause subscriptions up to 30 days/year — real-world flexibility |
| **Enhanced Batch Processing** | Retry logic + priority queuing + grace period awareness — more reliable billing |
| **Cross-App Subscriptions** | Subscribe once, use across multiple Pi apps — ecosystem-wide portability |
