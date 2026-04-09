---
title: Promotion rules for moving code to shared
category: arch
tags: promotion, shared, domain, placement
---

## Promotion rules for moving code to shared

**Promotion** = moving an element (component, hook, util, service, provider, type, config) from a domain module to `modules/shared/`.

**Promote when:** Used in 2+ modules or app root, not domain-coupled, not subdomain-scoped, and (primitive, loosely coupled, or app-wide config).

**Keep in module when:**
- Single-module use
- Domain-coupled (e.g. `useServicePurchaseModal` used in builders, finances, workstream—keep in `modules/service-purchase/`)
- Subdomain scope (shared only between subdomains of same parent, e.g. `services` and `service-purchase`)
- Speculative ("could be useful elsewhere")—promote only when actually used

**Incorrect:** Promoting domain-coupled code or promoting speculatively.

**Correct:** Promote `Button`, `formatCurrency` (if used across domains), feature flags config. Keep `useServicePurchaseModal` in service-purchase.

Reference: [architecture.md § Promotion rules](../architecture.md#promotion-rules)
