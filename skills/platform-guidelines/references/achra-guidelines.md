# Achra Guidelines

**Version 0.1.0 (draft)**

This document is the human-facing index for Achra platform guidelines. It covers architecture, conventions, business domains, feature flags, data, and granular rules.

---

## Table of Contents

1. [Overview and business context](#1-overview-and-business-context)
2. [Architecture](#2-architecture)
3. [Conventions](#3-conventions)
4. [Feature flags and environment](#4-feature-flags-and-environment)
5. [Data and GraphQL](#5-data-and-graphql)
6. [Tech stack](#6-tech-stack)
7. [Skeleton loading](#7-skeleton-loading)
8. [Rules (granular)](#8-rules-granular)

---

## 1. Overview and business context

What Achra is and which domains exist; where each domain lives in the codebase.

- **[achra-overview.md](achra-overview.md)** — Product summary, domain list with module locations, and feature-flag gating.

---

## 2. Architecture

Where code lives: modules, shared vs domain, placement decision tree, imports.

- **[architecture.md](architecture.md)** — Module layout, shared and domain structure, component placement, import patterns, correct vs incorrect examples.

---

## 3. Conventions

Naming, file layout, and export patterns for consistent code.

- **[conventions.md](conventions.md)** — kebab-case, component directories and index files, named function and named export for components.

---

## 4. Feature flags and environment

When and where to use feature flags; how they are configured per environment.

- **[feature-flags-and-env.md](feature-flags-and-env.md)** — Location of flags, when to add/use them, how to consume, adding new flags, dev vs staging vs production.

---

## 5. Data and GraphQL

Where GraphQL operations and generated code live; how services use them.

- **[data-and-graphql.md](data-and-graphql.md)** — GraphQL file location, generated code in `__generated__`, using fetchers and types in services.

---

## 6. Tech stack

Frameworks, UI libraries, data layer, and tooling used in the project.

- **[tech-stack.md](tech-stack.md)** — Next.js, React, TypeScript, Tailwind, shadcn/ui, GraphQL and TanStack Query, forms, tooling (ESLint, Storybook, Vitest, Playwright, MSW), and related libraries.

---

## 7. Skeleton loading

How to build skeleton loaders that mirror layout and avoid shift; used for loading.tsx, Suspense fallbacks, and placeholders.

- **[skeleton-loading.md](skeleton-loading.md)** — Layout mirroring, sizing and line-height rules, contrast, interactivity cleanup, composition, imports, Suspense and route loading, accessibility, validation checklist.
- **[skeleton-loading-examples.md](skeleton-loading-examples.md)** — Hypothetical patterns: header, card, list row, multi-line text, Suspense and loading.tsx.

---

## 8. Rules (granular)

Short, grep-friendly rules for agents; each file covers one pattern.

- **[_sections.md](rules/_sections.md)** — Section list and filename prefixes (arch-, conv-, ff-, data-).
- **rules/** — Individual rule files (e.g. arch-module-placement.md, conv-kebab-case.md, ff-when-to-use.md). Use grep to find a topic: `grep -l "placement" references/rules/`.
