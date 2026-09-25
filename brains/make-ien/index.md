# IEN workspace brain

Official Make custom apps and IEN tickets. Workspace folder: `MAKE/APPS`.

Use this brain only in that workspace. eMonkey client conventions and the VS Code extension repo stay in their own brains.

## Read for the task

Paths are inside `make-apps-cursor-rules` unless noted.

| Topic | Where |
|---|---|
| Verify before claiming, Make MCP only, ask before creating modules / RPCs / functions | `rules/sdk-apps/agent-investigation-and-make.mdc` |
| Jira field limits | Workspace rule `.cursor/rules/working-with-jira-tasks.mdc` in `MAKE/APPS` |
| Developer Notes shape | `skills/sdk-apps/jira-developer-notes/SKILL.md` |
| App code vs vendor API docs (model split) | `rules/sdk-apps/ai-model-split-for-app-doc-investigation.mdc` |
| Unit tests | `rules/sdk-apps/apps-unit-testing-convention.mdc` |
| Backup before editing app files | `rules/sdk-apps/backup-make-app-files.mdc` |
| Pre-commit review of a ticket's changes | `skills/sdk-apps/sdk-app-change-review/SKILL.md` |
| Platform docs (IML, modules, parameters, pagination, UX) | SDK APPS brain vault. Start at `AI Agent Quick Index.md` inside `Make Custom Apps Obsidian` |

## How this workspace is worked

- Live app JSON comes from Make MCP. The docs vault is not a copy of any app.
- Ask before creating or deleting functions, RPCs, webhooks, or modules.
- Jira updates stay on Developer Notes unless explicitly told otherwise.
- After a correction that should stick, add one file in [decisions/](decisions/README.md).

Machine paths: [../machines.md](../machines.md).
