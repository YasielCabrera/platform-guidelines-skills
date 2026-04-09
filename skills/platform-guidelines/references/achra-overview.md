# Achra Overview

**Version 0.1.0 (draft)**

## What is Achra

Achra is a platform for networks, builders, workstreams, finances, and related operations. It provides global routes (networks list, workstreams, services) and network-scoped areas (builders, finances, roadmaps, workstream details, RFP, etc.). Many areas are gated by feature flags so they can be enabled or disabled per environment (e.g. dev vs production). See [feature-flags-and-env.md](feature-flags-and-env.md) for details.

## Domains

Domains align with the `modules/` directory and with feature flags where applicable.

| Domain | Module(s) | Description |
|--------|-----------|-------------|
| **Networks** | `modules/networks/` | Network listing, network-specific navigation, forum/governance integration. Routes: `/networks`, `/network/[slug]/...`. |
| **Builders** | `modules/builders/`, `modules/builder-profile/` | Builder list and builder profiles; budget statements. Routes: `/network/[slug]/builders`, `/network/[slug]/builders/[builderSlug]/...`. Feature flags: builders section links (finances, connect, projects). |
| **Workstreams** | `modules/workstream/` | Workstream list and details, projects, initial proposal, RFP. Routes: `/workstreams`, `/network/[slug]/workstreams`, `/network/[slug]/workstream/[workstreamSlug]/...`. Gated by `workstreams.WORKSTREAMS_ENABLED` and related flags. |
| **Finances** | `modules/finances/` | Network finances: summary, navigation, breakdown chart, wallets, budget statements. Routes: `/network/[slug]/finances`. Section visibility gated by finances feature flags. |
| **Expense reports** | `modules/expense-reports/` | Expense reporting (used in conjunction with finances and services). |
| **Roadmap** | `modules/roadmap/` | Roadmap listing and detail pages. Routes: `/network/[slug]/roadmaps`, `/network/[slug]/roadmap/[roadmapSlug]`. Gated by `ROADMAPS_ENABLED`. |
| **RFP** | `modules/rfp/` | RFP (request for proposal) flow under a workstream. Route: `/network/[slug]/workstream/[workstreamSlug]/rfp`. Gated by workstreams flags. |
| **Project** | `modules/project/` | Project details and deliverable-related UI. Used in workstream and builder contexts. |
| **Services** | `modules/services/` | Services catalog and service profiles; operators. Routes: `/services`, `/services/[serviceSlug]`, `/services/operators/[operatorSlug]`. |
| **Dashboard** | `modules/dashboard/` | Dashboard sections (e.g. builders section, finances section). |
| **Operator profile** | `modules/operator-profile/` | Operator profile UI and metrics. |
| **Whitelist** | `modules/whitelist/` | Whitelist overlay and logic. Gated by `WHITELIST_OVERLAY_ENABLED`. |
| **Shared** | `modules/shared/` | Cross-cutting UI (sidebar, header, design system), hooks, lib (feature flags, fetcher, utils), types, providers. Not a business domain—use for code used by multiple domains or app root. |

## Feature flags and visibility

Several domains or sections are toggled per environment (dev, staging, production). Always check the shared feature-flags module when adding or gating UI: [feature-flags-and-env.md](feature-flags-and-env.md).

## Where to put new code

- **New domain or feature area**: Add or use a domain module under `modules/<domain>/` and follow [architecture.md](architecture.md).
- **New shared UI or util**: Use `modules/shared/` as in [architecture.md](architecture.md).
- **New route**: Global under `(achra)` or network-scoped under `network/[slug]/...`.
