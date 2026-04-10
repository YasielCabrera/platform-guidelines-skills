# Platform Guidelines Plugin

AI agent plugin with platform guidelines, architecture, conventions, and engineering patterns. Compatible with **Claude Code** and **Cursor**.

## What's Included

| Type | Name | Description |
|------|------|-------------|
| Skill | `platform-guidelines` | Architecture, conventions, data & GraphQL, feature flags, Storybook, and tech stack rules |
| Skill | `skeleton-loading` | Skeleton loading patterns and component guidelines |
| Agent | `skeleton-builder` | Builds skeleton loading components following platform patterns |
| Agent | `architecture-compliance` | Audits code against platform guidelines and fixes violations |
| Command | `skeleton` | Create a skeleton loading component |

## Installation

### Claude Code

```bash
claude plugin install platform-guidelines
```

Or load locally:

```bash
claude --plugin-dir /path/to/platform-guidelines-skills
```

### Cursor

1. Clone this repo
2. Add the local directory via the plugin manager

### Generic

```bash
npx skills add https://github.com/YasielCabrera/platform-guidelines-skills --skill platform-guidelines
```

## License

MIT
