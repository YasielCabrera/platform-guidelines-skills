# Skeleton Loading Examples

Each example shows **original code** and its **skeleton** equivalent. Use these as patterns when building `*-skeleton.tsx` components. Skeleton snippets include **element-mapping comments** (e.g. `{/* avatar */}`, `{/* title */}`) to indicate which real UI element each part represents—see [skeleton-loading.md § Comments in Skeleton Code](skeleton-loading.md#7-comments-in-skeleton-code). Full rules: [skeleton-loading.md](skeleton-loading.md).

---

## 1. Header: title + icon + button

**Original:**
```tsx
<div className="flex justify-between gap-4">
  <div className="flex gap-2">
    <h1 className="text-xl leading-7 font-bold">Some text or variable</h1>
    <InfoIcon className="size-4" />
  </div>
  <Button variant="outline">Edit</Button>
</div>
```

**Skeleton:**
```tsx
<div className="flex justify-between gap-4">
  <div className="flex gap-2">
    {/* title */}
    <Skeleton className="h-7 w-full max-w-32" />
    {/* icon */}
    <Skeleton className="size-4" />
  </div>
  {/* action button */}
  <Skeleton className="h-9 w-20" />
</div>
```

- `text-xl leading-7` → line height 28px → `h-7`. No width on heading → `w-full max-w-*`.
- Icon `size-4` → `Skeleton className="size-4"`.
- Button (default height) → `Skeleton className="h-9 w-20"` (no interactive Button).

---

## 2. Header: avatar + title + subtitle + actions

**Original:**
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

**Skeleton:**
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

- Avatar → same size + `rounded-full`. Title/subtitle → line-height-based heights; use `max-w-*` when original has no width.
- Buttons removed; skeletons match `size="sm"` (`h-8`) and approximate label widths.

---

## 3. Card: title + paragraph + badge

**Original:**
```tsx
<div className="rounded-xl border p-4">
  <h3 className="text-lg font-medium">Card title</h3>
  <p className="mt-3 text-sm text-muted-foreground">
    First line of body text. Second line that wraps. Third shorter line.
  </p>
  <div className="mt-4">
    <Badge variant="secondary">Status</Badge>
  </div>
</div>
```

**Skeleton:**
```tsx
<div className="rounded-xl border p-4">
  {/* card title */}
  <Skeleton className="h-5 w-full max-w-40" />
  {/* description (paragraph) */}
  <div className="mt-3 flex flex-col gap-2">
    <Skeleton className="h-4 w-full" />
    <Skeleton className="h-4 w-full max-w-5/6" />
    <Skeleton className="h-4 w-full max-w-2/3" />
  </div>
  <div className="mt-4">
    {/* badge */}
    <Skeleton className="h-6 w-20 rounded-full" />
  </div>
</div>
```

- Title `text-lg` → ~`h-5`. Paragraph → multiple lines, last line shorter. Badge → pill-shaped skeleton.

---

## 4. List row: icon + label + chip

**Original:**
```tsx
<div className="flex items-center justify-between gap-3 py-2">
  <div className="flex items-center gap-2">
    <CheckIcon className="size-5" />
    <span className="text-sm">Item label or name</span>
  </div>
  <Badge variant="outline">Active</Badge>
</div>
```

**Skeleton:**
```tsx
<div className="flex items-center justify-between gap-3 py-2">
  <div className="flex items-center gap-2">
    {/* icon */}
    <Skeleton className="size-5 rounded-sm" />
    {/* label */}
    <Skeleton className="h-4 w-full max-w-36" />
  </div>
  {/* chip */}
  <Skeleton className="h-5 w-16 rounded-full" />
</div>
```

- Icon size and shape preserved. Label → single-line skeleton. Chip → rounded-full skeleton matching badge height.

---

## 5. Multi-line text with line-height (subtract 2px per line when using gap-1)

**Original:**
```tsx
<p className="text-sm/5 lg:text-base/6">
  Long paragraph that wraps to multiple lines with no gap between lines.
</p>
```

**Skeleton:**
```tsx
{/* description (multi-line paragraph) */}
<div className="flex flex-col gap-1">
  <Skeleton className="h-4.5 lg:h-5.5 w-full" />
  <Skeleton className="h-4.5 lg:h-5.5 w-1/3" />
</div>
```

- `text-sm/5` → line height 20px → `h-5`; with `gap-1` between lines, use `h-4.5` (subtract 2px per line). Same for `lg:text-base/6` → `lg:h-5.5`. Last line shorter (`w-1/3`).

---

## 6. Skeleton on accented background (use bg-border)

**Original:**
```tsx
<div className="bg-accent p-4 rounded-lg">
  <p className="text-sm">Content inside accent block</p>
  <Button variant="ghost" size="sm">Action</Button>
</div>
```

**Skeleton:**
```tsx
<div className="bg-accent p-4 rounded-lg">
  {/* content text */}
  <Skeleton className="h-4 w-full max-w-48 bg-border" />
  {/* action button */}
  <Skeleton className="mt-2 h-8 w-16 bg-border" />
</div>
```

- Parent has `bg-accent` → add `bg-border` to skeletons so they remain visible. Layout and sizes mirrored; Button replaced by skeleton.

---

## 7. Responsive widths

**Original:**
```tsx
<h1 className="text-2xl font-bold w-48 md:w-64 lg:w-80">{title}</h1>
```

**Skeleton:**
```tsx
{/* title */}
<Skeleton className="h-7 w-48 md:w-64 lg:w-80" />
```

- Original has explicit responsive widths → preserve them exactly. `text-2xl` line height → `h-7`.

---

## 8. Suspense and loading.tsx

Use the skeleton component as fallback or route loading UI:

```tsx
<Suspense fallback={<CardSkeleton />}>
  <Card />
</Suspense>
```

**loading.tsx: structure must mirror the page.** Avoid rendering only one skeleton component—the layout and sections should match what the page will show so the transition from loading to content feels stable (no layout shift, same structure).

**Avoid (single component only unless that is how the page is designed):**
```tsx
// app/.../loading.tsx
export default function Loading() {
  return <PageHeaderSkeleton />
}
```
This suggests the page is just a header; if the page has header + body + list, the loading UI should reflect that.

**Prefer (same structure as the page):**
```tsx
// app/.../loading.tsx
import { PageContent } from '@/shared/components/page-containers'
import { Skeleton } from '@/shared/components/ui/skeleton'

export default function Loading() {
  return (
    <PageContent className="mt-6 gap-6">
      {/* header: icon + title + chips + description */}
      <div className="flex flex-col gap-4">
        <div className="flex w-full flex-col gap-4 lg:flex-row">
          <div className="flex flex-col gap-2 lg:flex-2">
            <div className="flex items-center gap-2">
              {/* icon */}
              <Skeleton className="h-14 w-14 shrink-0" />
              <div className="flex min-w-0 flex-col gap-2">
                {/* title */}
                <Skeleton className="h-5.5 w-56" />
                {/* chips */}
                <div className="flex flex-wrap gap-2">
                  <Skeleton className="h-6 w-24" />
                  <Skeleton className="h-6 w-24" />
                  <Skeleton className="h-6 w-24" />
                </div>
              </div>
            </div>
            {/* description */}
            <Skeleton className="ml-16 h-5.5 w-full max-w-md" />
          </div>
          {/* metrics row (each card: label + value) */}
          <div className="flex w-full flex-col gap-2 sm:flex-row sm:gap-2 lg:w-86 lg:flex-col xl:w-108">
            {[1, 2, 3].map((i) => (
              <div
                key={i}
                className="border-input flex min-h-9.5 w-full items-center justify-between rounded-xl border p-2 sm:gap-2"
              >
                <Skeleton className="h-4.5 w-24 shrink-0" />
                <Skeleton className="h-5.5 w-20 shrink-0" />
              </div>
            ))}
          </div>
        </div>
        <div className="flex flex-col gap-2 lg:flex-1">
          {/* heading */}
          <Skeleton className="h-5.5 w-28" />
          <div className="flex flex-col gap-1">
            {/* description (paragraphs) */}
            <Skeleton className="h-5 w-full" />
            <Skeleton className="h-5 w-full" />
            <Skeleton className="h-5 w-full" />
            <Skeleton className="h-5 w-full max-w-4/5" />
          </div>
        </div>
      </div>
    </PageContent>
  )
}
```

- Same wrapper (`PageContent`), same sections and layout (header row, metrics, body) as the real page.
- Each block uses skeletons that match the sizes and spacing of the real components (headings, chips, metrics cards, paragraphs).
- Result: no layout shift when data loads; users see the same structure with placeholders instead of content.
