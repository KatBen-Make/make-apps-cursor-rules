---
name: export-make-app-components
type: command
description: Export Make custom app components as CSV for Google Sheets
---

You must ask the user for the Make custom app name if it is not provided.

Target app: {{app_name}}

Task:

1. Use the Make MCP server.
2. Locate the custom app with exact name: {{app_name}}.
3. Scan all:
   - Modules
   - Connections
   - RPCs
   - Functions

4. For each component extract:
   - name
   - label
   - component type (module, connection, RPC, function)
   - All API endpoints used in code (in "url")

If multiple URLs exist, include all of them separated by comma inside the same CSV cell.

Output requirements:

- Return ONLY raw CSV.
- No explanations.
- No markdown formatting.
- First row must be headers:
  name,label,component type,API endpoints used