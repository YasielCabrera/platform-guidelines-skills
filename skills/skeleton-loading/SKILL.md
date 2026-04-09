---
name: skeleton-loading
description: Create skeleton loading components that mirror existing UI layout. Use when creating skeleton loaders, loading placeholders, Suspense fallbacks, Next.js loading.tsx files, or any visual placeholder that must match the final layout to prevent layout shift.
---

# Skeleton Loading

Create skeleton loading states that mirror existing UI layout, preserve responsiveness, and prevent layout shift. Skeletons are visual-only placeholders — never interactive.

Always use the shared `Skeleton` component:

```tsx
import { Skeleton } from '@/shared/components/ui/skeleton'
```

## Workflow

Follow these steps in order:

1. **Read the target component** — understand its layout containers, responsive classes, and content elements.
2. **Create a sibling file** named `{component-name}-skeleton.tsx` next to the source component.
3. **Copy the layout structure** — keep every container, flex/grid wrapper, and spacing class identical.
4. **Replace content with `Skeleton` elements** — match dimensions using the sizing rules below.
5. **Remove all interactivity** — strip buttons, links, `onClick`, `href`, state, effects, and data fetching.
6. **Add element-mapping comments** — annotate each logical section (e.g. `{/* avatar */}`, `{/* title */}`).
7. **Validate** — confirm layout parity, correct sizing, responsive behavior, and sufficient contrast.

## Layout Mirroring

Keep the container hierarchy identical to the source component. This prevents layout shift when content loads.

- Preserve layout utilities: `flex`, `grid`, `absolute`, `relative`.
- Preserve all responsive classes: `sm:`, `md:`, `lg:`, `xl:`.
- Preserve spacing and sizing classes: `gap-*`, `p-*`, `m-*`, `w-*`, `h-*`.

## Sizing Rules

### Element Width

Use `w-full max-w-{value}` instead of fixed `w-{value}` so skeletons adapt on smaller viewports.

```tsx
// Do this — adapts to small screens
<Skeleton className="h-5 w-full max-w-32" />

// Not this — forces minimum width
<Skeleton className="h-5 w-32" />
```

**Exception:** Use fixed width for small elements that never exceed the viewport — icons (`size-4`, `size-5`), small standalone buttons.

**When the original has explicit width:** Preserve it exactly (e.g. `w-full`, `w-1/2`, or responsive `w-48 md:w-64 lg:w-80`).

### Text Line Height Conversion

Convert text line-height notation directly to skeleton height:

| Text class | Line height | Skeleton height |
|-----------|-------------|-----------------|
| `text-sm/5` | 20px | `h-5` |
| `text-base/6` | 24px | `h-6` |
| `text-lg/7` | 28px | `h-7` |

### Multi-line Text with `gap-1`

When creating multi-line skeletons for wrapping text and the original has no explicit gap between lines, use `gap-1` (4px) between skeleton lines. Subtract 2px from each line height to compensate for the added gap and prevent layout shift:

| Single-line height | Multi-line height (with `gap-1`) |
|-------------------|----------------------------------|
| `h-5` (20px) | `h-4.5` (18px) |
| `h-6` (24px) | `h-5.5` (22px) |
| `h-7` (28px) | `h-6.5` (26px) |

**Why subtract 2px?** The `gap-1` adds 4px total between two lines. Subtracting 2px from each line (4px total) keeps the combined height identical.

Apply across all breakpoints: `h-4.5 lg:h-5.5` for `text-sm/5 lg:text-base/6`.

**When to apply:** Only when the original has no explicit gap between text lines. If the original already has spacing (e.g. `gap-2`, `space-y-1`), match that spacing exactly without subtracting.

<example>
Original:
```tsx
<p className="text-sm/5 lg:text-base/6">
  Long paragraph that wraps to multiple lines with no gap between lines.
</p>
```

Skeleton:
```tsx
{/* description (multi-line paragraph) */}
<div className="flex flex-col gap-1">
  <Skeleton className="h-4.5 lg:h-5.5 w-full" />
  <Skeleton className="h-4.5 lg:h-5.5 w-1/3" />
</div>
```
</example>

## Content Replacement

- **Text** — replace with `Skeleton` matching line height and width. For paragraphs, use multiple lines and shorten the last one.
- **Icons** — match exact size: `<InfoIcon className="size-4" />` becomes `<Skeleton className="size-4" />`.
- **Avatars** — match size and add `rounded-full`: `<Avatar className="size-10" />` becomes `<Skeleton className="size-10 rounded-full" />`.
- **Badges/chips** — match height and use `rounded-full`: `<Skeleton className="h-6 w-20 rounded-full" />`.
- **Buttons** — replace with a Skeleton matching button height and approximate label width. Never render `Button`, `Link`, or `a` tags.
- **Typography classes** — remove `text-*`, `font-*`, `leading-*`, `tracking-*` from skeleton elements (they have no text to style).

<example>
Original:
```tsx
<div className="flex items-center justify-between gap-4">
  <div className="flex items-center gap-3">
    <Avatar className="size-10" />
    <div className="flex flex-col gap-1">
      <h2 className="text-base font-semibold">{name}</h2>
      <p className="text-sm text-muted-foreground">{role}</p>
    </div>
  </div>
  <div className="flex items-center gap-2">
    <Button variant="outline" size="sm">Cancel</Button>
    <Button size="sm">Save</Button>
  </div>
</div>
```

Skeleton:
```tsx
<div className="flex items-center justify-between gap-4">
  <div className="flex items-center gap-3">
    {/* avatar */}
    <Skeleton className="size-10 rounded-full" />
    <div className="flex flex-col gap-1">
      {/* title */}
      <Skeleton className="h-4 w-full max-w-32" />
      {/* subtitle */}
      <Skeleton className="h-3.5 w-full max-w-24" />
    </div>
  </div>
  <div className="flex items-center gap-2">
    {/* cancel button */}
    <Skeleton className="h-8 w-16" />
    {/* save button */}
    <Skeleton className="h-8 w-12" />
  </div>
</div>
```
</example>

## Contrast and Backgrounds

`Skeleton` defaults to `bg-accent`. When the parent background is accented or muted, skeletons become invisible. Add `bg-border` to restore contrast:

```tsx
<div className="bg-accent p-4">
  <Skeleton className="h-4 w-24 bg-border" />
</div>
```

Apply `bg-border` when the parent has `bg-accent`, `bg-muted`, or `bg-secondary`.

## Interactivity and Cleanup

- Remove `Button`, `Link`, `a`, and any `onClick`/`href`.
- Remove all state, effects, and data fetching.
- Avoid `'use client'` unless layout depends on runtime values.
- Strip non-visual attributes: `alt`, `src`, `target`, `rel`, `title`.

## Comments

- **Do not copy** comments from the original component (implementation notes, TODOs, business logic).
- **Do add** a short comment above each logical section indicating which real element it represents.

Use descriptive labels: `avatar`, `title`, `subtitle`, `description`, `card`, `transaction list`, `metrics row`, `badge`/`chip`, `action button`. Prefer one comment per logical block rather than commenting every single `<Skeleton>`.

## Composition and Reuse

- Place skeletons alongside their source components as `*-skeleton.tsx`.
- Reuse existing skeleton subcomponents when available.
- Keep skeletons static and composable — no props for dynamic behavior.
- Use absolute imports:

```tsx
import { Skeleton } from '@/shared/components/ui/skeleton'
import { cn } from '@/shared/lib/utils'
```

## Suspense and Route Loading

Use skeleton components as Suspense fallbacks:

```tsx
<Suspense fallback={<InfoCardSkeleton />}>
  <InfoCard />
</Suspense>
```

For Next.js route loading, add `loading.tsx` next to `page.tsx`. The loading UI must mirror the full page structure — not just one section.

<example>
Avoid (incomplete structure):
```tsx
// loading.tsx — only shows header, but page has header + body + list
export default function Loading() {
  return <PageHeaderSkeleton />
}
```

Prefer (same structure as the page):
```tsx
// loading.tsx — mirrors header + metrics + body sections
import { PageContent } from '@/shared/components/page-containers'
import { Skeleton } from '@/shared/components/ui/skeleton'

export default function Loading() {
  return (
    <PageContent className="mt-6 gap-6">
      {/* header: icon + title + chips + description */}
      <div className="flex flex-col gap-4">
        <div className="flex items-center gap-2">
          <Skeleton className="h-14 w-14 shrink-0" />
          <div className="flex min-w-0 flex-col gap-2">
            <Skeleton className="h-5.5 w-56" />
            <div className="flex flex-wrap gap-2">
              <Skeleton className="h-6 w-24" />
              <Skeleton className="h-6 w-24" />
            </div>
          </div>
        </div>
      </div>
      {/* body content */}
      <div className="flex flex-col gap-2">
        <Skeleton className="h-5.5 w-28" />
        <div className="flex flex-col gap-1">
          <Skeleton className="h-5 w-full" />
          <Skeleton className="h-5 w-full" />
          <Skeleton className="h-5 w-full max-w-4/5" />
        </div>
      </div>
    </PageContent>
  )
}
```
</example>

## Accessibility

- Skeletons are decorative — do not add screen-reader noise.
- Never render focusable elements (buttons, links, inputs) in skeletons.
- Avoid `aria-*` labels unless there is a specific accessibility requirement.
- To indicate loading, set `aria-busy="true"` on the parent container, not on each skeleton line.

## Validation Checklist

Before finishing, verify:

- [ ] Container structure matches the original layout exactly
- [ ] Sizes and spacing match original elements
- [ ] Responsive classes (`sm:`, `md:`, `lg:`, `xl:`) are preserved
- [ ] No interactive elements or data logic remain
- [ ] Skeleton contrast is sufficient on accented/muted backgrounds (`bg-border` where needed)
- [ ] Element-mapping comments are present on logical sections
- [ ] File is named `{component-name}-skeleton.tsx` and placed next to the source component

## References

- [skeleton-loading-examples.md](references/skeleton-loading-examples.md) — Full set of original-to-skeleton transformation patterns
