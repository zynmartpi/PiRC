# 8. Privacy Model

## 8.1 Core Principle

**On-chain data is public. AI models learn from aggregate patterns, not individual behavior.**

## 8.2 Privacy Layers

| Layer | Data | Access | Storage |
|---|---|---|---|
| **On-Chain** | Transaction amounts, addresses, timestamps | Public (Stellar ledger) | Permanent |
| **Feature Store** | Aggregated features (30-day windows) | AI pipeline only | 90-365 days |
| **Model Input** | Anonymized feature vectors | Training process only | Ephemeral (RAM) |
| **Predictions** | Score + confidence + action | Logged on-chain | Permanent |

## 8.3 Anonymization Rules

1. **No raw PII**: Names, emails, phone numbers never enter the pipeline
2. **Address hashing**: Wallet addresses are hashed before feature extraction
   ```
   GBCP...X3Y2 → sha256(GBCP...X3Y2 + salt) → a7f3...9b2c
   ```
3. **Aggregation minimum**: Any feature window must contain ≥5 users to be used
4. **Differential privacy**: Laplacian noise (ε=0.1) added to all aggregate features
5. **No cross-app user tracking**: Each app can only see its own feature contributions

## 8.4 User Rights

| Right | Implementation |
|---|---|
| **Know** | Users can query their on-chain prediction log |
| **Opt Out** | Users can disable AI-assisted features (manual mode only) |
| **Delete** | Feature store data deleted on request (on-chain logs immutable) |
| **Appeal** | Any AI-flagged action can be appealed to council |

## 8.5 Compliance

| Standard | Status |
|---|---|
| **GDPR** | ✅ Compliant (no PII in pipeline, right to opt out, right to delete features) |
| **Pi Network ToS** | ✅ Compliant (only on-chain public data used) |
| **Stellar GDPR Guide** | ✅ Compliant (on-chain data is ledger-fact, off-chain features deletable) |

## 8.6 Edge Computing (Future)

For maximum privacy, future versions will support **on-device inference**:

```
User's Device (Mobile App)
├── Local feature extraction from own transaction history
├── Pre-trained model (TFLite) runs inference on-device
├── Only prediction result sent to server (no raw data)
└── Server logs prediction on-chain
```

This ensures **zero raw data leaves the user's device**.

---

*This concludes PiRC7: AI-Enhanced Ecosystem Intelligence. For the full system architecture, see [Architecture.md](../Architecture.md).*
