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

**Local development** ([test plugins locally](https://cursor.com/docs/plugins#test-plugins-locally)):

1. Clone this repo (or use your existing checkout path).
2. Symlink the **repository root** (the folder that contains `.cursor-plugin/`) into Cursor’s local plugins directory:

   ```bash
   mkdir -p ~/.cursor/plugins/local
   ln -sfn /path/to/platform-guidelines-skills ~/.cursor/plugins/local/platform-guidelines
   ```

   Use `ln -sfn` so re-running the command updates an existing symlink.

3. Reload Cursor (**Command Palette** → **Developer: Reload Window**, or restart the app).
4. Confirm under **Settings → Plugins** that the plugin is listed.

If the plugin does not appear after reload, some Cursor versions do not follow symlinks under `~/.cursor/plugins/local/`; copy the repo instead: `cp -R /path/to/platform-guidelines-skills ~/.cursor/plugins/local/platform-guidelines` ([related issue](https://github.com/cursor/plugins/issues/35)).

### Generic

```bash
npx skills add https://github.com/YasielCabrera/platform-guidelines-skills --skill platform-guidelines
```

## License

MIT
