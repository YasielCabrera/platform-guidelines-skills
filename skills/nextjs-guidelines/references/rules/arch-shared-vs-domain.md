---
title: Shared vs domain module placement
category: arch
tags: shared, domain, placement, promotion
---

## Shared vs domain module placement

Use the [Promotion rules](../architecture.md#promotion-rules) and [Component placement decision tree](../architecture.md#component-placement-decision-tree). Do not promote when domain-coupled (e.g. `useServicePurchaseModal` used across builders, finances, workstream) or subdomain-scoped (shared only between `services` and `service-purchase`). Promote only when actually used in 2+ modules or app root—never speculatively.

**Incorrect:** Putting a shared button in a domain module, or a domain-specific form in shared. Also incorrect: promoting domain-coupled or subdomain-scoped code to shared.

```typescript
// In modules/transaction/components/shared-button.tsx — wrong; shared UI belongs in shared
```

**Correct:**

- Generic UI, feature flags, fetcher, nav config → `modules/shared/`
- Transaction list, transaction form, transaction service → `modules/transaction/`
- Domain-coupled hook used in 2+ modules (e.g. `useServicePurchaseModal`) → keep in domain module

Reference: [architecture.md](../architecture.md)
