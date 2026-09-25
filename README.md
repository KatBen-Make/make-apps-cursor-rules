# Personal Cursor Rules / Skills / Commands

This repository holds **KatBen's personal** Cursor AI configuration: rules,
skills, and commands used day to day in Cursor, organized by domain.

> **Note:** This repo previously held a set of Make SDK Apps rules/skills/commands
> (unit test generation, app review, app component export). That content has
> been retired — it's now covered by the **make-integration-engineering**
> Cursor plugin (marketplace), which provides more complete, actively
> maintained equivalents:
> - `sdk-app-review` skill — app review against coding practices and UX guidelines
> - `sdk-app-inventory` skill — component inventory / export / API-vs-vendor-docs checkup
>
> If you're looking for that content, install the `make-integration-engineering`
> plugin from the Make Cursor Marketplace instead of copying files from here.

## Structure

Type-first (mirrors Cursor's own `~/.cursor/{rules,skills,commands}` layout),
with one subfolder per domain inside each type:

```
rules/
  sdk-apps/            # Make custom (SDK) apps — general, cross-app
  vscode-apps-sdk/      # Developing the integromat/vscode-apps-sdk VS Code extension
skills/
  sdk-apps/
commands/
  sdk-apps/
```

### Domains

- **sdk-apps** — working on Make custom (SDK) apps via the Make MCP server:
  investigation discipline, backup-before-edit, unit test conventions, model
  routing for API-doc investigations, Jira Developer Notes formatting,
  pre-commit change review, and the Limit-parameter checker command.
  (The local SDK docs knowledge-base skill lives in a colleague's separate
  repo, not here.)
- **vscode-apps-sdk** — contributing to the `integromat/vscode-apps-sdk` VS
  Code extension codebase itself (TypeScript) — distinct from building apps
  *with* that extension.

Client- or app-specific skills (e.g. anything scoped to a single customer's
app family) are **not** kept here — they live alongside that project instead.

## Brains

Project memory (approach and decisions) is separate from rules and skills.
Start at [`brains/index.md`](brains/index.md). Paths per computer are in
[`brains/machines.md`](brains/machines.md).

| Brain | Canonical file | Installed on this PC |
|---|---|---|
| IEN / `MAKE/APPS` | `brains/make-ien/index.md` | `.cursor/rules/project-brain.mdc` and `CLAUDE.md` in that workspace |
| vscode-apps-sdk | `brains/vscode-apps-sdk/index.md` plus the extension repo's `AGENTS.md` | `.cursor/rules/project-brain.mdc` in the extension repo (local only; `AGENTS.md` stays the tracked architecture doc) |
| eMonkey | **Not in this repo.** `eMonkey/APPS/agent-brain/index.md` | `.cursor/rules/project-brain.mdc` and `CLAUDE.md` in that workspace |

Entry-rule copies to drop into each workspace live in `brains/cursor-entry/`.
Claude Code's global router is `brains/user-CLAUDE.md` (copy to `~/.claude/CLAUDE.md`).

After a correction that should survive the next session, add one file under
that project's `decisions/` folder. A habit for every workspace goes in `rules/`.

## Where each file actually lives in Cursor

Files in this repo are **portable copies**. Where the original actually lives
in Cursor differs per item — this matters because User Rules aren't files you
can drop into a folder:

| Repo file | Cursor location | How to re-apply |
|---|---|---|
| `rules/sdk-apps/backup-make-app-files.mdc` | **User Rule** (Cursor Settings → Rules) | Paste body into Settings → Rules → add rule |
| `rules/sdk-apps/agent-investigation-and-make.mdc` | **User Rule** — titled *"Always apply this rule"* in Settings | Paste body into Settings → Rules → add rule |
| `rules/sdk-apps/apps-unit-testing-convention.mdc` | **User Rule** | Paste body into Settings → Rules → add rule |
| `rules/sdk-apps/ai-model-split-for-app-doc-investigation.mdc` | **Workspace/Project Rule** (`.cursor/rules/` in the MAKE/APPS workspace) | Drop the `.mdc` file as-is into `<workspace>/.cursor/rules/` |
| `rules/vscode-apps-sdk/dev-conventions.mdc` | **Workspace/Project Rule** (`.cursor/rules/` in the vscode-apps-sdk workspace) | Drop the `.mdc` file as-is into `<workspace>/.cursor/rules/` |
| `brains/cursor-entry/make-apps-project-brain.mdc` | **Project rule** in `MAKE/APPS` | Copy to `<MAKE/APPS>/.cursor/rules/project-brain.mdc` |
| `brains/cursor-entry/vscode-apps-sdk-project-brain.mdc` | **Project rule** in the extension repo | Copy to `<vscode-apps-sdk>/.cursor/rules/project-brain.mdc` |
| `brains/cursor-entry/emonkey-project-brain.mdc` | **Project rule** in `eMonkey/APPS` | Copy to `<eMonkey/APPS>/.cursor/rules/project-brain.mdc` |
| `brains/user-CLAUDE.md` | Claude Code global instructions | Copy to `~/.claude/CLAUDE.md` |
| `skills/sdk-apps/*` | User-level skill (`~/.cursor/skills/<name>/`) | Copy folder as-is |
| `commands/sdk-apps/*` | User-level command (`~/.cursor/commands/`) | Copy file as-is |

User Rules have no backing file on disk (confirmed — they live in Cursor's
internal settings, not `~/.cursor/user-rules/` or any plain file), so the
`.mdc` copies here are the **only portable source of truth** for them. This is
also why they're most important to keep in sync (see below) and to re-paste
manually when setting up Cursor on a new machine (e.g. the planned Mac move).

## Current contents

### rules/sdk-apps
- `backup-make-app-files.mdc` — back up Make app files before editing (no git history). Cross-platform: includes both Windows (PowerShell) and macOS (bash/zsh) paths and snippets.
- `agent-investigation-and-make.mdc` — verify-before-claiming, Make MCP only, ask before changing apps
- `apps-unit-testing-convention.mdc` — deterministic `it()`/`assert` unit test standard
- `ai-model-split-for-app-doc-investigation.mdc` — orchestrator/subagent model split for app-vs-vendor-doc investigations

### rules/vscode-apps-sdk
- `dev-conventions.mdc` — lessons distilled from PR review history on the extension repo

### skills/sdk-apps
- `jira-developer-notes/` — structured Analysis/Changes/QA Developer Notes format for Jira
- `sdk-app-change-review/` — scoped pre-commit review of a Jira task's described changes

### commands/sdk-apps
- `check-parameter-limit.md` — audits Search/List/Watch modules for a correct `limit` parameter

### brains
- `index.md` — which workspace reads which brain
- `machines.md` — paths on this PC and a blank column for the other computer
- `make-ien/` — IEN project memory and `decisions/`
- `vscode-apps-sdk/` — extension project memory and `decisions/`
- `emonkey/README.md` — pointer only; client notes stay in the eMonkey workspace
- `cursor-entry/` — project rules already installed on this PC
- `user-CLAUDE.md` — Claude Code router, already copied to `~/.claude/CLAUDE.md` on this PC

## How to use in Cursor

Copy the relevant files into your local Cursor config, preserving the relative
path under the domain folder — see the [location table](#where-each-file-actually-lives-in-cursor)
above for exactly where each one goes (User Rule vs Project Rule vs skill vs command).

## Keeping this in sync

These are manual copies, not symlinks — editing the live version (a User Rule
in Cursor Settings, or a skill/command file under `~/.cursor/`) does **not**
update this repo automatically, and vice versa. Whenever you change one side:

1. Update the live version (Cursor Settings, or the local `~/.cursor/...` file) as usual.
2. Copy the updated content into the matching file here.
3. Commit and push.

If it's been a while, diff the live content against the repo copy before
trusting either one — don't assume they still match.

## Adding new content

Follow the existing `<type>/<domain>/<file>` pattern:

- New domain → add a subfolder under each of `rules/`, `skills/`, `commands/`
  that applies (not all three are required — e.g. a domain might only need a skill).
- **Rules** (`rules/<domain>/<name>.mdc`) — frontmatter should include
  `description` and either `alwaysApply: true` (Project Rule / most User Rules)
  or `globs: [...]` for pattern-triggered Project Rules.
- **Skills** (`skills/<domain>/<name>/SKILL.md`) — frontmatter needs `name` and
  a `description` that states when to use it (skills are matched by description,
  not always active).
- **Commands** (`commands/<domain>/<name>.md`) — frontmatter needs `name`,
  `type: command`, and `description`.
- Update the "Current contents" section and the location table above when you add something.
