# Tech Stack

**Version 0.1.0 (draft)**

Frameworks, UI libraries, data layer, and tooling used in the Achra project. Use this doc when choosing libraries, onboarding, or answering “what does Achra use for X?” For current versions, check `package.json`, `next.config.ts`, and `codegen.ts`.

## Framework and runtime

| Technology | Notes |
|------------|--------|
| **Next.js** | 16.x. App Router, typed routes (`typedRoutes: true`), React Compiler (`reactCompiler: true`), cache components (`cacheComponents: true`), Turbopack (with file-system cache in dev). Server actions for mutations and form submissions; placement in `modules/<module>/actions/`. See `next.config.ts` and [architecture.md](architecture.md). |
| **React** | 19.x. Used with React Compiler (Babel plugin). |

## Language and types

| Technology | Notes |
|------------|--------|
| **TypeScript** | 5.x. Strict mode. Path aliases: `@/*` (repo root), `@/shared/*` → `modules/shared/*`. See `tsconfig.json`. |

## UI and styling

| Technology | Notes |
|------------|--------|
| **Tailwind CSS** | 4.x. PostCSS via `@tailwindcss/postcss`. Global styles in `app/globals.css`. |
| **shadcn/ui** | New-york style, RSC, components in `@/shared/components`. Config: `components.json` (aliases: `@/shared/components`, `@/shared/lib/utils`, etc.; icon library: Lucide). |
| **Radix UI** | Primitives (accordion, alert-dialog, dialog, dropdown, select, tabs, tooltip, etc.) used by shadcn components. |
| **Lucide React** | Icon library. |
| **Motion** | Animation library. |
| **class-variance-authority (CVA)** | Component variants. |
| **tailwind-merge**, **clsx** | Class name utilities. |

## Data and API

| Technology | Notes |
|------------|--------|
| **GraphQL** | Schema from `NEXT_PUBLIC_SWITCHBOARD_URL`. Operations in `modules/**/*.graphql` and co-located `.tsx`/`.ts`. |
| **GraphQL Codegen** | `@graphql-codegen/cli` with `typescript`, `typescript-operations`, `typescript-react-query`. Output: `modules/__generated__/graphql/switchboard-generated.ts`. Custom fetcher: `@/shared/lib/fetcher#switchboardFetcher` (Next.js fetch/cache). See `codegen.ts`. |
| **TanStack Query (React Query)** | 5.x. Generated hooks from codegen; Suspense queries, query keys, and fetcher exposed. |

## Forms and validation

| Technology | Notes |
|------------|--------|
| **react-hook-form** | Form state and validation. |
| **@hookform/resolvers** | Schema-based validation resolvers. |
| **Zod** | Schema validation (used with resolvers). |

## URL and client state

| Technology | Notes |
|------------|--------|
| **nuqs** | Type-safe search params in the URL (parsing, serialization). |

## Tooling

| Technology | Notes |
|------------|--------|
| **ESLint** | 9.x. Config: Next, Prettier, React, React Hooks, Storybook, etc. See `eslint.config.mjs`. |
| **Prettier** | With `prettier-plugin-tailwindcss`. |
| **Storybook** | 10.x. Next.js framework; addons: a11y, docs, themes, Vitest. Chromatic for visual regression. |
| **Vitest** | Unit/browser tests. |
| **Playwright** | E2E tests. |
| **MSW** | Mock Service Worker for API mocks; worker directory `public`. Used in Storybook and tests. |
| **SVGR** | SVG as React components (Turbopack rule in `next.config.ts`). |

## Other

| Technology | Notes |
|------------|--------|
| **date-fns** | Date utilities (including `@date-fns/utc` for timezone management). |
| **ethers** | 6.x. Web3 / Ethereum (e.g. wallet, blockies). |
| **Vercel** | Analytics, Speed Insights; deployment target. |
