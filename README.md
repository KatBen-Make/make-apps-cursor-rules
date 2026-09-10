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

## Current contents

### rules/sdk-apps
- `backup-make-app-files.mdc` — back up Make app files before editing (no git history)
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

## How to use in Cursor

Copy the relevant files into your local Cursor config, preserving the relative
path under the domain folder, e.g.:

- `rules/sdk-apps/*.mdc` → `~/.cursor/rules/`
- `skills/sdk-apps/<name>/SKILL.md` → `~/.cursor/skills/<name>/SKILL.md`
- `commands/sdk-apps/*.md` → `~/.cursor/commands/`

(User Rules set via Cursor Settings, rather than rule files, aren't distributed
as files by Cursor — the `.mdc` copies of those here are the portable source of
truth to re-apply them elsewhere.)
