# 5. Error Codes

Includes all PiRC2 original error codes plus new ones:

| Code | Name | Description |
|---|---|---|
| 1 | `InvalidPrice` | Price must be > 0 |
| 2 | `InvalidPeriod` | `period_secs` and `approve_periods` must be > 0 |
| 3 | `AlreadySubscribed` | Active subscription to this service already exists |
| 4 | `SubscriptionNotFound` | Subscription ID does not exist |
| 5 | `ServiceNotFound` | Service ID does not exist or is inactive |
| 6 | `Unauthorized` | Caller is not authorized for this action |
| 7 | `AlreadyCancelled` | Subscription is already non-recurring |
| 8 | `TimestampOverflow` | Timestamp arithmetic would overflow `u64` |
| 9 | `NotServiceOwner` | Caller is not the merchant of this service |
| 10 | `InvalidServiceName` | Service name must not be empty |
| 11 | `SubscriptionExpired` | Cannot re-enable or extend an expired subscription |
| 12 | `InsufficientMerchantStake` | Merchant ZYN stake below required (NEW) |
| 13 | `GracePeriodActive` | Cannot modify subscription during grace period (NEW) |
| 14 | `DisputeAlreadyOpen` | A dispute is already open for this charge (NEW) |
| 15 | `DisputeNotFound` | Dispute ID does not exist (NEW) |
| 16 | `PauseLimitExceeded` | Maximum pause duration exceeded (NEW) |
| 17 | `InvalidDiscountTier` | Discount tier not offered by this service (NEW) |
| 18 | `InvalidPaymentToken` | Payment token not accepted by this service (NEW) |
| 19 | `TrialCooldownActive` | User recently used trial for this service (NEW) |
| 20 | `NotDisputeParticipant` | Caller is not subscriber or merchant of dispute (NEW) |

Next: [`6-Constructor-and-Service-Management`](6-constructor-and-service-management.md)
