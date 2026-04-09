---
title: Reusable types in types.ts (or types/ folder)
category: conv
tags: types, conventions, types.ts, component-props
---

## Reusable types in types.ts (or types/ folder)

Reusable interfaces, type aliases, and enums (used in more than one file in the same module or across modules) must live only at the root of the module: in `types.ts` or in a file under the root-level `types/` folder (e.g. `types/transaction.ts`, `types/something.ts`). Do not put types.ts in lib/, utils/, or other subfolders. Component props must stay in the same component file.

**Incorrect (what to avoid):**

```typescript
// lib/utils.ts — wrong: reusable type in utils
export interface CalculatedBalance {
  startingBalance: number
  endingBalance: number
}
export function getBalance(): CalculatedBalance { /* ... */ }

// lib/subfeature/types.ts — wrong: types.ts inside a lib subfolder (all types at module root only).
// Exception: modules/shared/lib/feature-flags/types.ts is intentional—feature-flags is a self-contained submodule.
export interface SubfeatureConfig { /* ... */ }

// component-name.tsx — wrong: props in separate file
// Props type lives in types.ts or another file
```

**Correct (what to do):**

```typescript
// modules/{module}/types.ts (or modules/{module}/types/transaction.ts when using types/ folder at module root)
export interface CalculatedBalance {
  startingBalance: number
  endingBalance: number
}

// lib/utils.ts — import reusable type
import type { CalculatedBalance } from '@/modules/{module}/types'
export function getBalance(): CalculatedBalance { /* ... */ }

// component-name.tsx — props in same file
interface ButtonProps {
  children: React.ReactNode
  variant?: 'primary' | 'secondary'
}
function Button({ children, variant = 'primary' }: ButtonProps) { /* ... */ }
```

Reference: [conventions.md](../conventions.md), [architecture.md](../architecture.md)

Full rules: [conventions.md § Types placement](../conventions.md#types-placement)
