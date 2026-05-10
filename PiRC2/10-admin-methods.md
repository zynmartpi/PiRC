# 10. Admin Methods

### 10.1 `upgrade(new_wasm_hash)`

Upgrades the contract WASM. Requires admin authorization.

**Events:** `upgrade`

### 10.2 `version() -> u32`

Returns the current contract version (currently 4).

### 10.3 `set_grace_period_secs(secs)` (NEW)

Updates the grace period duration. Default: 259200 (3 days). Requires admin.

**Events:** `config_update`

### 10.4 `set_dispute_window_secs(secs)` (NEW)

Updates the dispute filing window. Default: 604800 (7 days). Requires admin.

**Events:** `config_update`

### 10.5 `set_base_merchant_stake(amount)` (NEW)

Updates the base merchant stake requirement. Default: 100 ZYN. Requires admin.

**Events:** `config_update`

### 10.6 `set_cancellation_fee_bps(bps)` (NEW)

Updates the cancellation fee in basis points. Default: 1000 (10%). Requires admin.

**Events:** `config_update`

---

*This concludes PiRC2: Enhanced Subscription Contract API. For the decentralized marketplace protocol, see [PiRC3](../PiRC3/ReadMe.md).*
