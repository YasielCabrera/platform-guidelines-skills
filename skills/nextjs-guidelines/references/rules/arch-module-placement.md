---
title: Place all code under modules/
category: arch
tags: placement, modules, architecture
---

## Place all code under modules/

All components, logic, hooks, and utilities must live under `modules/`. Nothing goes in a root-level `components/`, `app/components/`, or `src/components/`.

**Incorrect:**

```
app/components/transaction-list.tsx
components/shared/button.tsx
src/lib/utils.ts
```

**Correct:**

```
modules/transaction/components/transaction-list.tsx
modules/shared/components/ui/button.tsx
modules/shared/lib/utils.ts
```

Reference: [architecture.md](../architecture.md)
