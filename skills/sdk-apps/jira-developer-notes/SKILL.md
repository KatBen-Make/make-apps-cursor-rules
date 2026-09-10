---
name: jira-developer-notes
description: >-
  Write structured Developer Notes for a Jira issue using a fixed three-section
  format (Analysis, Changes, QA — what to test) with concise bullet points
  aimed at the committer and the tester. Use when the user asks to write, draft,
  format, or update Developer Notes on a Jira ticket, or to summarize what was
  analyzed, changed, and what QA should test.
---

# Developer Notes in Jira

Write Developer Notes in a fixed three-section structure. Keep every bullet simple, concise, and easy to read for the committer and the tester.

## Structure

Always use these three sections, in this order. Render each section title as **Heading 3** in Jira's editor (the ADF rich-text field), not as plain text:

```
### Analysis
- ...

### Changes
- ...

### QA — what to test
- ...
```

## Section rules

- **Analysis** — Why the change is needed. State the problem plainly (e.g. mismatch with API documentation, bug, missing field).
- **Changes** — What was changed. Every bullet must say:
  - which **component** (module, RPC, function, connection, webhook...),
  - the **label** of the component (especially if the name differs a lot from the label),
  - **where** the change is (e.g. mappable parameters, communication, RPC body, expect, interface).
- **QA — what to test** — Concrete steps a tester can follow to verify the change.

## Style

- Use simple, concise bullet points.
- One change or one test per bullet.
- Prefer exact identifiers and values (field names, `required: true`, etc.) over vague descriptions.
- Component name should be bold (e.g. `- Module **Create a contact** — ...`).

## Jira ADF formatting constraints

Developer Notes is a rich-text (ADF) custom field (`customfield_10483`). When writing it programmatically (e.g. via the Atlassian MCP `editJiraIssue` tool with `contentFormat: "adf"`), keep these in mind to avoid a generic `INVALID_INPUT` error:

- Do **not** combine `strong` and `code` marks on the same text node — Jira's ADF validator rejects it. Pick one per span: use `strong` (bold) for component names, and `code` for inline identifiers; if you want a component name to read as code, put it in its own `code`-only span.
- Do **not** include an empty `marks: []` array on a text node. Omit the `marks` key entirely when there is no mark.
- Render each section title (`Analysis`, `Changes`, `QA — what to test`) as a `heading` node with `attrs.level: 3`.
- Render bullets as a `bulletList` of `listItem` nodes, each containing a `paragraph`.
- When a write fails with `INVALID_INPUT` (empty `errors` object), isolate the problem by first setting a minimal valid ADF doc, then re-add nodes/marks until the offending one is found.

## Example

```
### Analysis
- Module **Create a contact** has required parameter "name", but according to API documentation it should not be required.

### Changes
- Module **Create a contact** — mappable parameters: removed `required:true` from parameter "name"

### QA — what to test
- Module **Create a contact**: create a contact without name.
```
