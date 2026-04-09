---
title: GraphQL in domain graphql/, generated in __generated__
category: data
tags: graphql, generated, services
---

## GraphQL in domain graphql/, generated in __generated__

Put GraphQL operations in `modules/<domain>/graphql/*.graphql`. Generated types and fetchers live in `modules/__generated__/graphql/`. Use them from domain services or hooks; do not edit the generated file.

**Incorrect:** Putting `.graphql` files in a random folder or calling an API directly without going through the generated fetcher.

**Correct:**

- Add or edit `modules/builders/graphql/builders-list.graphql`
- Run codegen → `modules/__generated__/graphql/` is updated
- In `modules/builders/services/builders.ts`: import from `@/modules/__generated__/graphql/...` and use the fetcher

Reference: [data-and-graphql.md](../data-and-graphql.md)
