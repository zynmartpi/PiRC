# 9. Query Methods (Enhanced)

### 9.1 `get_subscription(caller, sub_id) -> Subscription`

Returns subscription details. Caller must be subscriber or merchant.

**Errors:** `SubscriptionNotFound`, `Unauthorized`, `ServiceNotFound`

### 9.2 `get_subscriber_subs(subscriber) -> Vec<Subscription>`

Returns all subscriptions for a subscriber. Requires authorization.

### 9.3 `get_merchant_subs(merchant, service_id) -> Vec<Subscription>`

Returns all subscriptions for a service. Requires merchant authorization.

**Errors:** `ServiceNotFound`, `NotServiceOwner`

### 9.4 `get_service(service_id) -> Service`

Returns service details. No authorization required.

**Errors:** `ServiceNotFound`

### 9.5 `get_merchant_services(merchant) -> Vec<Service>`

Returns all services owned by a merchant. No authorization required.

### 9.6 `is_subscription_active(subscriber, service_id) -> bool`

Returns true if subscriber has active subscription (current time < service_end_ts).

### 9.7 `get_dispute(dispute_id) -> Dispute` (NEW)

Returns dispute details. Caller must be subscriber, merchant, or council.

**Errors:** `DisputeNotFound`, `NotDisputeParticipant`

### 9.8 `get_merchant_stake(merchant) -> StakeInfo` (NEW)

Returns merchant's current ZYN stake info.

| Field | Type | Description |
|---|---|---|
| `staked_amount` | `i128` | Currently staked ZYN |
| `required_amount` | `i128` | Required based on subscribers |
| `bond_amount` | `i128` | Amount reserved for disputes |
| `is_compliant` | `bool` | Whether stake meets requirement |

### 9.9 `get_service_disputes(service_id) -> Vec<Dispute>` (NEW)

Returns all disputes for a service. Requires merchant authorization.

**Errors:** `ServiceNotFound`, `NotServiceOwner`

Next: [`10-Admin-Methods`](10-admin-methods.md)
