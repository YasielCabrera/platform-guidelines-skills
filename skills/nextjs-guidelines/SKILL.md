---
name: nextjs-guidelines
description: Next.js application guidelines, architecture, conventions, and engineering patterns. Use when writing or refactoring code, adding modules or components, answering questions about architecture, deciding when to use feature flags, applying naming and placement conventions, or answering which technologies and libraries the project uses.
---

# Next.js Guidelines

## Overview

Engineering guidelines for Next.js applications: architecture, conventions, and technical patterns. This skill covers where code lives, how to name and structure it, when to use feature flags, and how routes and modules map.

**WIP:** These guidelines are a draft. When a task is not covered here or in linked references, prefer consistency with existing code and patterns.

## When to Apply

Reference these guidelines when:
- Writing new code (components, pages, services, hooks, providers, server actions, etc.)
- Refactoring or reviewing code for consistency with project conventions
- Adding a new module, feature, or route
- Answering questions about architecture or conventions
- Deciding where to place a component or whether to add a feature flag
- Answering questions about the tech stack or choosing libraries/tools

## Quick Reference

| Topic | Rule | Details |
|-------|------|---------|
| **Module placement** | Shared vs domain, promotion rules | Used in 2+ modules or app root → check [Promotion rules](references/architecture.md#promotion-rules); single domain → `modules/{domain}/`. See [architecture.md](references/architecture.md). |
| **Naming** | kebab-case | Files and directories: `component-name.tsx`, `use-hook-name.ts`. See [conventions.md](references/conventions.md). |
| **Components** | Directory + index | One dir per component; **one component per file** (subcomponents in separate files). Helpers in **lib/utils**, not in component files. Named function, named export. See [conventions.md](references/conventions.md). |
| **Feature flags** | Shared, env-specific | `modules/shared/lib/feature-flags/`. Use for gating domains/sections per environment. See [feature-flags-and-env.md](references/feature-flags-and-env.md). |
| **Data / GraphQL** | Domain graphql, generated | Queries in `modules/<domain>/graphql/*.graphql`; generated in `modules/__generated__/graphql/`. See [data-and-graphql.md](references/data-and-graphql.md). |
| **Types** | Props in file; reusable at module root | [conventions.md](references/conventions.md) |
| **Constants** | One `constants.ts` per module in `lib/`; UPPER_SNAKE_CASE | [conventions.md](references/conventions.md) |
| **Tech stack** | Next 16, React 19, TS, Tailwind 4, shadcn, GraphQL + TanStack Query | Framework, UI, data, forms, and tooling. See [tech-stack.md](references/tech-stack.md). |
| **Skeleton loading** | See dedicated **skeleton-loading** skill | Skeleton creation has its own skill with full rules, workflow, and examples. |
| **Storybook stories** | See dedicated **storybook-stories** skill | Story authoring (variants, decorators, MSW, mocks) has its own skill. |
| **Server actions** | actions/, one per file, suffix action | In `modules/<module>/actions/`; file and function names end with `action`. See [architecture.md](references/architecture.md). |

## Skeleton loading

Skeleton loading has been extracted into a dedicated **skeleton-loading** skill with comprehensive rules, workflow, sizing guidelines, and examples. Use that skill when creating skeleton loaders, loading placeholders, Suspense fallbacks, or Next.js `loading.tsx` files.

## Storybook stories

Storybook story authoring has been extracted into a dedicated **storybook-stories** skill covering when to create stories, variant policy, file placement, title convention, decorators, MSW mocking, mock data, and full examples. Use that skill when writing or reviewing stories.

## References

Full documentation:

- [architecture.md](references/architecture.md) — Module layout, shared vs domain, promotion rules, placement decision tree, imports
- [conventions.md](references/conventions.md) — Naming, component directories, exports
- [feature-flags-and-env.md](references/feature-flags-and-env.md) — When and where to use feature flags; env behavior
- [data-and-graphql.md](references/data-and-graphql.md) — GraphQL and services location; generated code
- [tech-stack.md](references/tech-stack.md) — Framework, UI, data, forms, and tooling used in the project
- [rules/](references/rules/) — Granular rules (arch-, conv-, ff-, data-)
