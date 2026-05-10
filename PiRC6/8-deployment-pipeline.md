# 8. Deployment Pipeline

## 8.1 CI/CD Architecture

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│  Commit  │───▶│  Build   │───▶│   Test   │───▶│   Audit  │───▶│  Deploy  │
│  (Git)   │    │  (Rust)  │    │ (Soroban)│    │ (Security)│    │ (Pi Net) │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
     │               │               │               │               │
     ▼               ▼               ▼               ▼               ▼
  GitHub         cargo build    soroban test    cargo audit    soroban deploy
  Push           --release      --coverage      --deny         --network
```

## 8.2 Build Configuration

```toml
# Cargo.toml
[package]
name = "zyn-contracts"
version = "0.1.0"
edition = "2021"

[dependencies]
soroban-sdk = "21.0.0"

[dev-dependencies]
soroban-sdk = { version = "21.0.0", features = ["testutils"] }

[profile.release]
opt-level = "z"      # Minimize WASM size
overflow-checks = true
debug = 0
strip = "symbols"
```

## 8.3 GitHub Actions Workflow

```yaml
name: ZYN Contract CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: stellar/actions/rust-setup@v1
      - run: cargo build --release --target wasm32-unknown-unknown
      - run: cargo test --all-features
      - run: soroban contract optimize --wasm target/wasm32-unknown-unknown/release/*.wasm

  security-audit:
    needs: build-and-test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: cargo install cargo-audit
      - run: cargo audit --deny warnings

  deploy-testnet:
    needs: security-audit
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: soroban contract deploy --network testnet --wasm target/wasm32-unknown-unknown/release/zyn_token.wasm
      - run: soroban contract deploy --network testnet --wasm target/wasm32-unknown-unknown/release/zyn_subscription.wasm
      - run: soroban contract deploy --network testnet --wasm target/wasm32-unknown-unknown/release/zyn_marketplace.wasm
      - run: soroban contract deploy --network testnet --wasm target/wasm32-unknown-unknown/release/zyn_trust.wasm
      - run: soroban contract deploy --network testnet --wasm target/wasm32-unknown-unknown/release/zyn_wallet.wasm
```

## 8.4 Deployment Checklist

Before deploying to Pi Mainnet:

- [ ] All unit tests pass (`cargo test`)
- [ ] All integration tests pass (`soroban test`)
- [ ] Security audit clean (`cargo audit`)
- [ ] Formal verification of critical paths
- [ ] Gas estimation within budget
- [ ] Testnet deployment verified for 7+ days
- [ ] Community review period complete
- [ ] Admin keys secured in multi-sig

Next: [`9-Testing-Framework`](9-testing-framework.md)
