---
name: app-reviewer
description: Review a Make custom app against coding practices and UX guidelines. Checks spelling, casing, labels, hints, error messages, ID finders, unit tests, and all components. Use when the user asks to review, audit, lint, or check a Make custom app for quality, compliance, or best practices.
---

# Custom App Reviewer

Review a Make custom app through the `user-Make All` MCP server and the `user-Atlassian` MCP server, checking all components against coding practices and UX guidelines.

## Required Input

- `appName` — the app identifier (e.g., `my-app`)
- `caseType` — `Title Case` or `Sentence case` for label consistency checks

If either is missing, ask the developer before proceeding.

## Review Workflow

### Step 0: Load guidelines

Fetch these resources in parallel:

| Source | How |
|--------|-----|
| Make UX best practices | `FetchMcpResource` server `user-Make All`, uri `file://static/custom-apps-ux-best-practices` |
| Make parameters docs | `FetchMcpResource` server `user-Make All`, uri `file://static/custom-apps-docs-parameters` |
| Atlassian UX guidelines | `CallMcpTool` server `user-Atlassian`, tool `getConfluencePage`, args `{ "cloudId": "make.atlassian.net", "pageId": "886734999", "contentFormat": "markdown" }` |
| App API documentation | `CallMcpTool` server `user-Make All`, tool `app_documentation_get`, args `{ "appName": appName }` |

Store these as reference for all subsequent checks.

### Step 1: Fetch app base

```
CallMcpTool  server: "user-Make All"  tool: "custom_apps_fetch"
  { appName, appVersion: 1, sections: ["base", "groups"] }
```

Extract `baseUrl`, error handling config, and module groups ordering.

### Step 2: Fetch all component lists (parallel)

| Tool | Arguments |
|------|-----------|
| `custom_apps_modules_fetch` | `{ appName, appVersion: 1 }` |
| `custom_apps_connections_fetch` | `{ appName }` |
| `custom_apps_rpcs_fetch` | `{ appName, appVersion: 1 }` |
| `custom_apps_functions_fetch` | `{ appName, appVersion: 1 }` |
| `custom_apps_webhooks_fetch` | `{ appName }` |

### Step 3: Fetch detail sections for each component

**Modules** — for each module:
```
custom_apps_modules_fetch
  { appName, appVersion: 1, moduleName, sections: ["communication", "mappable_parameters", "static_parameters", "interface", "samples"] }
```

**Connections** — for each connection:
```
custom_apps_connections_fetch
  { connectionName, sections: ["api", "parameters"] }
```

**RPCs** — for each RPC:
```
custom_apps_rpcs_fetch
  { appName, appVersion: 1, rpcName, sections: ["api", "parameters"] }
```

**Webhooks** — for each webhook:
```
custom_apps_webhooks_fetch
  { webhookName, sections: ["api", "parameters", "attach", "detach"] }
```

**Functions** — for each function, fetch both code and test:
```
custom_apps_functions_get_code  { appName, appVersion: 1, functionName }
custom_apps_functions_get_test  { appName, appVersion: 1, functionName }
```

### Step 4: Run all checks

For every component, run the checks described below. Collect all findings.

---

## Check Categories

### Check 1: US English Spellcheck

Scan all user-facing text — `label`, `help`, `text` (banners), error `message` fields, `description`, array `labels.add`, and RPC parameter labels.

- Flag any non-US English spelling (e.g., "colour" should be "color", "organisation" should be "organization").
- Flag obvious typos and grammar errors.
- Ignore technical terms, API names, brand names, and IML expressions (`{{...}}`).

### Check 2: Case Consistency

Based on the developer's chosen `caseType`:

**Title Case rules:**
- Capitalize first and last word.
- Capitalize all words except short prepositions (in, on, at, to, for, of, by, with), articles (a, an, the), and coordinating conjunctions (and, but, or, nor).
- Acronyms stay ALL CAPS (ID, API, URL, HTML, UUID, VAT, CSV, PDF).
- Proper nouns keep their official casing.

**Sentence case rules:**
- Capitalize only the first word.
- Acronyms stay ALL CAPS.
- Proper nouns keep their official casing.

Check these fields:
- Module labels and descriptions
- Parameter `label` fields in mappable_parameters, static_parameters, connection parameters, RPC parameters, webhook parameters
- Array item labels and `labels.add` button text
- RPC labels
- Banner titles
- Parameter names mentioned in `help` text (should be bold and match their actual label casing)

Flag any field that does not match the chosen case type.

### Check 3: UX Guidelines Compliance

Check against loaded UX best practices (from Step 0). Key checks:

**Module naming (BP-011):**
- Starts with a verb (Watch, Create, Get, Update, Delete, Search, List, etc.)
- Plural for Watch/Search/List, singular with a/an for actions
- Additional info tags lowercase in parentheses: (advanced), (beta), (deprecated)

**Module descriptions (BP-001):**
- States the end result, not the technical process
- Follows templates: Trigger → "Triggers when...", Action → "Creates/Updates/Deletes...", Search → "Returns..."

**Input field labels (BP-012):**
- 1–3 words, descriptive not instructional
- No articles (the, a, an) in labels
- No punctuation in labels
- Match 3rd party UI terminology

**Hints (BP-002):**
- Non-obvious fields have hints
- Hints don't start with verbs
- Hints end with a period
- Use backtick for example values, bold for parameter names
- Use markdown links, not "click here"
- Connection hints link to Help Center or 3rd party docs

**Messages/Banners (BP-015):**
- Use banner type (not HTML parameters)
- Correct theme: info (blue), warning (yellow), danger (red)
- 50–300 characters

**Array labeling (BP-016):**
- Array label is plural
- Array item label is singular
- Add button: "Add [singular item]"

**Date inputs (BP-018):**
- Use "date" type for full date+time fields
- Date-only fields (no time component) must use type `"text"` with a `help` explaining the expected format (e.g., `YYYY-MM-DD`)
- Time-only fields must use type `"text"` with a `help` explaining the expected format (e.g., `HH:mm`)

**ID field naming (BP-013):**
- Map toggle ON → include "ID" in label
- Map toggle OFF → no "ID" in label

**Types of fields (BP-004):**
- Standard fields visible by default, advanced fields behind toggle
- Limit field is last
- Required fields have defaults when advanced
- No "advanced" inside nested parameters

**Limit field (BP-005):**
- Present in List/Search modules
- Uses standard hint text
- Is last field, not in advanced

**Connection metadata (BP-021):**
- Metadata saved if user info endpoint exists
- UID saved for shared webhook scenarios

**Connection types (BP-020):**
- Multiple connection types → type in parentheses

**Error handling (BP-022):**
- 429 mapped to RateLimitError
- 501/502/503 mapped to ConnectionError
- User-friendly error messages

**Page size (BP-009):**
- Set to API maximum

**Custom fields (BP-019):**
- Dynamic parameters, not manual array mapping

**Validate empty responses (BP-003):**
- Get modules validate empty responses

**Groups ordering (BP-010):**
- Triggers first, then business logic groups (RCUD order), then Other

### Check 4: Coding Practices

**Base configuration:**
- baseUrl set correctly
- Default error handling defined
- Log sanitization for sensitive headers (`authorization`, `x-api-key`, etc.)

**Communication sections:**
- Proper HTTP methods
- Pagination implemented for search/list modules (use API max page size per BP-009)
- Response output correctly maps data
- Error handling overrides where needed (429, 501-503)

**Parameters:**
- `name` uses camelCase
- `type` is lowercase and valid
- Required parameters listed first
- No pagination params (page, offset, pageSize) in mappable_parameters

**IML expressions:**
- Valid syntax (balanced `{{` and `}}`)
- Using built-in functions correctly

### Check 5: Function Unit Tests

For every custom function:
- Check if test code exists (via `custom_apps_functions_get_test`)
- If no test exists → flag as **error**
- If test exists → verify it is non-empty and passes the quality checks below

**Unit test quality checks:**
- Tests use only `it()` blocks — no `describe()`, no imports, no comments
- Assertions use Node `assert` library only
- Primitive comparisons use `assert.strictEqual()`
- Object/array comparisons use `assert.deepStrictEqual()`
- Never `assert.ok()`, never non-strict assertions, never loose equality
- Simple cases: inline assertion with exact value (e.g., `assert.strictEqual(fn(null), undefined)`)
- Complex cases: define `const input` and `const output`, then `assert.deepStrictEqual(fn(input), output)` — output must be fully defined and deterministic
- When an `it()` block has multiple assertions, every assertion must include a descriptive message string
- Assertions must be strong enough to fail on incorrect implementations — no vague boolean or truthy/falsey checks

### Check 6: ID Finder Usage

Using the app's API documentation (fetched in Step 0):

- For every module parameter that represents an entity ID (e.g., `projectId`, `contactId`, `boardId`), check if an ID finder RPC is configured.
- An ID finder should exist when the API provides a list/search endpoint for that entity.
- Check the ID finder follows BP-017:
  - Button labeled "ID finder" (or "Finder" for non-ID values)
  - Includes info banner: "If you don't see the result you're looking for, try more specific search criteria."
  - Search field labeled "[Item] keywords" for keyword search
  - For exact match: hint "Must be the exact [item]"
  - Results in alphabetical order when possible

---

## Output Format

Present the report as markdown with this structure:

```
# App Review: {appName}

## Summary
- **Components scanned**: X modules, Y connections, Z RPCs, W functions, V webhooks
- **Issues found**: X errors, Y warnings, Z suggestions
- **Label case**: {caseType}

---

## Modules

### {moduleLabel} (`{moduleName}`) — {typeName}

#### Errors
- **[mappable_parameters]** [SPELL] Label "Organsiation" should be "Organization"
- **[mappable_parameters]** [CASE] Label "Create New Item" should be "Create new item" (Sentence case)
- **[module]** [UX:BP-011] Module name should start with a verb
- **[communication]** [CODE] Missing log sanitization for authorization header
- **[mappable_parameters]** [ID-FINDER] Parameter `projectId` should have an ID finder — API has GET /projects endpoint

#### Warnings
- **[mappable_parameters]** [UX:BP-002] Parameter "status" has no hint
- **[mappable_parameters]** [UX:BP-004] Limit field is not last

#### OK
- **[communication]** Error handling correctly maps 429 to RateLimitError

---

## Connections

### {connectionLabel} (`{connectionName}`)

#### Errors
- **[parameters]** [UX:BP-002] Parameter "API Key" has no hint
- **[api]** [CODE] Missing error handling for 429

---

## RPCs

### {rpcLabel} (`{rpcName}`)

#### Errors
- **[parameters]** ...

---

## Functions

### `{functionName}`

#### Errors
- [TEST] No unit test found

---

## Webhooks

### {webhookLabel} (`{webhookName}`)

#### Errors
- **[parameters]** ...
```

## Severity Definitions

- **Error**: Must be fixed. Violates a coding practice or UX guideline.
- **Warning**: Should be fixed. Potential issue or missing recommended practice.
- **Suggestion**: Nice to have. Minor improvement opportunity.

## Rules

- Never skip a component. Every module, connection, RPC, webhook, and function must appear.
- If a component has no issues, show it with "No issues found."
- Batch parallel MCP calls where possible.
- Reference specific best practice IDs (BP-XXX) in findings.
- Cross-reference API documentation when checking ID finder coverage.
- For each component, run ALL check categories — don't stop after finding the first issue.
- Every finding for modules, connections, RPCs, and webhooks must be prefixed with the **block** it belongs to: `[module]`, `[communication]`, `[mappable_parameters]`, `[static_parameters]`, `[interface]`, `[samples]`, `[api]`, `[parameters]`, `[attach]`, `[detach]`. Use `[module]` or `[connection]` for top-level metadata (label, description). Functions and their tests do not need a block prefix.
