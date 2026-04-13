---
name: create-pr
description: Open a pull request on GitHub using the GitHub MCP, following the repo's PR template.
---

# Create Pull Request

Open a pull request on GitHub using the GitHub MCP, following the repo's PR template. Always propose, confirm, then execute — iterate until the user accepts.

## Workflow

### 1. Verify GitHub MCP is available

Before anything else, confirm a GitHub MCP tool for creating pull requests is available (e.g. `mcp_github_create_pull_request` or current equivalent).

- If **not available**: report this to the user and **abort**. Do **not** fall back to `gh`, the REST API, or any other tool. Wait for the user to enable the MCP.

### 2. Inspect state

Run in parallel:

- `git status` — detect uncommitted changes
- `git branch --show-current` — current branch
- `git log -n 10 --oneline` — recent commits on this branch
- `git remote -v` — resolve `owner/repo`

### 3. Handle uncommitted changes

If `git status` shows uncommitted changes, use **AskUserQuestion** with:

- `question`: `You have uncommitted changes. Commit them before opening the PR?`
- `options`: `Yes, run /commit` and `No, abort`

- **Yes** → invoke the `/commit` command as-is and only proceed to step 4 **after** the commit succeeds.
- **No** → abort the PR flow.

### 4. Branch check

- If on a **protected branch** (`main`, `dev`, `staging`): abort and tell the user to switch to a feature branch.
- Otherwise: continue on the current branch.

### 5. Verify build and lint

Detect tooling the same way `/commit` does — don't assume commands exist:

- Read `package.json` `scripts` (root + affected workspace in monorepos). Common names: `lint`, `typecheck`, `build`.
- If no `package.json`, use the right ecosystem (`cargo`, `pyproject.toml`, `go.mod`, `Makefile`, …).
- Honor the repo's package manager (`pnpm-lock.yaml` → `pnpm`, `yarn.lock` → `yarn`, `bun.lockb` → `bun`, else `npm`).

Run the detected **lint** and **build** commands. If either fails:

- **Do not** attempt to fix it automatically.
- Stop, report which step failed with trimmed error output, and wait for the user's direction. Do not proceed until lint and build pass.

### 6. Gather PR info

- **Ticket**: do **not** ask for one. If the user supplied a Trello/Notion/Jira/Linear/GitHub-issue link, include it inline in **Summary**.
- Analyze commits and diff vs. the base branch (`dev` by default, or user-specified) to understand the change at a product/feature level.
- Generate a PR **title** based on the branch name and changes (conventional style, e.g. `feat(scope): …`).

### 7. Fill the PR template

- Read `.github/PULL_REQUEST_TEMPLATE.md`. If it doesn't exist, use the default sections below.
- Fill each section:

**Summary** — what this PR delivers or fixes, in plain language. No file paths, no file lists. If a ticket URL was supplied, add `Related: …`.

**What changed** — reviewer-oriented detail: behavior, UI, APIs, data flow, meaningful refactors. Expand when non-obvious; stay brief when obvious from the title. **No** file-by-file diffs.

**How to test** — actionable steps for reviewers: where to go in the app, what to click/enter, expected outcome. Prefer user-visible paths over "run tests".

**Snapshots** — placeholder or brief note if no visuals; fill in user-supplied screenshots/Chromatic links.

### 8. Ensure branch is pushed

- Check whether the current branch exists on the remote.
- If not, push it with `git push -u origin <branch>`.

### 9. Present the proposal

Show a single summary table:

```
| Field  | Value                                                 |
| ------ | ----------------------------------------------------- |
| Repo   | owner/repo                                            |
| Base   | dev                                                   |
| Head   | feat/networks-grid-layout                             |
| Title  | feat(networks): improve network grid layout           |
```

Then **always** render the full PR description inside a fenced ` ```markdown ` code block so the user sees the raw markdown verbatim — never as prose. Example:

````
```markdown
## Summary
…

## What changed
…

## How to test
…

## Snapshots
…
```
````

Then use **AskUserQuestion**/**AskQuestion**:

- `question`: `Do we proceed?`
- `options`: `Yes` and `No`

### 10. Iterate or execute

- **Yes** → call the GitHub MCP `create_pull_request` with `owner`, `repo`, `title`, `head`, `base`, `body`. Report the PR URL.
- **No** → ask what to change (title, base, body section), regenerate, re-render the table, and re-ask via AskUserQuestion. Keep iterating until Yes.

## Guardrails

- **Never** create a PR from `main`, `dev`, or `staging`.
- **Never** fall back to another tool if the GitHub MCP is unavailable — abort and wait for the user.
- **Never** assign reviewers, assignees, labels, or request reviews unless the user explicitly asks.
- **Never** paste file lists, diffs, or line-by-line notes into the body.
- Only execute after an explicit **Yes** from AskUserQuestion.
- Do **not** use the AskQuestion tool in this command — AskUserQuestion only.
