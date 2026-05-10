# Security Audit Framework

## Overview

This document defines the security audit process for all ZYN smart contracts and infrastructure.

## 1. Threat Model

| Threat | Impact | Mitigation |
|---|---|---|
| **Reentrancy** | Drain contract funds | Soroban is not EVM — no reentrancy by design (single-threaded execution) |
| **Integer Overflow** | Incorrect balances | Rust checked arithmetic by default; Soroban SDK uses `i128` with overflow checks |
| **Unauthorized Access** | Admin functions called by attacker | `require_auth()` on every sensitive function |
| **Front-Running** | MEV on DEX trades | Soroban has deterministic transaction ordering — no traditional MEV |
| **Flash Loan Attack** | Manipulate price/oracle in single tx | No external oracle dependencies; all prices set by contract params |
| **Sybil Attack** | Fake accounts manipulate trust/jury | Stake requirement + account age + Pi KYC verification |
| **Wash Trading** | Inflate volume for visibility | FraudGuard AI detection + trade partner concentration metric |
| **Admin Key Compromise** | Upgrade contract to malicious code | Multi-sig admin (3/5 threshold) + 48h timelock on upgrades |
| **Grace Period Exploit** | Subscribe → use → cancel → repeat | Trial cooldown + pro-rata refund (10% burned) |
| **Dispute Spam** | File disputes to harass merchants | 50 ZYN deposit to file; lost deposit if dispute rejected |

## 2. Audit Checklist

### Pre-Deployment

- [ ] All functions have `require_auth()` where needed
- [ ] No unchecked arithmetic operations
- [ ] Admin functions are multi-sig protected
- [ ] Upgrade function has timelock (48h minimum)
- [ ] No hardcoded addresses or magic numbers
- [ ] All external contract calls use `invoke_contract` with validated addresses
- [ ] Storage keys are namespaced to prevent collisions
- [ ] Events emitted for all state changes
- [ ] Error codes match PiRC2-5 specifications
- [ ] TTL management for temporary storage is correct

### Post-Deployment

- [ ] Contract IDs verified on Pi Testnet
- [ ] Admin keys stored in multi-sig wallet
- [ ] Monitor contract for unexpected invocations
- [ ] Verify initial state matches specification
- [ ] Test upgrade mechanism with non-malicious WASM
- [ ] Verify event emission matches backend expectations

## 3. Formal Verification Targets

| Contract | Function | Property to Verify |
|---|---|---|
| **zyn_token** | `transfer` | Burn amount = exactly 0.5% of transfer amount |
| **zyn_token** | `stake` | Total stake never exceeds total supply |
| **zyn_marketplace** | `initiate_purchase` | Escrow amount = listing price × quantity |
| **zyn_marketplace** | `distribute_fees` | burn + LP + treasury + merchant = total escrow |
| **zyn_subscription** | `cancel` | Refund + burn = remaining proportional value |
| **zyn_trust** | `recalculate_trust` | Score always in [0.0, 5.0] range |
| **zyn_wallet** | `transfer_cross_app` | Transfer is atomic (all-or-nothing) |

## 4. Bug Bounty Program

| Severity | Criteria | Reward |
|---|---|---|
| **Critical** | Funds can be drained or permanently locked | 10,000 ZYN |
| **High** | Contract logic bypassed (e.g., skip escrow) | 5,000 ZYN |
| **Medium** | Unexpected behavior with financial impact | 2,000 ZYN |
| **Low** | Non-critical logic error, no fund risk | 500 ZYN |
| **Informational** | Gas optimization or code quality improvement | 100 ZYN |

## 5. Incident Response

| Step | Action | Timeline |
|---|---|---|
| **1. Detect** | AnomalyScout alert or community report | Immediate |
| **2. Assess** | Council reviews severity and scope | < 1 hour |
| **3. Contain** | Emergency pause if critical | < 2 hours |
| **4. Fix** | Develop and test patch | < 24 hours |
| **5. Upgrade** | Deploy fix via upgrade mechanism | < 48 hours |
| **6. Post-Mortem** | Publish incident report to community | < 72 hours |
| **7. Prevent** | Update detection rules + audit process | < 1 week |
