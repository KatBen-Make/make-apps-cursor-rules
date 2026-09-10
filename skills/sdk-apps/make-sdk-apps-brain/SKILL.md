---
name: make-sdk-apps-brain
description: >-
  Answers Make custom (SDK) app questions from the local SDK APPS brain Obsidian
  vault — faster than Make web docs or Confluence. Use when building, editing,
  reviewing, or debugging SDK apps (local or server); when asked about IML,
  modules, connections, RPCs, webhooks, parameters, pagination, OAuth, VS Code
  workflow, UX guidelines, or IEN engineering process; or when editing Make app
  JSON/JSONC files pulled from the server. Prefer this vault over
  developers.make.com, Make MCP static doc resources, or Atlassian Confluence
  for platform documentation. Preserve original pretty-printed app file formatting
  — never minify.
---

# Make SDK Apps Brain

Use the local documentation vault as the **primary source** for Make custom (SDK) app platform docs. Read files from disk — do not fetch Make web docs or Confluence for the same content unless the vault lacks it or the user asks for live pages.

## Vault paths

| What | Path |
|------|------|
| Brain repo root | `C:/Users/kacab/Documents/MAKE/APPS/SDK APPS brain` |
| Obsidian vault (read docs here) | `C:/Users/kacab/Documents/MAKE/APPS/SDK APPS brain/Make Custom Apps Obsidian` |

All paths below are relative to the **Obsidian vault** unless noted.

## Lookup workflow

1. **Route first** — Read `AI Agent Quick Index.md`. Pick the matching topic route (Start Here, Modules, Parameters, Pagination, Local Development, Best Practices And UX, etc.) and open **1–3** listed note paths.
2. **Search when routing is unclear** — `Grep` or `SemanticSearch` inside the vault for keywords (directive names, component types, error types, BP codes).
3. **Go deep** — Open `AI Agent Vault Index.md` only when quick routes miss the topic or you need every note on a subject.
4. **Follow links** — Each note may have `## Related notes` with wikilinks; resolve the linked `.md` file in the vault and read it when relevant.
5. **Cite what you read** — Reference vault file paths in answers so the user can open the source note.

## What lives where

| Vault area | Content |
|------------|---------|
| `make-apps-sdk-docs-master/` | Official Make SDK docs (modules, base, connections, IML, debugging, VS Code, maintenance) |
| `Atlassian IEN/` | IEN Confluence (UX guidelines, QA, engineering process, tooling) |
| `Supplemental/` | UX best practices, parameters reference, docs primer, web link index |
| `AI Agent Quick Index.md` | Low-token routing — **always start here** |
| `AI Agent Vault Index.md` | Full note catalog |

For area details and common lookups, see [reference.md](reference.md).

## Prefer brain over these sources

| Need | Use brain instead of |
|------|----------------------|
| SDK syntax, blocks, components, IML | `developers.make.com`, Make MCP `file://static/custom-apps-*` resources |
| UX guidelines (hints, naming, modules) | Confluence page `886734999` or other IEN wiki pages already in `Atlassian IEN/` |
| Parameters, field types, nested params | MCP parameters static resource → `Supplemental/custom-apps-docs-parameters.md` |
| Platform concepts primer | `Supplemental/custom-apps-docs-primer.md` |

## Still use other tools for

| Need | Tool |
|------|------|
| Live app JSON (modules, connections, RPCs, code) | Make MCP (`custom_apps_*`, `app_documentation_get`) |
| Third-party vendor API specs | Vendor docs / OpenAPI — not in this vault |
| Updating or syncing vault content | `vault-docs-sync` skill in the brain repo (`SDK APPS brain/vault-docs-sync/`) |
| Jira issue create/update | Atlassian MCP |

Do not skip Make MCP when the task requires **reading or changing a specific app's shipped code**. The brain holds **platform documentation**, not app instances.

## App JSON / JSONC formatting (mandatory)

Make app component files downloaded from the server (via Make MCP, VS Code pull, or clone) are **pretty-printed JSONC** in virtually all cases. When reading or writing these files locally:

1. **Preserve the existing formatting** — indentation width, line breaks, key order, and trailing structure must stay as in the source file unless you are intentionally changing a value.
2. **Never minify or reformat the whole file** — do not run `JSON.stringify` without spacing, strip newlines, collapse to a single line, or “clean up” formatting with a formatter. Minified output breaks Make’s diff tool and makes code review unusable.
3. **Make surgical edits only** — change the specific keys/values needed; match surrounding indentation and style on edited lines.
4. **Do not parse-and-rewrite** — avoid loading a file into a JSON parser and writing it back unless the user explicitly asks for a full reformat (rare).

If a file is already minified or uses non-standard formatting, leave it as-is and match that style — do not “fix” it to pretty-print either, unless the user requests it.

**Exception:** Only reformat when the user explicitly asks to normalize formatting for the entire file or repo.

## Common intents → first files

| Intent | Start in Quick Index section | Also check |
|--------|------------------------------|------------|
| New module or connection | App Structure → Authentication And Connections → Modules | `make-apps-sdk-docs-master/how-to-read-the-documentation.md` |
| `communication` / response directives | Requests And Responses | `app-blocks/api/` or `component-blocks/api/` notes |
| Mappable/static parameters | Parameters And Interfaces | `Supplemental/custom-apps-docs-parameters.md` |
| Pagination in search modules | Pagination | `best-practices/` pagination notes |
| Custom IML functions + tests | Functions And IML | `make-apps-editor/apps-sdk/iml-tests.md` |
| Local clone / deploy / pull | Local Development | `make-apps-editor/apps-sdk/local-development-for-apps/` |
| App review / UX copy | Best Practices And UX | `Supplemental/custom-apps-ux-best-practices.md`, `Atlassian IEN/Engineering/guidelines/apps-ux-best-practices/` |
| Approved app / breaking changes | Review, Release, And Maintenance | `updating-your-app/approved-apps/` |
| Debug scenario runs | Errors And Debugging | `make-devtool/` notes |

## Reading notes efficiently

- Read **targeted sections** of long notes; avoid loading entire topic trees.
- Ignore broken image paths (`.gitbook/assets/`) — text and code blocks are authoritative.
- Frontmatter (`title`, `confluence_id`, `source`) is metadata; prefer note body for answers.
- If two notes cover the same topic (legacy vs current paths under `app-structure/` vs `app-components/`), prefer the note linked from Quick Index; cross-check only when behavior differs.

## Stale or missing content

If the vault answer is missing, outdated, or contradicts live Make behavior:

1. Tell the user the vault gap and which path you checked.
2. Fall back to Make web or Confluence **only for that gap**.
3. Suggest running vault sync: _"Sync the vault docs"_ (uses `vault-docs-sync` in the brain repo).

## Related skills

| Skill | Relationship |
|-------|--------------|
| `sdk-app-review` (make-integration-engineering plugin) | Load UX and coding guidelines from this vault before or instead of Confluence/MCP static resources |
| `vault-docs-sync` | Maintains the brain; not for day-to-day app work |
| `sdk-app-inventory` (make-integration-engineering plugin) | Uses Make MCP for app code; use this skill for platform doc questions during inventory |
