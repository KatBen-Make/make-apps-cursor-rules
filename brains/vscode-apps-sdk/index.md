# vscode-apps-sdk brain

The Make Apps Editor VS Code extension (`integromat/vscode-apps-sdk`). TypeScript and JavaScript, VS Code API, Axios. This is not a Make custom-app module tree.

Use this brain only while that repo is the workspace. IEN app rules and eMonkey client rules do not apply to extension code.

## Read for the task

| Topic | Where |
|---|---|
| Architecture, online vs local mode, API v2, gotchas | `AGENTS.md` in the extension repo. `CLAUDE.md` there includes it. |
| Lessons from PR review | `rules/vscode-apps-sdk/dev-conventions.mdc` in this rules repo (also installed as a project rule in the extension workspace) |
| PowerShell / `gh` / `npm` on Windows | `.cursor/rules/windows-tooling.mdc` in the extension repo |
| Backup before edit | `.cursor/rules/backup-before-edit.mdc` in the extension repo |

## How this repo is worked

- New API work uses Make API v2. Leave v1 (legacy Integromat) alone.
- `AGENTS.md` is tracked in the extension git repo. Personal decisions that should not land in an upstream PR go in [decisions/](decisions/README.md) here, not in `AGENTS.md`.

Machine paths: [../machines.md](../machines.md).
