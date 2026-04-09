---
title: One component per file
category: conv
tags: component, file, structure
---

## One component per file

Each component file (`*.tsx`) must contain exactly one component (the one that matches the file name). Additional components used only by that component belong in separate files in the same directory (subcomponents). Do not define multiple unrelated components in a single file.

**Incorrect:**

```typescript
// user-card.tsx — multiple components in one file
function UserCard() { ... }
function UserAvatar() { ... }
function UserName() { ... }
export { UserCard }
```

**Correct:**

One component per file: `user-card.tsx`, `user-avatar.tsx`, `user-name.tsx` in the same directory; re-export only what is used elsewhere.

### Exception: Composition-pattern components

Compound components that export multiple subcomponents as a single object may keep all subcomponents in one file when:

- The component exports a compound object (e.g. `export const Card = { Root, Header, Row }`).
- Subcomponents are used together as `<Component.Root>`, `<Component.Header>`, etc.
- Subcomponents are tightly coupled and not reused elsewhere.
- Each subcomponent is small (~30 lines or less).

If subcomponents grow large or are reused elsewhere, split them into separate files per the default rule.

**Correct (composition-pattern exception):**

```typescript
// card.tsx — compound component, all subcomponents in one file
function Root({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}
function Header({ children }: { children: React.ReactNode }) {
  return <header>{children}</header>
}
function Row({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}

export const Card = { Root, Header, Row }
```

```jsx
// Usage:
<Card.Root>
  <Card.Header>Title</Card.Header>
</Card.Root>
```

Reference: [conventions.md](../conventions.md)
