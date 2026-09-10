# SDK APPS Brain — Vault Reference

Paths are relative to `Make Custom Apps Obsidian/`.

## Top-level layout

```
Make Custom Apps Obsidian/
├── AI Agent Quick Index.md      # Agent entrypoint (read first)
├── AI Agent Vault Index.md      # Full catalog
├── make-apps-sdk-docs-master/   # Make SDK platform docs
├── Atlassian IEN/               # IEN Confluence mirror
│   ├── Engineering/
│   ├── How we work/
│   └── Tools/
└── Supplemental/                # Curated quick references
    ├── custom-apps-docs-primer.md
    ├── custom-apps-docs-parameters.md
    ├── custom-apps-ux-best-practices.md
    └── custom-apps-documentation-web-links.md
```

## make-apps-sdk-docs-master — major sections

| Folder | Topics |
|--------|--------|
| `get-started/`, `create-your-first-app/` | Tutorials, first app |
| `app-structure/`, `app-components/` | Base, modules, connections, RPCs (parallel doc trees) |
| `app-blocks/`, `component-blocks/` | Block-level API specs (communication, response directives) |
| `best-practices/` | Recommended patterns |
| `make-apps-editor/apps-sdk/` | VS Code extension, local dev, deploy, IML tests |
| `make-devtool/`, `debug-your-app/`, `debugging-your-app/` | Scenario debugger, live stream |
| `updating-your-app/`, `app-maintenance/` | Approved apps, breaking changes |
| `other/` | Edge cases (empty values, JSON strings, custom CA) |

## Atlassian IEN — common paths

| Path | Use for |
|------|---------|
| `Engineering/guidelines/apps-ux-best-practices/` | Module naming, hints, descriptions, custom fields, BP-style UX rules |
| `Engineering/quality-assurance/` | App testing, E2E, regression |
| `Engineering/development/` | Native apps, app-specific engineering notes |
| `How we work/` | Jira guidance, team responsibilities |
| `Tools/jira/` | IEN Jira project usage |

## Supplemental — when to open directly

| File | Use when |
|------|----------|
| `custom-apps-docs-primer.md` | Explaining what custom apps are, JSON architecture, hierarchy |
| `custom-apps-docs-parameters.md` | Parameter types, validation, nested/select/array patterns |
| `custom-apps-ux-best-practices.md` | Consolidated UX checklist (alternative to browsing IEN tree) |
| `custom-apps-documentation-web-links.md` | Map vault topics to official web URLs (fallback links only) |

## Search tips

```text
Grep pattern examples (vault root):
  "iterate"           → response directive docs
  "RateLimitError"    → error handling
  "nested"            → parameter nesting
  "BP-0"              → UX best practice IDs in IEN notes
  "oauth2"            → connection types
```

Use `SemanticSearch` with natural language when you know the concept but not the filename:

- "How do search module pagination limits work?"
- "OAuth additional scopes in connections"
- "Deploy local app changes to Make"

## Wikilinks in notes

Notes use Obsidian `[[wikilink]]` syntax. Resolve to `.md` files:

- `[[Base URL]]` → search vault for `base-url.md` or grep the link target
- `## Related notes` sections list curated follow-ups — prefer these over random grep hits

## Brain repo maintenance (not for doc lookup)

| Path | Purpose |
|------|---------|
| `SDK APPS brain/vault-docs-sync/SKILL.md` | Sync docs repo + Confluence into vault |
| `SDK APPS brain/vault-docs-sync/scripts/sync_vault_docs.py` | Local normalize/index rebuild |
| `SDK APPS brain/README.md` | Human overview of the brain repo |
