# 5. Security Model

## 5.1 Threat Model

| Threat | Mitigation |
|---|---|
| **App drains user balance** | Per-app allowance limits + user approval |
| **Fake app integration** | Governance council approval + ZYN stake requirement |
| **Cache manipulation** | On-chain is source of truth + periodic reconciliation |
| **Replay attacks** | Nonce-based transaction IDs + ledger timestamps |
| **Front-running** | Soroban's deterministic execution prevents MEV |
| **Key compromise** | Pi SDK handles signing — apps never see private keys |

## 5.2 App Authorization

Before an app can access the ZYN wallet system:

1. **Governance Vote**: Community votes on new app integration
2. **Stake Requirement**: App must stake 100-10,000 ZYN (based on integration tier)
3. **Code Review**: Smart contract integration code reviewed by council
4. **Audit**: External security audit for high-tier integrations
5. **Monitoring**: Ongoing transaction monitoring for suspicious patterns

## 5.3 User Controls

Users have full control over their ZYN:

- **Allowance Management**: Set, modify, or revoke app allowances anytime
- **Transaction Review**: Every cross-app transaction requires user confirmation
- **Emergency Freeze**: User can freeze all allowances with one action
- **Activity Log**: Full on-chain transaction history viewable in any app
- **Notification**: Real-time alerts for every ZYN movement

## 5.4 Rate Limits

| Action | Rate Limit | Purpose |
|---|---|---|
| **Cross-app transfers** | 10 per hour | Prevent rapid draining |
| **Allowance changes** | 5 per hour | Prevent rapid approval exploitation |
| **New app approvals** | 3 per day | Prevent user from being overwhelmed |
| **Burn threshold** | Max 100 ZYN/hour | Prevent excessive burning |

## 5.5 Emergency Procedures

| Event | Response |
|---|---|
| **Suspicious app activity** | Auto-pause app's allowance, alert user + council |
| **Smart contract vulnerability** | Admin can pause contract, upgrade WASM |
| **Mass discrepancy** | Freeze all cache reads, force on-chain verification |
| **User report** | Investigate within 24 hours, temporary freeze if needed |

Next: [`6-App-Integration-Guide`](6-app-integration-guide.md)
