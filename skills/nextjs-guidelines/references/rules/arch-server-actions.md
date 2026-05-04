---
title: Server actions in actions/, one per file, suffix action
category: arch
tags: server-actions, actions, nextjs
---

## Server actions in actions/, one per file, suffix action

Next.js server actions must live in `modules/<module>/actions/`. Each file must contain exactly one action. Both the file name and the exported action function must use the suffix `action` (file: kebab-case, e.g. `submit-transaction-action.ts`; function: camelCase, e.g. `submitTransactionAction`).

**Incorrect:**

```typescript
// modules/transaction/lib/submit-transaction.ts — wrong folder; missing suffix
'use server'
export async function submitTransaction() { ... }

// modules/transaction/actions/transactions.ts — multiple actions in one file; wrong file name
'use server'
export async function submitTransactionAction() { ... }
export async function deleteTransactionAction() { ... }

// modules/transaction/actions/submit-transaction-action.ts — function missing suffix
'use server'
export async function submitTransaction() { ... }
```

**Correct:**

```typescript
// modules/transaction/actions/submit-transaction-action.ts
'use server'
export async function submitTransactionAction(formData: FormData) {
  // ...
}

// modules/builders/actions/create-builder-action.ts
'use server'
export async function createBuilderAction(formData: FormData) {
  // ...
}
```

Reference: [architecture.md](../architecture.md)
