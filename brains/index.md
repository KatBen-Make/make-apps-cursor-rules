# Brain routing

Three project memories. One shared rulebook (this repo). One shared Make platform-docs vault.

Read only the brain for the workspace that is open.

| Open workspace | Read |
|---|---|
| `MAKE/APPS` (IEN / official Make apps) | [make-ien/index.md](make-ien/index.md) |
| `MAKE/Tools/vscode-apps-sdk` (the VS Code extension) | `AGENTS.md` in that repo, then [vscode-apps-sdk/index.md](vscode-apps-sdk/index.md) |
| `eMonkey/APPS` (client apps) | `agent-brain/index.md` inside that workspace. Not stored in this repo. |

Shared behavior (every Make-related session) still comes from `rules/` and `skills/` in this repo. Project brains hold decisions and approach. They do not repeat those rules.

Make platform documentation (IML, modules, parameters, pagination) is the SDK APPS brain vault. Paths are in [machines.md](machines.md).

When a correction should survive the next session, add one file under that project's `decisions/` folder. A personal habit that applies everywhere goes in `rules/` instead.
