---
name: sdk-app-change-review
description: >-
  Pre-commit code review for a Make SDK (custom) app committer. Reads a Jira
  task, fetches the app via the make MCP server, and reviews ONLY the components
  and changes described in the task — touched modules, RPCs, functions and their
  unit tests, plus sentence/title case consistency of touched mappable
  parameters. Use when reviewing a developer's changes before committing an SDK
  app fix, when acting as committer/reviewer on a Make custom app Jira ticket, or
  when the user asks to review the changes for a specific IEN task.
---

# Make SDK App — Changes Review (Committer)

Reviews a developer's changes on a Make custom app **before committing**. Scope is
strictly limited to what the Jira task says was changed — this is not a full app audit.

## Prerequisites

- **atlassian** MCP server — read the Jira task (`getJiraIssue`). Requires the Atlassian plugin.
- **make** MCP server — fetch app components and code (bundled with the make plugin).

If either server is missing, stop and ask the user to install the required plugin.

Required inputs (ask if missing):

| Input | Description |
|-------|-------------|
| **Jira key** | The IEN task being committed (e.g. `IEN-1234`) |
| **App name + version** | Make app identifier and version. Read from the Jira task first (see below); only ask the user if it cannot be resolved. |

**Resolving app name + version from the Jira task** (do this before asking the user):

1. **Explicit slug in the body** — many tasks state it directly, e.g. `**App slug:** airtable`
   or `App: airtable v3` in the description or a subtask. Prefer this when present.
2. **Task title prefix** — IEN tasks are usually titled `AppName: <subject>` or `AppName > <subject>`
   (e.g. `Personio: ...` → `personio`, `Airtable > ...` → `airtable`). Lowercase and hyphenate
   the prefix to get the likely app identifier.
3. **App HQ URL** — if the task links to the admin page, e.g.
   `https://eu1.make.com/admin/apps/personio/1`, the path is `/admin/apps/{appName}/{appVersion}`.
   Scan the description, Developer Notes, and remote/web links for any `/admin/apps/<name>/<version>` URL.
4. **Version** — read from `App: <slug> v<N>`, the App HQ URL, or a version string in the
   Developer Notes / SDK API-checkup comment (e.g. `personio (v1.12.15)` → major version `1`).
   When still unknown, list the app with `custom_apps_fetch` to find the current version.

Confirm the resolved name with `custom_apps_fetch`. If sources conflict or none resolve, ask the user.

## Workflow

```
- [ ] 1. Read the Jira task and extract the change scope
- [ ] 2. Fetch the app and list components
- [ ] 3. Review ONLY the touched components against the described change
- [ ] 4. Review touched functions and their unit tests
- [ ] 5. Check mappable-parameter case consistency (only if touched)
- [ ] 6. Output the review as bullet points
```

### Step 1 — Read the Jira task

`getJiraIssue` for the key with
`fields: ["summary", "description", "customfield_10483", "comment", "status", "subtasks", "parent"]`.

Read for change scope, in priority order:

1. **Developer Notes** (`customfield_10483`) — often the primary source. Look for a `Changes`
   heading with bullets and a `Breaking / non-breaking impact` section; these name the
   touched modules, functions, RPCs and the specific change to each. Component names here are
   **internal names** (e.g. module `listAttendances`, function `mapAttendancePeriodV2`).
2. **Description** — for the overall intent / acceptance criteria, and sometimes a `## Changes`
   section with the actual change/code.
3. **Comments** — read the latest automated `IML FUNCTIONS TEST RESULTS` comment (if present)
   to see the current pass/fail state of each function's unit tests, and any reviewer comments
   asking for fixes. Use the most recent run, not an earlier one.

**Subtasks.** If the issue has `subtasks` (or its Developer Notes/description say something like
"In each subtask"), the per-component changes usually live in the subtasks, not the parent.
Fetch each subtask with `getJiraIssue` (`fields: ["summary", "description", "customfield_10483", "status"]`)
and read its `## Changes` section — each subtask typically states `App: <slug> v<N>`, the
component (e.g. `Function: functions/processRecord`), and the exact change. **Aggregate** the
change scope across all subtasks into one list, noting each subtask key and status. Skip
subtasks the user tells you to exclude.

While reading, resolve the **app name + version** from the task (or its subtasks) per the Inputs section.

Build a **change scope** list: each entry is a component (type + internal name), the specific
change made to it, and the source Jira key (parent or subtask). If the scope is ambiguous
(no component named anywhere), ask the user to confirm which components were touched before
reviewing — do not guess and review the whole app.

### Step 2 — Fetch the app

1. `custom_apps_fetch` with `{ appName, appVersion }` to confirm it exists and resolve version.
2. List the component types that appear in the change scope so you can resolve **labels**:
   - Modules: `custom_apps_modules_fetch` → `{ appName, appVersion }`
   - RPCs: `custom_apps_rpcs_fetch` → `{ appName, appVersion }`
   - Functions: `custom_apps_functions_fetch` → `{ appName, appVersion }`
   - Connections: `custom_apps_connections_fetch` → `{ appName }`
   - Webhooks: `custom_apps_webhooks_fetch` → `{ appName }`

Module and RPC **labels** can differ greatly from their internal **name**. Always capture
both. Every module/RPC in the output must be referred to as `Label (name)`.

### Step 3 — Review touched components only

For each component in the change scope, fetch the relevant detail sections and review the
change. Do **not** review components the task did not touch.

| Component | Fetch | Sections |
|-----------|-------|----------|
| Module | `custom_apps_modules_fetch` (with `moduleName`) | `communication`, `mappable_parameters`, `interface` |
| RPC | `custom_apps_rpcs_fetch` (with `rpcName`) | `api`, `parameters` |
| Connection | `custom_apps_connections_fetch` | `api`, `parameters` |
| Webhook | `custom_apps_webhooks_fetch` | `api`, `parameters` |

Review the change for correctness against what the task asked: does it actually fix/implement
the described behavior, is the IML/API valid, are referenced fields real, is user-facing text
US-English and spelled correctly. Keep findings limited to the changed area.

### Step 4 — Functions and unit tests

For every function named in the change scope (or referenced by a touched module/RPC change):

1. `custom_apps_functions_get_code` → review the implementation against the described change.
2. `custom_apps_functions_get_test` → review the unit tests against the **updated** code.
3. Cross-check against the latest `IML FUNCTIONS TEST RESULTS` comment from Step 1: every
   touched function should appear there and pass. Flag any function with failing or missing
   runs (e.g. a `❌` line, or a function absent from the latest results).

Treat stale, missing, or weak tests as findings. Tests must cover happy path, edge cases,
invalid input, null/undefined, and boundary conditions for the changed logic. Assertions
must use Node `assert` with `assert.strictEqual` (primitives) / `assert.deepStrictEqual`
(objects/arrays) — never `assert.ok` or loose checks. Flag any assertion that would pass an
incorrect implementation.

### Step 5 — Mappable parameter case consistency

Only run this check for modules whose **mappable parameters were touched** by the developer
(per the change scope). Skip untouched modules entirely.

- A module may use **Title Case** (older modules) or **Sentence case** (newer modules) for
  parameter labels — both are acceptable. The rule is **internal consistency**: every
  mappable-parameter label within a single module must use the same case style.
- Determine the module's prevailing style from its existing labels, then flag any label
  (especially newly added/edited ones) that breaks that style.
- Nested parameters (collections, arrays of fields) count — check their child labels too.
- Report the conflict per module, naming the offending label(s) and the expected style.

## Output format

Always respond in **bullet points** — clear and concise. State the **component type** for every
finding. Refer to modules and RPCs by `Label (name)`, never the name alone. Group by component.

```markdown
## Review: {appName} v{version} — {JIRA-KEY}

**Change scope:** {one line listing the touched components; note subtask keys when split}

### Module: {Label} ({name}) {— IEN-xxxx if from a subtask}
- {finding — what's right or wrong about the change, concise}

### RPC: {Label} ({name})
- {finding}

### Function: {name} {— IEN-xxxx if from a subtask}
- Code: {finding}
- Tests: {finding}

### Mappable parameter casing
- {Module Label (name)}: prevailing style is {Sentence case|Title Case}; `{label}` breaks it (expected {style}).

### Verdict
- {Ready to commit | Changes requested} — {one-line reason}
```

Keep each bullet to one idea. If a touched component is clean, say so in one bullet rather
than omitting it, so the committer knows it was reviewed.
