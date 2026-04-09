---
title: Constants in lib/constants.ts
category: conv
tags: constants, lib, conventions
---

## Constants in lib/constants.ts

Module-level constants (magic numbers, config values, string literals, option arrays, default values, etc.) must live in a single `constants.ts` file in the module's `lib/` folder: `modules/{module}/lib/constants.ts`. Each module has **at most one** `constants.ts` file — do not create multiple constants files or nest them in subfolders.

**Incorrect (what to avoid):**

```typescript
// modules/expense-reports/lib/balance/constants.ts — wrong: nested in a subfolder
export const MAX_ITEMS = 50

// modules/expense-reports/constants.ts — wrong: at module root instead of lib/
export const DEFAULT_CURRENCY = 'USD'

// modules/expense-reports/lib/format-constants.ts — wrong: separate constants file per concern
export const DATE_FORMAT = 'YYYY-MM-DD'

// modules/expense-reports/components/transaction-list/transaction-list.tsx — wrong: shared constant in component
const STATUS_OPTIONS = ['pending', 'approved', 'rejected'] as const
```

**Correct (what to do):**

```typescript
// modules/expense-reports/lib/constants.ts — single constants file in lib/
export const MAX_ITEMS = 50
export const DEFAULT_CURRENCY = 'USD'
export const DATE_FORMAT = 'YYYY-MM-DD'
export const STATUS_OPTIONS = ['pending', 'approved', 'rejected'] as const

// modules/expense-reports/components/transaction-list/transaction-list.tsx — import from constants
import { STATUS_OPTIONS } from '@/modules/expense-reports/lib/constants'
```

Reference: [conventions.md](../conventions.md)

Full rules: [conventions.md § Constants placement](../conventions.md#constants-placement)
