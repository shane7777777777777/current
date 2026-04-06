# VERIFICATION — Phoenix Current Software (PCS)

**Verified:** 2026-03-28 (original audit), 2026-04-05 (cleaned by Browser Echo)
**Classification:** FLAGSHIP — Service Fusion replacement. KEEP AS-IS.

---

## Repo Identity

| Field | Value |
|-------|-------|
| Name | Phoenix Current Software (PCS) |
| Package | `@phoenix/servicefusion-mcp` v2.0.0 |
| Purpose | TypeScript MCP server — Service Fusion v1 API integration for Phoenix Electric LLC |
| Tagline | "Current" = electrical current + up to date |

---

## MCP Tool Inventory

**23 active tools** confirmed functional against live SF v1 endpoints.
**33 deprecated stubs** confirmed 404 on SF v1 API (discovery run 2026-03-10).

### Active Tools

| Category | Tools |
|----------|-------|
| CRM | list_customers, get_customer, get_customer_equipment, create_customer, search_customers |
| Jobs | list_jobs, get_job, create_job, list_job_statuses, list_job_categories |
| Estimates | list_estimates, get_estimate, create_estimate |
| Invoices | list_invoices, get_invoice |
| Technicians | list_technicians, get_technician |
| Calendar | list_calendar_tasks, create_calendar_task |
| Lookups | list_payment_types, list_sources |
| Meta | me, health |

**Write-protected tools** (require approval token or `ALLOW_SF_WRITES=true`):
`create_customer`, `create_job`, `create_estimate`, `create_calendar_task`

---

## Architecture

### Transport
- **stdio** — default, for Claude Code `--mcp` flag
- - **http** — Express on port 3100, for Gateway or HTTP consumers (`POST/GET /mcp`)
 
  - ### Authentication
  - - SF OAuth 2.0 Client Credentials Grant
    - - Token endpoint: `POST https://api.servicefusion.com/oauth/access_token`
      - - Token caching with 60-second refresh buffer + automatic refresh fallback
       
        - ### Credentials
        - - **Primary:** Azure Key Vault (`phoenixaaivault`)
          - - **Fallback:** `SERVICEFUSION_CLIENT_ID` + `SERVICEFUSION_CLIENT_SECRET` env vars
           
            - ### Rate Limiting
            - - Token bucket: 60 req/min (updates from `X-Rate-Limit-*` headers at runtime)
              - - Queue capacity: 100 requests, 429 handled with `Retry-After` + single retry
               
                - ### Caching (GET responses)
                - - Lookup tables: 5 min TTL
                  - - `/me`: 10 min TTL
                    - - All other GET: 60 sec TTL
                      - - Cache invalidated on POST to same path prefix
                       
                        - ---

                        ## Plugin

                        | Component | Count |
                        |-----------|-------|
                        | Slash commands | 6 (`/sf-briefing`, `/sf-jobs`, `/sf-customers`, `/sf-estimate`, `/sf-schedule`, `/sf-pricebook`) |
                        | Agents | 1 (`sf-operations-agent` — autonomous multi-step SF orchestrator) |
                        | Skills | 1 (with 6 reference docs) |

                        ---

                        ## KNOWN BUG — BLOCKING

                        **File:** `plugin/.mcp.json`

                        The `.mcp.json` points to a dead path from the old staging repo:

                        ```
                        /Users/shanewarehime/GitHub/phoenix-ai-core-staging/packages/servicefusion-mcp/dist/index.js
                        ```

                        **Correct path** (after `npm run build` from this repo):

                        ```
                        <repo-root>/packages/mcp-server/dist/index.js
                        ```

                        **Action required:** Update path when activating this server. This should use a relative path or environment variable — never a hardcoded absolute path.

                        ---

                        ## File Inventory

                        | Category | Count |
                        |----------|-------|
                        | TypeScript source | 7 |
                        | Package/config | 5 |
                        | Plugin files | 14 |
                        | Documentation | 8 |
                        | Reference/spec | 3 |
                        | Governance (CODEOWNERS, README) | 2 |

                        ---

                        ## Key Documents

                        | File | Contents |
                        |------|----------|
                        | `docs/SERVICEFUSION_MCP_REWRITE_BRIEF.md` | MCP rewrite spec — correct endpoints, auth, build plan (2026-03-05) |
                        | `docs/api-surface.md` | Authoritative SF v1 API surface reference |
                        | `references/servicefusion-api-complete-spec.md` | 17,929-line processed API reference (75 types, 26 endpoints) |
                        | `references/servicefusion-api-spec.json` | Raw 4.3MB RAML specification |

                        ---

                        *Verified by Phoenix Echo (Wave B, 2026-03-28). Cleaned and committed by Browser Echo (2026-04-05).*
