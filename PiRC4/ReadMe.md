# PiRC4: Reputation & Trust Score System (ZynMart Request for Comment)

This document specifies the **Reputation & Trust Score System** — an on-chain reputation protocol for the Pi ecosystem. Unlike off-chain rating systems that can be manipulated, this system anchors trust scores to verifiable on-chain behavior, creating a transparent and tamper-proof reputation layer.

Community feedback is welcome.

## Table of Contents

- [`1-introduction`](1-introduction.md) **(start here)**
- [`2-trust-score-calculation`](2-trust-score-calculation.md)
- [`3-merchant-reputation`](3-merchant-reputation.md)
- [`4-buyer-reputation`](4-buyer-reputation.md)
- [`5-cross-app-portability`](5-cross-app-portability.md)
- [`6-anti-manipulation`](6-anti-manipulation.md)
- [`7-data-types`](7-data-types.md)

---

## Why PiRC4 Is Needed

*Extending the ecosystem framework established by [PiNetwork/PiRC](https://github.com/PiNetwork/PiRC) with a dedicated reputation layer.*

PiRC1-3 rely on reputation for visibility, fees, and dispute outcomes, but **none define how reputation is calculated or protected**. PiRC4 provides the trust infrastructure the ecosystem needs.

### What PiRC4 Adds

| Innovation | Description |
|---|---|
| **Verified On-Chain Reviews** | Only on-chain verified buyers can review — eliminates fake reviews by design |
| **Stake-Weighted Scoring** | Reviewer trust + stake determines review impact — committed users have more influence |
| **Cross-App Trust Portability** | Trust score travels with users across all Pi apps — one reputation, entire ecosystem |
| **Time-Decay & Rehabilitation** | Negative events decay over time; 6 months of good behavior reduces impact by 50% |
| **Transparent Formula** | Fully public on-chain scoring: 40% transactions + 30% reviews + 20% disputes + 10% account |
| **Merchant Tiers** | New → Rising Star → Trusted → Top Merchant — each tier unlocks benefits and lower fees |
| **Anti-Manipulation** | Sybil resistance, wash trade detection, review swap flagging, random jury selection |
| **Community Audit** | Any user can report suspected manipulation (50 ZYN deposit) — council investigates |
