# Platform Guidelines Plugin

AI agent plugin with platform guidelines, architecture, conventions, and engineering patterns. Compatible with **Claude Code** and **Cursor**.

## Available Skills

### platform-guidelines

Platform guidelines for architecture, conventions, business domains, and technical patterns. Covers module placement, naming, routing, GraphQL, feature flags, skeleton loading, Storybook stories, and tech stack.

**Use when:**
- Writing or refactoring code (components, pages, services, hooks)
- Creating skeleton loaders, loading placeholders, or Suspense fallbacks
- Writing or reviewing Storybook stories
- Adding modules, features, or routes
- Answering questions about architecture, conventions, or tech stack

**Categories covered:**
- Architecture (shared vs domain modules, placement, server actions)
- Conventions (kebab-case, component structure, types, constants)
- Data & GraphQL (domain graphql, generated code)
- Feature flags (when and where to use)
- Skeleton loading (layout mirroring, sizing, contrast)
- Storybook stories (when to write, variants, decorators, MSW)
- Tech stack (Next 16, React 19, Tailwind 4, shadcn, GraphQL)

## Commands

- **skeleton** — Create a skeleton loading component following platform patterns

## Installation

### Claude Code

Install from the CLI or use the plugin manager:

```bash
claude plugin install platform-guidelines
```

Or load locally during development:

```bash
claude --plugin-dir /path/to/platform-guidelines-skills
```

Once installed, use skills with the `platform-guidelines:` namespace:

```
/platform-guidelines:platform-guidelines
/platform-guidelines:skeleton
```

### Cursor

Install from the Cursor Marketplace, or add locally:

1. Clone this repo
2. In Cursor, use the plugin manager to add the local directory

Once installed, reference the skill:

```
@platform-guidelines
```

### Agent Skills (generic)

```bash
npx skills add https://github.com/YasielCabrera/platform-guidelines-skills --skill platform-guidelines
```

## Plugin Structure

```
platform-guidelines-skills/
├── .claude-plugin/           # Claude Code plugin manifest
│   └── plugin.json
├── .cursor-plugin/           # Cursor plugin manifest
│   └── plugin.json
├── skills/
│   └── platform-guidelines/     # Platform guidelines skill
│       ├── SKILL.md          # Skill entry point
│       └── references/       # Detailed reference docs
│           ├── achra-overview.md
│           ├── architecture.md
│           ├── conventions.md
│           ├── data-and-graphql.md
│           ├── feature-flags-and-env.md
│           ├── skeleton-loading.md
│           ├── skeleton-loading-examples.md
│           ├── storybook-stories.md
│           ├── tech-stack.md
│           └── rules/        # Granular rule files
├── commands/
│   └── skeleton.md           # Skeleton component task spec
├── LICENSE
└── README.md
```

## License

MIT
