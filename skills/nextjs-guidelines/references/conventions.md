# Conventions

**Version 0.1.0 (draft)**

Naming, file, and export conventions so code stays consistent. These are the source of truth for component structure and exports in this codebase.

## Naming

### Files and directories

Use **kebab-case** for all file and directory names.

- **Do**: `transaction-list.tsx`, `use-transaction-form.ts`, `transaction-service.ts`, `app-sidebar.tsx`
- **Don’t**: `transactionList.tsx` (camelCase), `transaction_list.tsx` (snake_case), `TransactionList.tsx` (PascalCase)

### Component files

- Component file name must match the component name in kebab-case (e.g. component `UserCard` → file `user-card.tsx`).
- Use `.tsx` for React components.
- Use `.ts` for utilities, hooks, and types.

## Component directory and exports

### One directory per component

Each component lives in a directory named after the component (kebab-case).

**Structure**:

```
modules/{module}/components/{component-name}/
  ├── {component-name}.tsx   # Main component
  ├── {component-name}-skeleton.tsx   # skeleton for the component (optional)
  ├── index.ts               # Re-export (required)
  └── {subcomponent}.tsx     # Subcomponents (optional)
```

**Example**:

```
modules/shared/components/app-sidebar/
  ├── app-sidebar.tsx
  ├── nav-main.tsx
  ├── nav-secondary.tsx
  ├── nav-user.tsx
  └── index.ts
```

### One component per file

Each component file (`*.tsx`) must contain **exactly one** component (the one that matches the file name).

- Additional components used only by that component belong in **separate files** in the same directory (subcomponents). Do not define multiple unrelated components in a single file.
- **Exception:** Composition-pattern components may keep all subcomponents in the main file when they are small, tightly coupled, and exported as a single compound object (e.g. `export const Card = { Root, Header, Row }`). See [conv-one-component-per-file](rules/conv-one-component-per-file.md).

### Helpers and pure logic

Helper functions, formatters, and non-React utilities must **not** live inside component files. Place them in the module’s `lib/` (e.g. `modules/{module}/lib/`) or in a dedicated utils module/file.

- Component files should contain only the component, its props type (in the same file), and minimal JSX-related logic (e.g. simple derived state). This keeps components readable and logic testable and reusable.

### Types placement

**Summary:** Component props in component file; reusable types at module root only (`types.ts` or `types/`). Never in lib/, utils/, or subfolders. No index.ts or types.ts inside types/.

- **Component props:** MUST live in the same component file. Do not move props types to a separate file (no component-local `types.ts` for props).
- **Reusable / intermediate types** (used in more than one file in the same module, or across modules) must live **only at the root of the module** — never inside `lib/`, `utils/`, or other subfolders:
  - **Default:** a single **types.ts** file at module root (e.g. `modules/{module}/types.ts`, `modules/shared/types.ts`).
  - **When types.ts grows too much:** use a **types/** folder at module root and split into domain-specific files (e.g. `modules/{module}/types/transaction.ts`, `modules/{module}/types/something.ts`, `modules/{module}/types/something-else.ts`). The types folder has **no** `index.ts` and **no** `types.ts` inside it; types are exported directly from the file they live in. Consumers import from the specific file (e.g. `from '@/modules/expense-reports/types/transaction'`).
- In types (file or folder) live: **interfaces**, **type** aliases, and **enums**.
- **Do not** put reusable types in `lib/`, `utils/`, or any other subfolder; do not define them in component files. Export them only from the module root `types.ts` or from a file inside the module root `types/` folder and import where needed.
- **Exception:** The feature-flags module (`modules/shared/lib/feature-flags/`) is a self-contained submodule with its own `types.ts` co-located with its implementation (ff.dev.ts, ff.staging.ts, etc.). This is intentional; do not move feature-flags types to `modules/shared/types/`.

### Constants placement

**Summary:** Module-level constants in `modules/{module}/lib/constants.ts` only. One `constants.ts` per module; never at module root, in subfolders of lib/, or in component files.

- **Module-level constants** (magic numbers, config values, string literals, option arrays, default values, etc.) must live in a single **`constants.ts`** file inside the module's `lib/` folder: `modules/{module}/lib/constants.ts`.
- Each module has **at most one** `constants.ts`. Do not create multiple constants files (e.g. `lib/format-constants.ts`, `lib/api-constants.ts`) or nest them in subfolders (e.g. `lib/balance/constants.ts`).
- Do not place `constants.ts` at the module root (e.g. `modules/{module}/constants.ts`); it belongs inside `lib/`.
- Use **UPPER_SNAKE_CASE** for constant names and **named exports**.

**Don't** (constants scattered):

```typescript
// modules/expense-reports/constants.ts — wrong: at module root instead of lib/
export const DEFAULT_CURRENCY = 'USD'

// modules/expense-reports/lib/balance/constants.ts — wrong: nested in a subfolder
export const MAX_ITEMS = 50

// modules/expense-reports/components/transaction-list/transaction-list.tsx — wrong: shared constant in component file
const STATUS_OPTIONS = ['pending', 'approved', 'rejected'] as const
```

**Do** (single constants file in lib/):

```typescript
// modules/expense-reports/lib/constants.ts
export const DEFAULT_CURRENCY = 'USD'
export const MAX_ITEMS = 50
export const STATUS_OPTIONS = ['pending', 'approved', 'rejected'] as const

// modules/expense-reports/components/transaction-list/transaction-list.tsx — import from constants
import { STATUS_OPTIONS } from '@/modules/expense-reports/lib/constants'
```

See [conv-constants-placement](rules/conv-constants-placement.md).

**Don’t** (reusable type in utils):

```typescript
// modules/expense-reports/lib/balance-utils.ts — wrong: reusable type in lib
export interface CalculatedBalance {
  startingBalance: number
  endingBalance: number
}
export function getBalance(/* ... */): CalculatedBalance { /* ... */ }
```

**Do** (reusable type in types.ts, import in lib):

```typescript
// modules/expense-reports/types.ts
export interface CalculatedBalance {
  startingBalance: number
  endingBalance: number
}

// modules/expense-reports/lib/balance-utils.ts
import type { CalculatedBalance } from '@/modules/expense-reports/types'
export function getBalance(/* ... */): CalculatedBalance { /* ... */ }
```

### Index file

Every component directory must have an `index.ts` that re-exports the main component:

```typescript
export { ComponentName } from './component-name'
```

Subcomponents stay in the same directory but are **not** re-exported from `index.ts` unless they are used outside this component directory.

### Function declaration and export

- Use **named function declarations**, not arrow functions.
- Use **named export** at the end of the file (e.g. `export { ComponentName }`).
- Do not use default exports for components.

**Do**:

```typescript
interface ButtonProps {
  children: React.ReactNode
  onClick?: () => void
  variant?: 'primary' | 'secondary'
}

function Button({ children, onClick, variant = 'primary' }: ButtonProps) {
  return (
    <button onClick={onClick} data-variant={variant}>
      {children}
    </button>
  )
}

export { Button }
```

**Don’t**:

```typescript
const Button = ({ children }: ButtonProps) => <button>{children}</button>
export default function Button() {}
export function Button() {}  // Prefer export { Button } at end
```

## Subcomponents

- Place subcomponents in the **same directory** as the main component.
- Use kebab-case for their file names.
- Use the same function + named-export pattern.
- Re-export from `index.ts` only if they are used outside this directory.
- **Exception:** Composition-pattern components may keep all subcomponents in the main file when they are small, tightly coupled, and exported as a single compound object.

## Quick reference

| Item | Convention |
|------|------------|
| Files and dirs | kebab-case |
| Component file | `{component-name}.tsx` matching component name |
| Component dir | `{component-name}/` with `{component-name}.tsx` + `index.ts` |
| Export | Named: `export { ComponentName }` at end of file |
| Function | Named function, not arrow |
| Subcomponents | Same dir; re-export only when used elsewhere |
| One component per file | Each `.tsx` file contains exactly one component; others in separate files; exception for composition-pattern components |
| Helpers | In `lib/` or utils, not in component files |
| Component props | In the same component file only |
| Reusable types | At module root only: `types.ts` or `types/` (e.g. types/transaction.ts); never in lib/, utils/, or other subfolders; no index or inner types.ts; interfaces, type aliases, enums |
| Constants | One `constants.ts` per module in `lib/` (`modules/{module}/lib/constants.ts`); UPPER_SNAKE_CASE; named exports |
