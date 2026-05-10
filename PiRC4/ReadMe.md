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

PiRC1-3 rely on reputation for visibility, fees, and dispute outcomes, but **none define how reputation is calculated or protected**. PiRC4 fills this gap:

| Problem | Current State | PiRC4 Solution |
|---|---|---|
| **Fake Reviews** | Off-chain, easily manipulated | Only verified on-chain buyers can review |
| **Reputation Farming** | Sybil accounts boost scores | Stake-weighted + account age requirements |
| **No Cross-App Trust** | Each app has isolated ratings | Portable trust score across Pi ecosystem |
| **Recovery After Dispute** | One bad review can ruin a merchant | Time-decay + rehabilitation mechanism |
| **Transparency** | Black-box algorithms | Fully transparent on-chain formula |
