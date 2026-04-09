# Storybook Stories

## Purpose

Stories in Achra serve two goals:

1. **Component documentation** — primarily for shared/reusable components, so developers understand available variants and props
2. **Visual regression testing via Chromatic** — Chromatic runs on every PR and fails if it detects unexpected UI changes, making stories the primary regression safety net

---

## When to Create a Story

**Create a story for:**
- Reusable components (especially anything in `modules/shared/`)
- Section-level components (e.g., `ProposalsSection`, `WalletsSection`, `RoadmapSwiper`)
- Page-level components where meaningful UI regression detection is possible

**Do not create a story for:**
- One-time small components used only in a single parent — write the story on the parent section or page component instead
- Pure layout wrappers with no visual logic
- Internal sub-components that are never rendered independently
- Components that are rendered as part of other component which already have Stories

The goal is maximum regression coverage with minimum story files. When in doubt, prefer one story on the parent component over several stories on its children.

---

## Variants Policy

Only create story variants that produce **visually distinct UI states** relevant for regression detection:

| Good variant | Reason |
|---|---|
| `Default` | The typical happy-path rendering |
| `Loading` | Skeleton or spinner state (visually distinct) |
| `Empty` | Zero-data state (different layout) |
| `Error` | Error state (different UI) |
| Data-shape variants | E.g., `SingleProposal` vs many — affects layout |

**Avoid:**
- Variants that only differ by a single color prop or class
- Variants covering every boolean prop permutation
- Variants that look identical in Chromatic snapshots
- Variants that represent quantity elements and don't show relevant UI changes

Aim for the **minimum set** that exercises all meaningfully different UI states.

---

## File Placement and Naming

Colocate the story file with the component it documents:

```
modules/networks/components/proposals-section/
  proposals-section.tsx
  proposals-section.stories.tsx   ← here
  mocks/
    proposals.ts
```

File name: `[component-name].stories.tsx`

---

## Story Structure

Always use Component Story Format (CSF 3) with TypeScript:

```typescript
import type { Meta, StoryObj } from '@storybook/nextjs-vite'
import { MyComponent } from './my-component'

const meta = {
  title: 'Modules/Networks/Components/MyComponent',
  component: MyComponent,
  parameters: {
    layout: 'fullscreen',  // 'centered' for isolated UI components
  },
} satisfies Meta<typeof MyComponent>

export default meta
type Story = StoryObj<typeof meta>

export const Default: Story = {
  args: {
    // ...
  },
}
```

**Always import from `@storybook/nextjs-vite`**, not `@storybook/react`.

---

## Args and ArgTypes

Document the component's args in the meta via `argTypes`, including their types and descriptions. This improves the Storybook Controls panel and serves as inline documentation for developers.

```typescript
const meta = {
  title: 'Modules/Example/Components/MyComponent',
  component: MyComponent,
  argTypes: {
    variant: {
      control: 'select',
      options: ['default', 'outline', 'ghost'],
      description: 'Visual style variant of the component',
    },
    disabled: {
      control: 'boolean',
      description: 'Whether the component is disabled',
    },
    size: {
      control: 'select',
      options: ['sm', 'md', 'lg'],
      description: 'Size of the component',
    },
  },
} satisfies Meta<typeof MyComponent>
```

- **Type**: Use `control` (e.g. `'boolean'`, `'select'`, `'text'`) and optionally `options` for enums
- **Description**: Add a `description` string for each arg to explain its purpose
- Prefer `argTypes` over inline comments — they surface in Storybook's docs and Controls

---

## Title Convention

| Component type | Title pattern | Example |
|---|---|---|
| Shared UI component | `Shared/{Category}/{ComponentName}` | `Shared/Shadcn/Button` |
| Domain component | `Modules/{Module}/Components/{ComponentName}` | `Modules/Networks/Components/ProposalsSection` |
| Nested domain component | `Modules/{Module}/Components/{Parent}/{ComponentName}` | `Modules/Networks/Components/GovernanceSection/ForumOverview` |

---

## Layout Parameter

```typescript
parameters: {
  layout: 'fullscreen',  // sections, pages, full-width components
  layout: 'centered',    // isolated UI components (buttons, cards, inputs)
  layout: 'padded',      // components that need some surrounding space
}
```

---

## Shared Decorators

Import shared decorators from `modules/shared/lib/decorators.tsx`. Apply them at the meta level (affects all stories) or per-story.

```typescript
import {
  withNuqsAdapter,
  withReactQueryProvider,
  withPortalFontStyles,
} from '@/modules/shared/lib/decorators'
```

| Decorator | When to use |
|---|---|
| `withNuqsAdapter` | Component reads URL search params via `nuqs` |
| `withReactQueryProvider` | Component uses TanStack Query (`useQuery`, `useMutation`) |
| `withPortalFontStyles` | Component renders portals, tooltips, menus, or overlays |
| `withNextjsExtras` | Applied globally — adds fonts; DO NOT add manually |

The `withNextjsExtras` and theme decorators are registered globally in `.storybook/preview.ts` and apply to all stories automatically.

**Provider requirements:** If a component uses a provider that lives outside the component (e.g. `QueryClientProvider`, `NuqsAdapter`, `CustomModalProvider`, etc), wrap the story with that provider using a decorator. This ensures the component has the same context it would have in the app.

**Adding new decorators:**
- App-level decorators → `modules/shared/lib/decorators.tsx`
- Module-specific decorators → `<module>/lib/decorators.ts` (e.g. `modules/expense-reports/lib/decorators.ts`)
- Before creating a decorator, check that no existing decorator already provides the needed provider or behavior.
- Avoid inline decorators — add reusable decorators to the appropriate file instead.

---

## MSW for API-Dependent Components

**All components that perform API calls MUST be mocked** to avoid real network requests. This applies to both **queries** (GET) and **mutations/actions** (POST, PUT, PATCH, DELETE, etc.). Never let stories hit real APIs.

Use [MSW](https://mswjs.io) to mock HTTP requests in stories. The global `mswLoader` is already registered in `.storybook/preview.ts`.

```typescript
import { delay, http, HttpResponse } from 'msw'

export const Default: Story = {
  parameters: {
    msw: {
      handlers: [
        http.get('/api/my-endpoint', () => HttpResponse.json(mockedData)),
      ],
    },
  },
}

// Loading state — delay indefinitely
export const Loading: Story = {
  parameters: {
    msw: {
      handlers: [
        http.get('/api/my-endpoint', async () => {
          await delay('infinite')
        }),
      ],
    },
  },
}

// Error state
export const Error: Story = {
  parameters: {
    msw: {
      handlers: [
        http.get('/api/my-endpoint', () => HttpResponse.error()),
      ],
    },
  },
}
```

---

## Mock Data

Place mock data in the module's `mocks/` directory when it is substantial or reused. **Keep small, one-off mock data inline** (<10 lines) to reduce friction — avoid moving it to a dedicated file unless it is reused elsewhere.

```
modules/roadmap/
  mocks/
    roadmap.ts       ← export mockedMilestones, mockedDeliverables, etc.
  components/
    roadmap-swiper/
      roadmap-swiper.stories.tsx  ← import from '@/modules/roadmap/mocks'
```

Keep mock data realistic and representative of production data shapes.

### Keep inline (do not move to a dedicated file)

- Array of short strings with just a few elements (e.g. `['Option A', 'Option B', 'Option C']`)
- JSX code that is not excessively large
- Small–medium array of JSX elements
- Objects with a few keys that total ~10 lines or less
- Simple primitives or enums used as a single arg (e.g. `status: 'active'`)
- One-off fixture for a single story that is unlikely to be reused

### Move to a dedicated mock file

- Mock data reused in multiple stories or components
- Large objects or arrays (many keys, nested structures, 10+ lines)
- Large strings (several sentences, lines, or paragraphs)
- Large JSX passed as a param (consider extracting a component if it grows)
- Data that mirrors production shapes and benefits from a single source of truth
- Complex nested structures that would clutter the story file

---

## Stateful Components

For components that manage internal state (controlled inputs, toggles), use a `render` function with local `useState` rather than exposing internal state as args:

```typescript
function ControlledSearch(args: SearchInputProps) {
  const [value, setValue] = useState(args.value ?? '')
  return <SearchInput {...args} value={value} onChange={setValue} />
}

export const Default: Story = {
  args: { placeholder: 'Search...' },
  render: (args) => <ControlledSearch {...args} />,
}
```

Similarly, for form components using `react-hook-form`, wrap the story in a local component that sets up the form — do not expose `UseFormReturn` in args.

---

## `tags: ['autodocs']`

Use `tags: ['autodocs']` on any reusable components. Do not use it on pages, sections, or single-use components. The only exception is when the user explicitly requests adding a docstring to a specific story to document certain information — do not infer that this might be needed.

---

## Next.js Navigation

For components that call `useSearchParams`, `usePathname`, `useRouter`, or depends on URL in Server Components set the navigation context:

```typescript
parameters: {
  nextjs: {
    appDirectory: true,
    navigation: {
      pathname: '/networks',
      query: { search: '', status: 'active' },
    },
  },
}
```

For full documentation of Nextjs Navigation: https://storybook.js.org/docs/get-started/frameworks/nextjs-vite/?renderer=react#nextjs-routing

---

## Width Constraints

For full-width components that look wrong in `centered` layout, use an inline decorator to constrain the width:

```typescript
decorators: [
  (Story) => (
    <div style={{ maxWidth: '1312px', width: '100%' }}>
      <Story />
    </div>
  ),
],
```

---

## Full Example: Section Component with API

```typescript
import type { Meta, StoryObj } from '@storybook/nextjs-vite'
import { delay, http, HttpResponse } from 'msw'
import { withReactQueryProvider } from '@/modules/shared/lib/decorators'
import { mockedItems } from '@/modules/example/mocks'
import MySection from './my-section'

const meta = {
  title: 'Modules/Example/Components/MySection',
  component: MySection,
  parameters: {
    layout: 'fullscreen',
    nextjs: { appDirectory: true },
  },
  decorators: [withReactQueryProvider],
} satisfies Meta<typeof MySection>

export default meta
type Story = StoryObj<typeof meta>

export const Default: Story = {
  parameters: {
    msw: { handlers: [http.get('/api/items', () => HttpResponse.json(mockedItems))] },
  },
}

export const Loading: Story = {
  parameters: {
    msw: { handlers: [http.get('/api/items', async () => { await delay('infinite') })] },
  },
}

export const Empty: Story = {
  parameters: {
    msw: { handlers: [http.get('/api/items', () => HttpResponse.json([]))] },
  },
}

export const Error: Story = {
  parameters: {
    msw: { handlers: [http.get('/api/items', () => HttpResponse.error())] },
  },
}
```
