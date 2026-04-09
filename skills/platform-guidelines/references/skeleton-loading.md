# Skeleton Loading Guidelines

Guidelines for creating skeleton loading states that mirror existing UI layout, preserve responsiveness, and use the shared `Skeleton` component consistently. See [conventions.md](conventions.md) for file naming (`{component-name}-skeleton.tsx`). Add `loading.tsx` next to `page.tsx` in the same segment for route loading.

---

## 1. Core Principles

- Skeletons are visual-only placeholders, not interactive UI.
- Preserve the original layout to prevent layout shift.
- Prefer shared patterns and existing skeleton components.
- Use the shared `Skeleton` component from `@/shared/components/ui/skeleton`.

---

## 2. Layout Mirroring

- Keep container hierarchy identical to the source component.
- Preserve layout utilities (`flex`, `grid`, `absolute`, `relative`).
- Preserve all responsive classes (`sm:`, `md:`, `lg:`, `xl:`).
- Keep spacing and sizing classes (`gap-*`, `p-*`, `m-*`, `w-*`, `h-*`).

---

## 3. Content Replacement and Typography

- Replace text with `Skeleton` matching the original line height and width.
- For paragraphs, use multiple lines and shorten the last line.
- Replace icons, avatars, and images with same-size skeletons.
- Remove typography classes (`text-*`, `font-*`, `leading-*`, `tracking-*`) from skeletons.

---

## 4. Sizing Rules

- Skeletons must match the exact dimensions of the original elements.
- If height is implicit, derive it from padding and line-height.
- Prefer explicit `h-*` and `w-*` to avoid layout shift.

### Element width and responsiveness

Prefer `w-full max-w-{value}` instead of fixed `w-{value}` so the skeleton adapts on smaller viewports (e.g. `w-full max-w-32` rather than `w-32`). That way the skeleton can shrink with the layout instead of forcing a minimum width.

**Exception:** Use fixed width (`w-*`) for really small elements that will never exceed the viewport by layout—for example small icons (`size-4`, `size-5`), or a small button that sits alone in a row. For those, fixed dimensions are fine.

### Text Line Height Conversion

Text with line height notation (e.g., `text-sm/5`, `text-base/6`) means the text size with a specific line-height. Convert the line height value directly to skeleton height:

- `text-sm/5` → `h-5` (20px line height)
- `text-base/6` → `h-6` (24px line height)
- `text-lg/7` → `h-7` (28px line height)

**Multi-line text with `gap-1`:** When creating multi-line skeletons for text that wraps and the original has no explicit gap between lines, use `gap-1` (4px) between skeleton lines. To prevent layout shift, **subtract 2px from each line height** to compensate for the gap:

- `h-5` → `h-4.5` (18px = 20px - 2px)
- `h-6` → `h-5.5` (22px = 24px - 2px)
- `h-7` → `h-6.5` (26px = 28px - 2px)

**Why subtract 2px?** The `gap-1` adds 4px total spacing between two lines. To maintain the same total height, subtract 2px from each line (4px total compensation).

Apply the same rule across all breakpoints (e.g. `h-4.5 lg:h-5.5` for `text-sm/5 lg:text-base/6`).

**When to apply:** Original has text with line height notation, text wraps to multiple lines, skeleton uses multiple lines with `gap-1`, and the original has no explicit gap between text lines. If the original already has spacing (e.g. `gap-2`, `space-y-1`), match that spacing exactly without subtracting from line heights.

### Text Element Width Handling

For text elements (headings, paragraphs, labels, captions), follow the same rule as above: use `w-full max-w-{value}` when the original has no width, so the skeleton adapts on smaller screens:

```tsx
// Original: <p className="text-sm">Some text content</p>
<Skeleton className="h-4.5 w-full max-w-24" />
```

**When the original has explicit width:** Preserve it (e.g. `w-full`, `w-1/2`, or responsive `w-48 md:w-64 lg:w-80`). For containers, icons, and images, use fixed size matching the original; small icons and small standalone controls are covered by the exception above.

---

## 5. Contrast and Backgrounds

`Skeleton` defaults to `bg-accent`. When the parent background is accented or muted, add `bg-border`:

```tsx
<div className="bg-accent p-4">
  <Skeleton className="h-4 w-24 bg-border" />
</div>
```

Apply `bg-border` for parents with `bg-accent`, `bg-muted`, or `bg-secondary`.

---

## 6. Interactivity and Cleanup

- Remove `Button`, `Link`, `a`, and any `onClick`/`href`.
- Remove all state, effects, and data fetching.
- Avoid `'use client'` unless layout depends on runtime values.
- Strip non-visual attributes (`alt`, `src`, `target`, `rel`, `title`).

---

## 7. Comments in Skeleton Code

- **Do not copy** comments from the original component (e.g. implementation notes, TODOs, or business logic).
- **Do add** a short comment above or beside each logical section of the skeleton to indicate which real element it stands for. This keeps the skeleton readable and makes it easier to keep it in sync with the source UI.

Use descriptive labels such as: `avatar`, `title`, `subtitle`, `description`, `card`, `transaction list`, `metrics row`, `badge` / `chip`, `action button`, etc. Prefer one comment per logical block (e.g. one for the whole "header" row, or one per card in a list) rather than commenting every single `<Skeleton>`.

See [skeleton-loading-examples.md](skeleton-loading-examples.md) for examples with these comments applied.

---

## 8. Composition and Reuse

- Place skeletons alongside their source components using `*-skeleton.tsx` (see [conventions.md](conventions.md)).
- Reuse existing skeleton subcomponents when available.
- Keep skeletons static and composable.

---

## 9. Imports

Use absolute imports:

```tsx
import { Skeleton } from '@/shared/components/ui/skeleton'
import { cn } from '@/shared/lib/utils'
```

---

## 10. Suspense and Route Loading

Use skeletons as fallbacks:

```tsx
<Suspense fallback={<InfoCardSkeleton />}>
  <InfoCard />
</Suspense>
```

For Next.js route loading, add `loading.tsx` next to `page.tsx` and export a default component that renders the appropriate skeleton:

```tsx
export default function Loading() {
  return <PageHeaderSkeleton />
}
```

---

## 11. Accessibility

- Skeletons are decorative; avoid adding screen-reader noise.
- Do not render focusable elements (buttons, links, inputs) in skeletons.
- Avoid `aria-*` labels unless there is a specific accessibility requirement.
- If a wrapper needs to indicate loading, set `aria-busy="true"` on the parent container, not on each skeleton line.

---

## 12. Validation Checklist

- [ ] Structure matches the original layout
- [ ] Sizes and spacing match original elements
- [ ] Responsive classes are preserved
- [ ] No interactive elements or data logic
- [ ] Skeleton contrast is sufficient on muted backgrounds

---

## References

- [skeleton-loading-examples.md](skeleton-loading-examples.md) — Patterns and code examples
- [conventions.md](conventions.md) — Component directory and `*-skeleton.tsx` naming
