# VERIFICATION — Phoenix Current Software (PCS)

**Verified:** 2026-03-28
**Repo:** `~/Documents/GITHUB_CLONED_REPOS_DONT_TOUCH/current`
**Branch:** `main` (3 commits, also has `governance-docs` branch on remote)
**Classification:** FLAGSHIP — Service Fusion replacement. KEEP AS-IS. Docs/README only.

---

## Repo Identity

- **Name:** Phoenix Current Software (PCS)
- **Package name:** `@phoenix/servicefusion-mcp` v2.0.0
- **Purpose:** TypeScript MCP server providing Service Fusion v1 API integration for Phoenix Electric LLC
- **Tagline:** "Current" = electrical current + up to date

---

## File Inventory

| Category | Count |
|----------|-------|
| Non-.git total files | 41 |
| TypeScript source files | 7 |
| Package/config files | 5 |
| Plugin files (commands, agent, skills, hooks) | 14 |
| Documentation files | 8 |
| Reference/spec files | 3 |
| Triage manifests (added this pass) | 3 |
| CODEOWNERS | 1 |
| README.md | 1 |

---

## MCP Tool Count

| Status | Count | Notes |
|--------|-------|-------|
| Active tools | 23 | Confirmed functional, all wired to live SF v1 endpoints |
| Deprecated stubs | 33 | Confirmed 404 on SF v1 API (discovery run 2026-03-10) |
| Total registered | 56 | All stubs present for backward compatibility |

### Active Tools by Category

| Category | Tools |
|----------|-------|
| CRM | `list_customers`, `get_customer`, `get_customer_equipment`, `create_customer`, `search_customers` |
| Jobs | `list_jobs`, `get_job`, `create_job`, `list_job_statuses`, `list_job_categories` |
| Estimates | `list_estimates`, `get_estimate`, `create_estimate` |
| Invoices | `list_invoices`, `get_invoice` |
| Technicians | `list_technicians`, `get_technician` |
| Calendar | `list_calendar_tasks`, `create_calendar_task` |
| Lookups | `list_payment_types`, `list_sources` |
| Meta | `me`, `health` |

Write-protected tools (require approval token or `ALLOW_SF_WRITES=true`): `create_customer`, `create_job`, `create_estimate`, `create_calendar_task`

---

## Architecture

### Transport Modes
- **stdio** — default, for Claude Code `--mcp` flag
- **http** — Express on port 3100, for Gateway or HTTP-based consumers. Endpoint: `POST/GET /mcp`

### Authentication
- Service Fusion OAuth 2.0 Client Credentials Grant
- Token endpoint: `POST https://api.servicefusion.com/oauth/access_token`
- Token caching with 60-second refresh buffer
- Refresh token support with automatic fallback to full re-auth

### Credential Source
- Primary: Azure Key Vault (`phoenixaaivault` / `AZURE_KEY_VAULT_URI`)
- Fallback: `SERVICEFUSION_CLIENT_ID` + `SERVICEFUSION_CLIENT_SECRET` env vars
- Key Vault secret names tried (production): `SERVICEFUSION-CLIENT-ID`, `ServiceFusion-ClientId`, `PhoenixAiCommandClientId`

### Rate Limiting
- Token bucket: 60 req/min (initialized), updates from `X-Rate-Limit-*` response headers
- README states 120 req/min — headers take precedence at runtime
- Queues up to 100 requests, handles 429 with Retry-After parsing and single retry

### Caching (GET responses)
- Lookup tables (`/job-statuses`, `/payment-types`, `/sources`, `/job-categories`): 5 min TTL
- `/me`: 10 min TTL
- All other GET endpoints: 60 sec TTL
- Cache invalidated on POST to same path prefix

---

## Plugin

| Item | Count |
|------|-------|
| Slash commands | 6 |
| Agents | 1 |
| Skills | 1 (with 6 reference docs) |

Commands: `/sf-briefing`, `/sf-jobs`, `/sf-customers`, `/sf-estimate`, `/sf-schedule`, `/sf-pricebook`
Agent: `sf-operations-agent` — autonomous multi-step SF operations orchestrator

---

## PATH BUG — BLOCKING (Do Not Lose This)

**File:** `plugin/.mcp.json`

Current content points to a dead path from the old staging repo:
```
/Users/shanewarehime/GitHub/phoenix-ai-core-staging/packages/servicefusion-mcp/dist/index.js
```

Correct path when activating from this repo (after `npm run build`):
```
/Users/shanewarehime/Documents/GITHUB_CLONED_REPOS_DONT_TOUCH/current/packages/mcp-server/dist/index.js
```

**No code was changed in this pass.** This is a documented bug for the activation phase.

---

## Key Documents

| File | Contents |
|------|----------|
| `docs/SERVICEFUSION_MCP_REWRITE_BRIEF.md` | MCP rewrite spec — correct endpoints, auth, what to build (2026-03-05) |
| `docs/api-surface.md` | Authoritative SF v1 API surface reference |
| `references/servicefusion-api-complete-spec.md` | 17,929-line processed API reference (75 types, 26 endpoints) |
| `references/servicefusion-api-spec.json` | Raw 4.3MB RAML specification |

---

## Governance Status

- No Product Bible yet (pending Wave B governance pass)
- No Build Doc yet (pending Wave B governance pass)
- README.md: current, accurate
- No `.env` files, no secrets in repo — clean
- `CODEOWNERS` present: assigns `@shane7777777777777` as owner of all files

---

*Verified by Phoenix Echo — Wave B Protected Repo Pass — 2026-03-28*
