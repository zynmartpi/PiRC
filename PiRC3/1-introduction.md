# 1. Introduction

The Decentralized Marketplace Protocol enables trustless commerce on Pi Network. Buyers and sellers interact through smart contracts that hold funds in escrow until delivery is confirmed, eliminating the need for a trusted intermediary.

## 1.1 Core Innovation: Stake-Escrow-Reputation Triangle

```
        ┌─────────────┐
        │  REPUTATION  │
        │  (Trust Score)│
        └──────┬───────┘
               │
    ┌──────────┴──────────┐
    │                     │
┌───▼────┐         ┌──────▼─────┐
│  STAKE │         │   ESCROW   │
│ (ZYN)  │         │ (Payment)  │
└────────┘         └────────────┘
```

- **Stake**: Merchants stake ZYN to list products. Higher stake = better visibility.
- **Escrow**: Payments are held in smart contract until delivery confirmed.
- **Reputation**: Successful transactions increase trust score, affecting visibility and fee rates.

## 1.2 Commerce Flow

1. **Merchant** stakes ZYN and lists product on-chain
2. **Buyer** initiates purchase, payment goes to escrow contract
3. **Merchant** ships product and submits delivery proof
4. **Buyer** confirms receipt (or timeout auto-confirms after 7 days)
5. **Escrow** releases payment to merchant (minus marketplace fee)
6. **Reputation** updates for both parties
7. **Fee** is distributed: 0.5% burn + 1% LP + 0.5% treasury

## 1.3 Use Cases

- Physical goods marketplace (ZynMart)
- Digital goods & NFTs
- Service marketplace (freelance, consulting)
- Local commerce (delivery, reservations)
- Cross-app commerce (any Pi app can integrate)

Next: [`2-Overview`](2-overview.md)
