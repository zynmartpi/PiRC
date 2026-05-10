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

## Key Improvements Over PiRC2

| Feature | PiRC2 (Original) | ZYN Enhanced |
|---|---|---|
| **Grace Period** | ❌ Failed = auto-cancel | ✅ 3-day grace period before cancellation |
| **Dispute Resolution** | ❌ None | ✅ On-chain dispute with merchant bond |
| **Merchant Staking** | ❌ None | ✅ Merchants must stake ZYN to register services |
| **ZYN Token Support** | ❌ Pi only | ✅ Accepts both Pi and ZYN as payment |
| **Tiered Pricing** | ❌ Fixed price | ✅ Discount tiers for long-term subscribers |
| **Trial Guard** | Basic | ✅ Enhanced: trial abuse prevention + cooldown |
| **Refund Mechanism** | ❌ None | ✅ Pro-rata refund on cancellation |
| **Pause/Resume** | ❌ None | ✅ Subscribers can pause subscriptions |
| **Batch Processing** | Basic | ✅ Enhanced with retry logic + priority queuing |
| **Cross-App Subscriptions** | ❌ Single app | ✅ Cross-app subscription portability |
