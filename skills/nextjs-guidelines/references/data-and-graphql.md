# Data and GraphQL

**Version 0.1.0 (draft)**

GraphQL operations and domain data fetching follow a consistent layout: operations live in domain modules, generated types and hooks live in a single generated module, and services or hooks use them. This doc describes where things live and how to use them.

## GraphQL operations

**Where**: `modules/<domain>/graphql/*.graphql`

- Each domain that uses GraphQL keeps its `.graphql` files in a `graphql/` folder inside that module.

**Examples**:

- `modules/builders/graphql/builders-list.graphql`
- `modules/workstream/graphql/workstream-details.graphql`
- `modules/networks/graphql/all-networks.graphql`
- `modules/expense-reports/graphql/expense-reports-actuals.graphql`

## Generated code

**Where**: `modules/__generated__/graphql/`

- A single generated file (e.g. `switchboard-generated.ts`) is produced from all GraphQL operations (via a codegen step).
- It exports types, query/mutation shapes, and fetchers or hooks (e.g. `useBuildersListQuery`, `useBuildersListQuery.fetcher`).

**Do not edit** the generated file by hand; regenerate it when `.graphql` files or codegen config change.

## Using GraphQL in code

- **Import from generated**: Use types and fetchers/hooks from `@/modules/__generated__/graphql/...` (e.g. `switchboard-generated`).
- **Domain services**: Domain-specific data access lives in `modules/<domain>/services/` or `modules/<domain>/lib/`. Those modules call the generated fetchers and expose domain-shaped data to components.
- **Shared fetcher**: The actual HTTP call is wired through a shared fetcher (e.g. `modules/shared/lib/fetcher.ts`, `switchboardFetcher`). The generated code uses this; you typically only use the generated API.

**Example** (service using generated fetcher):

```typescript
import {
  type BuildersListQueryVariables,
  useBuildersListQuery,
} from '@/modules/__generated__/graphql/switchboard-generated'

export async function getBuilders(filter: BuildersListQueryVariables['filter']) {
  const data = await useBuildersListQuery.fetcher({ filter })()
  return data.builders
}
```

## Services and lib

- **Domain services**: `modules/<domain>/services/` — e.g. `builders.ts`, `networks-service.ts`. Use for server or shared data logic that uses GraphQL or other APIs.
- **Domain lib**: `modules/<domain>/lib/` — utilities and helpers specific to that domain.
- **Shared fetcher**: `modules/shared/lib/fetcher.ts` — shared fetch/GraphQL transport used by generated code.

When adding a new GraphQL-backed feature:

1. Add or update `.graphql` files in the appropriate `modules/<domain>/graphql/`.
2. Run the codegen so `modules/__generated__/graphql/` is updated. (pnpm run codegen)
3. Use the generated types and fetchers from a domain service or hook, and keep components consuming the domain API rather than raw GraphQL.
