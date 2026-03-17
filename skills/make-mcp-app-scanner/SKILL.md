---
name: make-mcp-app-scanner
description: Scan a Make custom app via MCP to extract all components (modules, connections, RPCs, webhooks, functions) with their names, labels, types, and API URLs. Use when the user asks to scan, audit, inspect, or list components of a Make custom app, or when they need a structured overview of an app's endpoints and structure.
---

# Make MCP App Scanner

Scan a Make custom app through the `user-make` MCP server and produce a structured JSON inventory of every component and its URLs.

## Required Input

- `appName` — the app identifier
- `appVersion` — the app version (number)

If the user doesn't provide these, ask before proceeding.

## Scan Workflow

### Step 1: Fetch the app base section

```
CallMcpTool  server: "user-make"  tool: "custom_apps_fetch"
  { appName, appVersion, sections: ["base"] }
```

Extract `baseUrl` from the base section. This is the root URL prefix inherited by all modules and RPCs.

### Step 2: Fetch all component lists (parallel)

Call these four fetches in parallel — they are independent:

| Tool | Arguments | Returns |
|------|-----------|---------|
| `custom_apps_modules_fetch` | `{ appName, appVersion }` | Array of module names/labels/types |
| `custom_apps_connections_fetch` | `{ appName }` | Array of connection names/labels |
| `custom_apps_rpcs_fetch` | `{ appName, appVersion }` | Array of RPC names/labels |
| `custom_apps_functions_fetch` | `{ appName, appVersion }` | Array of function names |
| `custom_apps_webhooks_fetch` | `{ appName }` | Array of webhook names/labels |

### Step 3: Fetch detail sections for each component

For every component returned in Step 2, fetch its URL-bearing sections:

**Modules** — for each module name:
```
custom_apps_modules_fetch
  { appName, appVersion, moduleName, sections: ["communication"] }
```
The `communication` section contains `url` (relative to baseUrl) and `method`.

**Connections** — for each connection name:
```
custom_apps_connections_fetch
  { connectionName, sections: ["api"] }
```
The `api` section contains endpoint URLs and auth-related URLs (token, authorize, etc.).

**RPCs** — for each RPC name:
```
custom_apps_rpcs_fetch
  { appName, appVersion, rpcName, sections: ["api"] }
```
The `api` section contains `url` and `method`.

**Webhooks** — for each webhook name:
```
custom_apps_webhooks_fetch
  { webhookName, sections: ["api", "attach", "detach"] }
```
The `api`, `attach`, and `detach` sections may contain URLs.

**Functions** — for each function name:
```
custom_apps_functions_get_code
  { appName, appVersion, functionName }
```
Search the returned code for any URL strings (patterns like `https://`, `http://`, or path literals starting with `/`).

### Step 4: URL extraction rules

For each component, collect URLs from:
- `url` fields in communication/api sections
- `baseUrl`, `authorize`, `token`, `invalidate`, `info` fields in connection api sections
- Inline URL strings found in function code
- `attach`/`detach` URL fields in webhook sections

If a URL is relative (starts with `/`), note it is relative to the app `baseUrl`.

### Step 5: Assemble output

Build a JSON structure with this shape:

```json
{
  "appName": "example-app",
  "appVersion": 1,
  "baseUrl": "https://api.example.com",
  "modules": [
    {
      "name": "getItems",
      "label": "Get Items",
      "typeId": 4,
      "typeName": "action",
      "connection": "example-app",
      "urls": ["/v1/items"]
    }
  ],
  "connections": [
    {
      "name": "example-app",
      "label": "My Connection",
      "type": "basic",
      "urls": ["https://api.example.com/oauth/token"]
    }
  ],
  "rpcs": [
    {
      "name": "rpc123",
      "label": "List Options",
      "connection": "example-app",
      "urls": ["/v1/options"]
    }
  ],
  "webhooks": [
    {
      "name": "wh456",
      "label": "Watch Events",
      "urls": ["/v1/webhooks"]
    }
  ],
  "functions": [
    {
      "name": "formatDate",
      "urls": []
    }
  ]
}
```

## Type ID Reference

| typeId | typeName |
|--------|----------|
| 1 | trigger (polling) |
| 4 | action |
| 9 | search |
| 10 | instant trigger (webhook) |
| 11 | responder |
| 12 | universal |

## Rules

- Never skip a component. Every module, connection, RPC, webhook, and function must appear in the output.
- If a component has no URLs, include it with an empty `urls` array.
- If multiple URLs exist for one component, return all of them as an array.
- Batch parallel MCP calls where possible to minimize round-trips.
- Present the final JSON to the user, formatted for readability.
