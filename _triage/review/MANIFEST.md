# REVIEW — Flagged Items Needing Attention

## PATH BUG — plugin/.mcp.json

**File:** `plugin/.mcp.json`
**Severity:** BLOCKING (server will not start)

The `.mcp.json` points to a path in the old staging repo:

```json
"args": ["--experimental-specifier-resolution=node",
  "/Users/shanewarehime/GitHub/phoenix-ai-core-staging/packages/servicefusion-mcp/dist/index.js"]
```

The correct path relative to this repo's build output should point to:
```
/Users/shanewarehime/Documents/GITHUB_CLONED_REPOS_DONT_TOUCH/current/packages/mcp-server/dist/index.js
```

**Action required:** Update path when activating this server. Do NOT change the source code.
This is a docs/config note only — per Shane's directive, no code changes in this pass.

Triaged: 2026-03-28
