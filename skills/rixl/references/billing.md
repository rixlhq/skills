# Billing and usage

Organization-scoped. `org_id` is a parameter on most of these but defaults to the caller's
organization, so leave it out unless targeting another org.

## Operations

| Operation | Params |
|-----------|--------|
| `billing.v1.InvoiceService.ListInvoices` | `pagination.limit`, `pagination.offset` |
| `billing.v1.SubscriptionService.GetSubscription` | none |
| `billing.v1.SubscriptionService.GetSubscriptionHistory` | `pagination.limit`, `pagination.offset` |
| `billing.v1.PlanService.ListPlans` | `currency`, `interval`, `free` |
| `billing.v1.PlanService.GetPlan` | `plan_id` |
| `billing.v1.UsageService.GetStorageUsage` | none |
| `billing.v1.UsageService.GetStorageUsageHistory` | `days` |
| `billing.v1.UsageService.GetBandwidthUsage` | none |
| `billing.v1.UsageService.GetBandwidthUsageHistory` | `days` |
| `billing.v1.PaymentService.ListPaymentMethods` | `refresh` |
| `billing.v1.PaymentService.GetBillingAddress` | none |
| `billing.v1.PaymentService.GetPaymentMethodFromPaymentIntent` | `payment_intent_id` |
| `billing.v1.PaymentService.GetPaymentMethodFromSetupIntent` | `setup_intent_id` |

## Notes

Usage history takes `days`, a lookback count rather than a start and end range. It does not follow
the `range.start` and `range.end` convention used by analytics.

"Am I over my limit" takes two calls: current usage from `GetStorageUsage` or `GetBandwidthUsage`,
read against the plan from `GetSubscription`.

The two payment intent operations are for reconciling a checkout that just happened. They need an id
from the payment flow, so skip them unless the user supplies one.

Payment method and billing address reads expose partial card and address details. Report only what
the user asked for.
