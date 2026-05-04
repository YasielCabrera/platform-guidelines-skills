---
title: Component directory, named function, named export
category: conv
tags: component, export, function
---

## Component directory, named function, named export

Each component lives in a directory named after the component (kebab-case), with a main file and an `index.ts` that re-exports it. Use a named function (not an arrow) and a named export at the end.

**Incorrect:**

```typescript
// Default export or arrow function
const Button = () => <button>Click</button>
export default Button
```

**Correct:**

```typescript
function Button({ children }: ButtonProps) {
  return <button>{children}</button>
}
export { Button }
```

Directory: `component-name/component-name.tsx` + `component-name/index.ts` with `export { ComponentName } from './component-name'`.

Reference: [conventions.md](../conventions.md)
