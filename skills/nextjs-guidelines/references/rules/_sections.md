# Sections

This file defines all sections, their ordering, and descriptions. The section ID (in parentheses) is the filename prefix used to group rules.

---

## 1. Architecture (arch)

**Description:** Where code lives: module placement, shared vs domain, imports. Ensures consistent structure and discoverability.

## 2. Conventions (conv)

**Description:** Naming (kebab-case), component directory layout, exports. Keeps files and exports consistent across the codebase.

## 3. Feature flags (ff)

**Description:** When and where to use feature flags; env-specific behavior. Ensures gating is done in one place and is predictable.

## 4. Data and GraphQL (data)

**Description:** Location of GraphQL operations and generated code; how services use them. Keeps data access patterns consistent.
