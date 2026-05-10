# PiRC3: Decentralized Marketplace Protocol (ZynMart Request for Comment)

This document specifies the **Decentralized Marketplace Protocol** — a Soroban smart contract system for trustless commerce on the Pi Network. Unlike traditional e-commerce platforms where a central authority controls listings, payments, and dispute resolution, this protocol enables **peer-to-peer marketplace operations** with on-chain escrow, reputation-weighted visibility, and community governance.

Community feedback is welcome. Pioneers, developers, and merchants are encouraged to review, comment, and share specific suggestions.

## Table of Contents

- [`1-introduction`](1-introduction.md) **(start here)**
- [`2-overview`](2-overview.md)
- [`3-listing-system`](3-listing-system.md)
- [`4-escrow-payment`](4-escrow-payment.md)
- [`5-dispute-resolution`](5-dispute-resolution.md)
- [`6-fee-structure`](6-fee-structure.md)
- [`7-search-ranking`](7-search-ranking.md)
- [`8-data-types`](8-data-types.md)

---

## Why PiRC3 Is Needed

PiRC1 covers token launches. PiRC2 covers subscriptions. But **neither addresses the core commerce flow**: listing products → accepting payments → ensuring delivery → resolving disputes. Current Pi apps handle this off-chain with centralized backends, creating trust issues and single points of failure.

PiRC3 brings the full marketplace stack on-chain:

| Feature | Current (Off-Chain) | PiRC3 (On-Chain) |
|---|---|---|
| **Product Listings** | Centralized database | On-chain with merchant stake |
| **Payments** | Pi SDK + backend approval | Smart contract escrow |
| **Delivery Verification** | Manual / trust-based | On-chain proof + timeout |
| **Disputes** | Centralized admin | Community jury + bond |
| **Search Ranking** | Algorithm / paid ads | Reputation-weighted |
| **Fee Distribution** | Platform takes all | Burn + LP + merchant reward |
