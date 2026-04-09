---
title: Helpers and pure logic outside component files
category: conv
tags: component, helpers, lib, utils
---

## Helpers and pure logic outside component files

Helper functions, formatters, and non-React utilities must not live inside component files. Place them in the module’s `lib/` (e.g. `modules/{module}/lib/`) or in a dedicated utils module/file. Component files should contain only the component, its props type (in the same file), and minimal JSX-related logic (e.g. simple derived state). Reusable types used by helpers or by multiple files belong in the module's types.ts (or types/ folder), not in lib or component files.

**Incorrect:**

```typescript
// transaction-list.tsx — helpers inside component file
function formatAmount(cents: number) {
  return (cents / 100).toFixed(2)
}
function TransactionList({ items }: Props) {
  return items.map((i) => <span>{formatAmount(i.amount)}</span>)
}
export { TransactionList }
```

**Correct:**

Put `formatAmount` in `modules/{module}/lib/` or a `utils` file; import it in the component. Component file contains only the component and minimal JSX logic.

Reference: [conventions.md](../conventions.md)
