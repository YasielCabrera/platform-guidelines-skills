---
title: Use feature flags for domain/section gating
category: ff
tags: feature flag, gating, environment
---

## Use feature flags for domain/section gating

Feature flags live in `modules/shared/lib/feature-flags/`. Use them to gate whole domains or sections (workstreams, roadmaps, finances sections, nav items), not for generic UI behavior.

**Incorrect:** Adding a one-off flag for a small UI tweak that could be config or component logic.

**Correct:** Gating a route or section that should be on in dev/staging and off (or different) in production.

```typescript
import ff from '@/shared/lib/feature-flags'

if (ff.workstreams.WORKSTREAMS_ENABLED) {
  // show workstreams nav / content
}
```

When adding a new flag: extend `FeatureFlags` in types.ts and set values in ff.dev, ff.staging, ff.production.

Reference: [feature-flags-and-env.md](../feature-flags-and-env.md)
