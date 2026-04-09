---
name: platform-guidelines
description: Platform guidelines, business rules, architecture, and engineering patterns. Use when writing or refactoring code, adding modules or components, writing or reviewing Storybook stories, answering questions about architecture or business domains, deciding when to use feature flags, applying naming and placement conventions, or answering which technologies and libraries the project uses.
---

# Platform Guidelines

## Overview

Platform guidelines for architecture, conventions, business domains, and technical patterns. This skill covers where code lives, how to name and structure it, when to use feature flags, and how domains and routes map.

**WIP:** These guidelines are a draft. When a task is not covered here or in linked references, prefer consistency with existing code and patterns.

## When to Apply

Reference these guidelines when:
- Writing new code in the Achra codebase (components, pages, services, hooks, providers, etc)
- Writing or reviewing Storybook stories
- Refactoring or reviewing code for Achra consistency
- Adding a new module, feature, or route
- Answering questions about Achra architecture, business domains, or conventions
- Deciding where to place a component or whether to add a feature flag
- Answering questions about Achra's tech stack or choosing libraries/tools

## Quick Reference

| Topic | Rule | Details |
|-------|------|---------|
| **Module placement** | Shared vs domain, promotion rules | Used in 2+ modules or app root → check [Promotion rules](references/architecture.md#promotion-rules); single domain → `modules/{domain}/`. See [architecture.md](references/architecture.md). |
| **Naming** | kebab-case | Files and directories: `component-name.tsx`, `use-hook-name.ts`. See [conventions.md](references/conventions.md). |
| **Components** | Directory + index | One dir per component; **one component per file** (subcomponents in separate files). Helpers in **lib/utils**, not in component files. Named function, named export. See [conventions.md](references/conventions.md). |
| **Feature flags** | Shared, env-specific | `modules/shared/lib/feature-flags/`. Use for gating domains/sections (workstreams, finances, roadmaps). See [feature-flags-and-env.md](references/feature-flags-and-env.md). |
| **Data / GraphQL** | Domain graphql, generated | Queries in `modules/<domain>/graphql/*.graphql`; generated in `modules/__generated__/graphql/`. See [data-and-graphql.md](references/data-and-graphql.md). |
| **Types** | Props in file; reusable at module root | [conventions.md](references/conventions.md) |
| **Constants** | One `constants.ts` per module in `lib/`; UPPER_SNAKE_CASE | [conventions.md](references/conventions.md) |
| **Tech stack** | Next 16, React 19, TS, Tailwind 4, shadcn, GraphQL + TanStack Query | Framework, UI, data, forms, and tooling. See [tech-stack.md](references/tech-stack.md). |
| **Storybook stories** | Sections/pages + reusable components; min variants | Create for section/page and shared components; skip one-time small components; cover Default, Loading, Empty, Error. See [storybook-stories.md](references/storybook-stories.md). |
| **Skeleton loading** | See dedicated **skeleton-loading** skill | Skeleton creation has its own skill with full rules, workflow, and examples. |
| **Server actions** | actions/, one per file, suffix action | In `modules/<module>/actions/`; file and function names end with `action`. See [architecture.md](references/architecture.md). |

## Storybook stories

Stories serve two purposes: **documentation** for reusable/shared components and **visual regression testing** via Chromatic on every PR.

**When to create a story:** For section-level and page-level components, and for any reusable component (especially in `modules/shared/`). Skip one-time small components — write the story on the parent section component instead.

**Variants:** Only create variants that produce visually distinct UI states. Default, Loading, Empty, and Error cover most components. Avoid prop-permutation variants that look identical in Chromatic.

**Key patterns:**
- Colocate `[component-name].stories.tsx` next to the component file
- Import from `@storybook/nextjs-vite`
- Use `layout: 'fullscreen'` for sections, `'centered'` for isolated components
- Use shared decorators from `modules/shared/lib/decorators.tsx`: `withNuqsAdapter`, `withReactQueryProvider`, `withPortalFontStyles`
- Mock API calls with MSW (`parameters.msw.handlers`); use `delay('infinite')` for the Loading variant
- Place mock data in `modules/{module}/mocks/`, not inline in story files
- Use `tags: ['autodocs']` only on shared UI components

Full rules, patterns, and examples: [storybook-stories.md](references/storybook-stories.md).

## Skeleton loading

Skeleton loading has been extracted into a dedicated **skeleton-loading** skill with comprehensive rules, workflow, sizing guidelines, and examples. Use that skill when creating skeleton loaders, loading placeholders, Suspense fallbacks, or Next.js `loading.tsx` files.

## References

Full documentation:

- [achra-overview.md](references/achra-overview.md) — Business and product context; main domains and where they live
- [architecture.md](references/architecture.md) — Module layout, shared vs domain, promotion rules, placement decision tree, imports
- [conventions.md](references/conventions.md) — Naming, component directories, exports
- [feature-flags-and-env.md](references/feature-flags-and-env.md) — When and where to use feature flags; env behavior
- [data-and-graphql.md](references/data-and-graphql.md) — GraphQL and services location; generated code
- [tech-stack.md](references/tech-stack.md) — Framework, UI, data, forms, and tooling used in the project
- [storybook-stories.md](references/storybook-stories.md) — When to create stories, variant policy, file placement, title convention, decorators, MSW, mock data
- [achra-guidelines.md](references/achra-guidelines.md) — Human-facing index with table of contents and links
- [rules/](references/rules/) — Granular rules (arch-, conv-, ff-, data-)
