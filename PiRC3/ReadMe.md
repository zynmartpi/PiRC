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

*Complementing the token launch (PiRC1) and subscription (PiRC2) foundations laid by [PiNetwork/PiRC](https://github.com/PiNetwork/PiRC) with a dedicated commerce protocol.*

PiRC1 covers token launches. PiRC2 covers subscriptions. PiRC3 completes the ecosystem by addressing the **core commerce flow**: listing products → accepting payments → ensuring delivery → resolving disputes.

### What PiRC3 Adds

| Innovation | Description |
|---|---|
| **On-Chain Product Listings** | Products listed on-chain with merchant stake commitment — transparent and verifiable |
| **Smart Contract Escrow** | Payments held in escrow until delivery confirmed — buyer protection by design |
| **Delivery Proof & Auto-Confirmation** | Merchants submit on-chain proof; 7-day auto-confirm protects from buyer silence |
| **Community Jury Disputes** | 5 randomly selected ZYN stakers resolve disputes — decentralized and fair |
| **Reputation-Weighted Search** | Trust score + stake + sales volume determine ranking — quality over ad spend |
| **Transparent Fee Distribution** | 0.5% burn + 1% LP + 0.5% treasury — every fee is on-chain and accounted for |
| **Merchant Stake Tiers** | Bronze/Silver/Gold/Platinum — merchants commit ZYN proportional to their ambitions |
| **Appeal Process** | Either party can appeal with 50 ZYN deposit — 7 new jurors, final decision |
