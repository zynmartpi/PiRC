# 6. Constructor and Service Management

## 6.1 Constructor

#### `__constructor(admin, pi_token, zyn_token, dispute_council)`

Initializes the enhanced contract.

| Param | Type | Description |
|---|---|---|
| `admin` | `Address` | Contract administrator |
| `pi_token` | `Address` | Pi token contract address |
| `zyn_token` | `Address` | ZYN token contract address (NEW) |
| `dispute_council` | `Address` | Governance council address (NEW) |

## 6.2 Service Management

#### `register_service(merchant, name, price, period_secs, trial_period_secs, approve_periods, accepts_zyn, discount_tiers) -> Service`

Creates a new service. Requires merchant authorization and sufficient ZYN stake.

| Param | Type | Description |
|---|---|---|
| `merchant` | `Address` | Service owner |
| `name` | `String` | Service name (non-empty) |
| `price` | `i128` | Price per period (> 0) |
| `period_secs` | `u64` | Billing cycle in seconds (> 0) |
| `trial_period_secs` | `u64` | Trial duration (0 = no trial) |
| `approve_periods` | `u64` | Periods to pre-approve (> 0) |
| `accepts_zyn` | `bool` | Whether ZYN is accepted (NEW) |
| `discount_tiers` | `Vec<DiscountTier>` | Available discount tiers (NEW) |

**Pre-conditions (NEW):**
- Merchant must have staked sufficient ZYN: `stake >= BaseStake`
- If `accepts_zyn = true`, merchant must stake additional 50 ZYN

**Errors:** `InvalidPrice`, `InvalidPeriod`, `InvalidServiceName`, `InsufficientMerchantStake`

**Events:** `srv_reg`

#### `deactivate_service(merchant, service_id)`

Deactivates a service. Existing subscriptions continue until their period ends. No new subscriptions accepted.

**Errors:** `ServiceNotFound`, `NotServiceOwner`

**Events:** `srv_deact`

Next: [`7-Subscription-Lifecycle`](7-subscription-lifecycle.md)
