# Feature Flags and Environment

**Version 0.1.0 (draft)**

Feature flags gate domains and sections (workstreams, finances, roadmaps, etc.) per environment. They live in the shared module and are consumed in config, nav, and conditional UI.

## Location

**Directory**: `modules/shared/lib/feature-flags/`

- **types.ts** — `FeatureFlags` interface; single source of truth for all flags.
- **ff.dev.ts** — Development flags (typically more features on).
- **ff.staging.ts** — Staging flags.
- **ff.production.ts** — Production flags (typically fewer experimental features).
- **index.ts** — Exports default `ff` based on `ENVIRONMENT` (development / staging / production); also exports `allFeatureFlags` for tooling or tests.

## When to use feature flags

Use flags to:

- **Gate whole domains or sections** — e.g. workstreams, roadmaps, finances sections, builders section links.
- **Toggle nav items or routes** — e.g. show/hide Workstreams or Governance in the navbar.
- **Control environment-specific behavior** — e.g. whitelist overlay, login button, etc.

Do **not** use them for:

- Generic React or UI behavior that could be handled by component logic or config.
- One-off A/B experiments unless that pattern is established; prefer the existing domain/section gating pattern.

## How to consume

Import the default export from the feature-flags module and read the appropriate flag:

```typescript
import ff from '@/shared/lib/feature-flags'

if (ff.workstreams.WORKSTREAMS_ENABLED) {
  // show workstreams nav / content
}

if (ff.finances.SUMMARY_SECTION_ENABLED) {
  // show summary section
}
```

Use flags in:

- **Nav/config** — e.g. `modules/shared/config/navbar-config.ts` to build links conditionally.
- **Layouts and pages** — to conditionally render sections or redirect.
- **Domain components** — when a section is optional (e.g. wallets, breakdown chart).

## Adding a new flag

1. **Extend the type** — Add the new property to `FeatureFlags` in `types.ts` with a short JSDoc comment.
2. **Set per environment** — Add the same property in `ff.dev.ts`, `ff.staging.ts`, and `ff.production.ts` with the desired value for each.
3. **Use in code** — Import `ff` and branch on the new flag where the feature is gated.

Keep flag names clear and grouped (e.g. under `workstreams.*`, `finances.*`, `builders.*`).

## Environment behavior (summary)

- **Development** — Most optional domains/sections enabled (workstreams, roadmaps, finances sections, builders links, etc.).
- **Staging** — Same as dev or tuned for QA; see `ff.staging.ts`.
- **Production** — Fewer experimental areas on by default (e.g. workstreams, project details, RFP, some finances sections and builders links can be off).

Always read the current `ff.*.ts` files for the exact behavior per environment.
