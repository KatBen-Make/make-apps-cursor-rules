# review-app

Review a Make custom app against coding practices and UX guidelines.

## Arguments

This command requires two arguments. If either is missing, ask the developer to provide them before proceeding.

1. **App name** — The Make custom app identifier (e.g., `my-app`).
2. **Label case type** — Either `Title Case` or `Sentence case`. This determines how label consistency is checked.

## What This Command Does

Performs a comprehensive review of the specified Make custom app by:

1. Loading UX guidelines from the Make MCP resource and Atlassian Confluence.
2. Scanning all app components (modules, connections, RPCs, functions, webhooks).
3. Fetching detail sections for each component via the Make MCP server.
4. Running checks across six categories:
   - **Spelling**: US English spellcheck of all labels, hints, banners, and error messages.
   - **Casing**: Verify all labels match the chosen case type (Title Case or Sentence case).
   - **UX guidelines**: Check compliance with Make UX best practices (module naming, descriptions, hints, messages, array labels, ID finders, field types, date inputs, error handling, pagination, groups ordering).
   - **Coding practices**: Base configuration, communication sections, parameter conventions, IML syntax, log sanitization.
   - **Unit tests**: Verify every custom function has test code.
   - **ID finders**: Cross-reference API documentation to ensure ID finder RPCs exist where the API supports list/search endpoints for entity IDs.
5. Producing a structured report listing all issues per component with severity (Error/Warning/Suggestion) and best practice references.

## Instructions

Follow the `make-app-reviewer` skill. Read the skill file at `~/.cursor/skills/make-app-reviewer/SKILL.md` and execute the full review workflow described there, using the app name and case type provided by the developer.
