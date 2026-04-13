# Commit Changes

Safely commit uncommitted changes with proper branch management and semantic commit messages. Always propose, confirm, then execute — iterate until the user accepts.

## Workflow

### 1. Inspect state

Run in parallel:

- `git status` — list changed/untracked files
- `git diff` (staged + unstaged) — understand the actual changes
- `git branch --show-current` — detect current branch
- `git log -n 5 --oneline` — follow repo commit style

### 2. Branch check

- If on a **protected branch** (`main`, `dev`, `staging`): a new feature branch must be created before committing (propose one in step 4).
- If on a **feature branch**: keep it; only a commit message will be proposed.

### 3. Detect repo quality tooling

Do **not** assume `pnpm format:check` / `pnpm lint` / `pnpm build` exist. Detect what this repo actually uses by inspecting it:

- Read `package.json` `scripts` (root and, in monorepos, the affected workspace) and pick what's defined. Common names: `format`, `format:check`, `fmt`, `lint`, `lint:fix`, `typecheck`, `check`, `build`.
- If no `package.json`, look for the right ecosystem: `Cargo.toml` (`cargo fmt --check`, `cargo clippy`, `cargo build`), `pyproject.toml` (`ruff`, `black --check`, `mypy`), `go.mod` (`gofmt -l`, `go vet`, `go build`), `Makefile` targets, etc.
- Honor the package manager the repo uses (`pnpm-lock.yaml` → `pnpm`, `yarn.lock` → `yarn`, `bun.lockb` → `bun`, else `npm`).

Run the detected **format check**, **lint**, and **build** commands (skip any that don't exist — don't invent them). If a formatter auto-fix exists and the check fails, run the fix and re-check once. Report each step's result briefly.

If **lint** or **build** fails:

- **Do not** attempt to fix the failure automatically.
- Stop the commit flow and report to the user:
  - which step failed (lint vs build) and the command that was run,
  - the relevant error output (trimmed to the meaningful lines),
  - a short list of proposed next steps with your recommended follow-up marked, e.g.:
    - **Recommended:** investigate and fix the failing [file:line] before committing.
    - Alternative: commit anyway on a WIP branch (explicitly acknowledge the broken state in the message).
    - Alternative: stash changes and retry later.
- Wait for the user's decision before doing anything else. Do not proceed to step 4 until the user explicitly tells you how to continue.

### 4. Build the proposal

Generate:

- A **branch name** (only if currently on a protected branch), using a conventional prefix:

  | Prefix | Use for |
  | --- | --- |
  | `feat/` | New features, components, functionality |
  | `fix/` | Bug fixes |
  | `chore/` | Maintenance, deps, config |
  | `docs/` | Documentation |
  | `refactor/` | Restructuring without behavior change |
  | `test/` | Tests or test infra |

- A **single-line conventional commit message**: `type(scope): description`
  - `type`: `feat` | `fix` | `docs` | `refactor` | `test` | `chore`
  - `scope`: optional area (e.g., `roadmap`)
  - `description`: short, imperative, lowercase, no trailing period
  - Exactly one line — no body, no line breaks

### 5. Present the proposal

Show the user a single summary table, then ask for approval.

```
| Field          | Value                                                      |
| -------------- | ---------------------------------------------------------- |
| Current branch | main                                                       |
| New branch     | feat/add-key-results-component                             |
| Commit message | feat(roadmap): add key results component to milestone card |
```

When on a feature branch already, render `New branch` as `— (staying on current)`.

Then use the **AskUserQuestion** tool with a single yes/no question:

- `question`: `Do we proceed?`
- `options`: `Yes` and `No`

### 6. Iterate or execute

- **Yes** → execute:
  1. Create and switch to the new branch if one was proposed.
  2. Stage the intended files **by name** (never `git add -A` / `git add .`).
  3. Create the commit with the approved single-line message.
  4. Run `git status` and report success.
- **No** → ask the user what to change (branch name, commit message, or both), regenerate the proposal, re-render the table, and re-ask via AskUserQuestion/AskQuestion. Keep iterating until they answer Yes.

## Guardrails

- Never commit directly to `main`, `dev`, or `staging`.
- Never use `--no-verify` or skip hooks. If a hook fails, fix the root cause and create a **new** commit (do not amend).
- Do not stage files that look sensitive (`.env`, credentials, keys); call them out before proceeding.
- Only execute after an explicit **Yes** from the AskUserQuestion/AskQuestion prompt.
- Keep commit messages short, single-line, and semantic.
