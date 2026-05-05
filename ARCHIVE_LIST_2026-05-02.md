# ARCHIVE LIST — Phoenix Current Software (PCS)

**Date:** 2026-05-02
**Compiled by:** Claude Opus 4.7 (1M ctx) — derived from `AUDIT_2026-05-02.md`
**Executor:** Phoenix Echo (CLI)
**Destination root:** `~/Documents/GIT-PHOENIX-HUB/phoenix-archive/`
**Action:** ARCHIVE ONLY — no deletion, ever. Per Shane's golden rules.
**Method:** `git mv` if you want history to follow, or plain `mv` if you want a clean cut. Either way, never `rm`.

> **Note on `.mcp.json`:** It's listed below as "archive" but functionally it needs to be **replaced** with a working version, not just removed. Move the broken one to the archive for record-keeping, then write a new one in its place during the design step. Do not leave the plugin without an `.mcp.json`.

---

## Summary

| Category | Items | Total bytes | Reason |
|---|---|---|---|
| 1. Aspirational skill references | 4 | 24,268 | Call deprecated tool stubs that always 404; document workflows the API can't serve |
| 2. Oversized / unread API dumps under `references/` | 3 | 5,502,224 (~5.3 MB) | Never imported by code, tests, or build; bloat every clone |
| 3. Broken or garbled docs | 1 | 4,999 | Markdown rendering broken from line 46 onward |
| 4. Dead config / wiring | 1 | 297 | Hardcoded path to dead `phoenix-ai-core-staging` repo |
| 5. Wrong tool prefix / wrong content | 2 | 2,039 | `sf_*` prefix tools that don't exist; references staging-repo paths |
| 6. Stale historical brief | 1 | 14,009 | Compiled 2026-03-05 to drive a one-time rewrite; rewrite is done; keep as history |
| 7. Misplaced internal docs | 1 | 13,402 | Generic CC plugin guide, not PCS-specific — belongs in cross-team docs |
| 8. Empty / vestigial scaffolds | 1 | 111 | `"hooks": {}` with no content |
| 9. Type shim of unclear value | 1 | 665 | Hand-written SDK typings — verify SDK now publishes these before archiving |
| **TOTAL** | **15** | **5,562,014 (~5.3 MB)** | |

---

## 1. Aspirational Skill References

These four files describe workflows or integrations that depend on Service Fusion endpoints that don't exist on v1 (confirmed 404 in the 2026-03-10 discovery run). They reference deprecated tool stubs (`sf_get_capacity`, `sf_compare_prices`, etc.) that throw errors when called. They are paper, not code.

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `plugin/skills/servicefusion-operations/references/workflows.md` | 7,446 B | Morning-briefing workflow calls 4 deprecated stubs (`sf_get_daily_job_summary`, `sf_get_missed_calls`, `sf_get_capacity`, `sf_get_on_call_technician`); other workflows similarly reference dead tools. The `/sf-briefing` command points users here. | `phoenix-archive/phoenix-current-software/skill-references-aspirational/` |
| `plugin/skills/servicefusion-operations/references/financials.md` | 5,192 B | Uses `sf_*` prefix throughout for tools that mostly don't exist (`sf_sell_estimate`, `sf_list_payments`); mixes real and phantom calls. | `phoenix-archive/phoenix-current-software/skill-references-aspirational/` |
| `plugin/skills/servicefusion-operations/references/rexel-integration.md` | 4,611 B | Entire document built around `sf_compare_prices`, `sf_list_materials`, `sf_create_material` — all confirmed-404 deprecated stubs. No real Rexel integration exists in this repo. | `phoenix-archive/phoenix-current-software/skill-references-aspirational/` |
| `plugin/skills/servicefusion-operations/references/future-gateway.md` | 7,019 B | Designs cron jobs that call deprecated `sf_*` stubs; uses `America/Phoenix` timezone (Arizona, no DST) when Phoenix Electric is in Denver, CO (`America/Denver`). Phoenix-rule violation per audit §5/P1. | `phoenix-archive/phoenix-current-software/skill-references-aspirational/` |

**Keep in repo:** `plugin/skills/servicefusion-operations/references/api-reference.md` (correct, just needs the stale `~/GitHub/phoenix-ai-core-staging/...` source-of-truth note rewritten) and `references/browser-fallback.md` (genuinely useful — documents SF web UI fallback strategy).

---

## 2. Oversized / Unread API Dumps

These three files under `references/` total 5.3 MB and are never imported by any code, tests, or build step in the repo. Confirmed by the audit: no source file reads them. They are upstream RAML, scrape output, and processed reference — useful as historical record, but they bloat every clone of this repo.

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `references/servicefusion-api-spec.json` | 4,357,826 B (4.2 MB) | Raw RAML spec. Unused by code/tests/build. Largest single file in repo. | `phoenix-archive/phoenix-current-software/api-dumps/` |
| `references/servicefusion-api-complete-spec.md` | 941,536 B (920 KB) | 17,929-line processed reference. Unused by code/tests/build. | `phoenix-archive/phoenix-current-software/api-dumps/` |
| `references/servicefusion-web-scrape.json` | 202,862 B (200 KB) | Raw web-scraped SF API data. Unused. | `phoenix-archive/phoenix-current-software/api-dumps/` |

**Note for Echo:** After moving, the `references/` directory will be empty. Decide: leave it with a `README.md` stub explaining where the dumps went, or remove the directory in the design step.

---

## 3. Broken or Garbled Docs

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `VERIFICATION.md` | 4,999 B | Cascading list-indentation bug from line 46 onward — file renders as deeply-nested-of-deeply-nested lists in any markdown viewer. Content is historically valuable (Browser Echo's 2026-04-05 verification), but the formatting is unrecoverable without manual rewrite. Archive the broken copy; design step can produce a clean replacement. | `phoenix-archive/phoenix-current-software/broken-docs/` |

---

## 4. Dead Config / Wiring

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `plugin/.mcp.json` | 297 B | Hardcoded absolute path to `/Users/shanewarehime/GitHub/phoenix-ai-core-staging/packages/servicefusion-mcp/dist/index.js` — that staging repo is dead. Plugin is unrunnable as committed. **Archive the broken file for record-keeping, but a working `.mcp.json` must be written in its place during design step.** Do not leave the plugin without one. | `phoenix-archive/phoenix-current-software/dead-config/` |

---

## 5. Wrong Tool Prefix / Wrong Content

These files are technically alive but contain enough wrong information that they're worse than nothing. Either they call tools by names that don't exist, or they reference dead paths.

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `plugin/commands/sf-customers.md` | 878 B | Uses `sf_*` prefix throughout — `sf_search_customers`, `sf_get_customer`, `sf_list_locations`, `sf_create_customer`, `sf_list_jobs`, `sf_list_customer_memberships`. None of those tool names exist in the registry (real prefix is `servicefusion_*`). Two of those names (`sf_list_locations`, `sf_list_customer_memberships`) map to deprecated stubs that throw. The slash command is broken end-to-end. | `phoenix-archive/phoenix-current-software/broken-commands/` |
| `plugin/commands/sf-pricebook.md` | 1,161 B | References `~/GitHub/phoenix-ai-core-staging/pricebook/` — dead path. The whole command is built around the assumption that local pricebook files live in the staging repo, which they don't (and shouldn't). | `phoenix-archive/phoenix-current-software/broken-commands/` |

**Note for Echo:** After archiving these two, the `plugin/commands/` directory will have 4 commands (`sf-briefing`, `sf-jobs`, `sf-estimate`, `sf-schedule`). The other four spot-check as correct. Design step rewrites these two against the real tool surface.

---

## 6. Stale Historical Brief

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `docs/SERVICEFUSION_MCP_REWRITE_BRIEF.md` | 14,009 B | Compiled 2026-03-05 by Echo Pro to drive the ServiceTitan→ServiceFusion rewrite. The rewrite is done — this brief's purpose is fulfilled. Some of its content (working curl commands, live data snapshot) is still useful as historical record. Archive in place, do not delete. Note: it cites `sf_*` tool names and 120 req/min rate limit, both contradicted by current code. | `phoenix-archive/phoenix-current-software/historical/` |

**Note for Echo:** `docs/api-surface.md` should stay in the repo — it's the current authoritative reference, and the audit recommends regenerating it via `api-discovery.sh` periodically.

---

## 7. Misplaced Internal Docs

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `plugin/PLUGIN_DEVELOPMENT_GUIDE.md` | 13,402 B | 416-line generic Claude Code plugin development guide — not PCS-specific. Belongs in a Phoenix-wide internal docs repo where the next plugin author can find it, not bundled inside one plugin's tree. Recommend moving (not copying) so there's one canonical location. | `phoenix-archive/cross-team-docs/claude-code-plugin-guide/` |

**Note for Echo:** This is the only entry that doesn't go under `phoenix-archive/phoenix-current-software/` — it's cross-team material, so it gets its own subtree. If there's already a Phoenix-wide docs repo, consider moving it there directly instead of into `phoenix-archive/`.

---

## 8. Empty / Vestigial Scaffolds

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `plugin/hooks/hooks.json` | 111 B | Empty stub: `{"description": "Service Fusion plugin hooks — optional session-start briefing reminder", "hooks": {}}`. No actual hooks defined. CC plugin auto-discovery treats this as zero hooks, identical to the file not existing. Archive for the description-string record, leave the directory in place if you might add hooks later. | `phoenix-archive/phoenix-current-software/vestigial/` |

---

## 9. Type Shim of Unclear Value

| Path | Size | Reason | Archive subpath |
|---|---|---|---|
| `packages/mcp-server/src/mcp-sdk.d.ts` | 665 B | Hand-written type declarations for `@modelcontextprotocol/sdk/server/stdio` and `@modelcontextprotocol/sdk/types`. The MCP SDK at `^1.25.1` likely publishes these types itself by now, making the shim either redundant (silent divergence risk) or a workaround for a real subpath-resolution problem (in which case the fix should be in `tsconfig` or imports, not a shim). **VERIFY before archiving** — if `tsc` fails after removal, restore it and note that in the design step. | `phoenix-archive/phoenix-current-software/verify-before-archiving/` |

**Note for Echo:** This one is conditional. Run `npm run build` (or `tsc --noEmit`) without it; if compilation succeeds, archive. If it fails, leave in place and flag for the design step.

---

## What stays in the repo (not archived)

For Echo's confidence: these are the load-bearing pieces the audit recommends KEEPING. Do not move any of these.

- `README.md`, `PRODUCT_BIBLE.md`, `BUILD_DOC.md`, `CODEOWNERS`
- `AUDIT_2026-05-02.md`, `ARCHIVE_LIST_2026-05-02.md` (this file)
- `packages/mcp-server/` source — `client.ts`, `cache.ts`, `rate-limiter.ts`, `tools/index.ts`, `index.ts`, package.json, tsconfig.json
- `packages/mcp-server/scripts/api-discovery.sh`
- `packages/shared/` — all of it
- `plugin/.claude-plugin/plugin.json`
- `plugin/commands/sf-briefing.md`, `sf-jobs.md`, `sf-estimate.md`, `sf-schedule.md` (4 of 6)
- `plugin/agents/sf-operations-agent.md`
- `plugin/skills/servicefusion-operations/SKILL.md`
- `plugin/skills/servicefusion-operations/references/api-reference.md`
- `plugin/skills/servicefusion-operations/references/browser-fallback.md`
- `docs/api-surface.md`

---

## Manifest template — paste into `phoenix-archive/MANIFEST_2026-05-02.md`

Echo: copy the table below into a fresh `MANIFEST_2026-05-02.md` (or append to an existing one) at `~/Documents/GIT-PHOENIX-HUB/phoenix-archive/MANIFEST_2026-05-02.md`. Fill in the `archived_at` timestamp per row as you execute the moves. The `source_repo` column lets you batch this with other archive operations from other Phoenix repos.

```markdown
# Phoenix Archive Manifest — 2026-05-02

**Moved by:** Phoenix Echo
**Source repo:** phoenix-current-software (commit 74c1598, branch main)
**Triggered by:** AUDIT_2026-05-02.md / ARCHIVE_LIST_2026-05-02.md
**Method:** `git mv` (history-preserving) | `mv` (clean cut) — note per row

| # | Source path (in source_repo) | Destination path (under phoenix-archive/) | Bytes | Reason | Archived at | Method |
|---|---|---|---|---|---|---|
| 1 | plugin/skills/servicefusion-operations/references/workflows.md | phoenix-current-software/skill-references-aspirational/workflows.md | 7446 | Calls deprecated stubs (sf_get_daily_job_summary, sf_get_missed_calls, sf_get_capacity, sf_get_on_call_technician) | YYYY-MM-DD HH:MM MT | mv |
| 2 | plugin/skills/servicefusion-operations/references/financials.md | phoenix-current-software/skill-references-aspirational/financials.md | 5192 | Mixes real and phantom sf_* tool calls; partly fictional | YYYY-MM-DD HH:MM MT | mv |
| 3 | plugin/skills/servicefusion-operations/references/rexel-integration.md | phoenix-current-software/skill-references-aspirational/rexel-integration.md | 4611 | Entire doc built around deprecated sf_compare_prices/sf_list_materials/sf_create_material stubs | YYYY-MM-DD HH:MM MT | mv |
| 4 | plugin/skills/servicefusion-operations/references/future-gateway.md | phoenix-current-software/skill-references-aspirational/future-gateway.md | 7019 | Aspirational gateway design; calls deprecated stubs; wrong timezone (America/Phoenix vs America/Denver) | YYYY-MM-DD HH:MM MT | mv |
| 5 | references/servicefusion-api-spec.json | phoenix-current-software/api-dumps/servicefusion-api-spec.json | 4357826 | 4.2 MB raw RAML spec, never imported by code/tests/build | YYYY-MM-DD HH:MM MT | mv |
| 6 | references/servicefusion-api-complete-spec.md | phoenix-current-software/api-dumps/servicefusion-api-complete-spec.md | 941536 | 17,929-line processed reference, never imported | YYYY-MM-DD HH:MM MT | mv |
| 7 | references/servicefusion-web-scrape.json | phoenix-current-software/api-dumps/servicefusion-web-scrape.json | 202862 | Raw web-scraped SF API data, never imported | YYYY-MM-DD HH:MM MT | mv |
| 8 | VERIFICATION.md | phoenix-current-software/broken-docs/VERIFICATION.md | 4999 | Markdown rendering broken from line 46 (cascading list indent) | YYYY-MM-DD HH:MM MT | mv |
| 9 | plugin/.mcp.json | phoenix-current-software/dead-config/plugin-mcp.json | 297 | Hardcoded path to dead phoenix-ai-core-staging repo. NOTE: replace with working .mcp.json before plugin is usable | YYYY-MM-DD HH:MM MT | mv |
| 10 | plugin/commands/sf-customers.md | phoenix-current-software/broken-commands/sf-customers.md | 878 | Uses sf_* prefix; tool names don't exist; references deprecated stubs | YYYY-MM-DD HH:MM MT | mv |
| 11 | plugin/commands/sf-pricebook.md | phoenix-current-software/broken-commands/sf-pricebook.md | 1161 | References dead ~/GitHub/phoenix-ai-core-staging/pricebook/ path | YYYY-MM-DD HH:MM MT | mv |
| 12 | docs/SERVICEFUSION_MCP_REWRITE_BRIEF.md | phoenix-current-software/historical/SERVICEFUSION_MCP_REWRITE_BRIEF.md | 14009 | Brief drove the ServiceTitan→ServiceFusion rewrite (2026-03-05); rewrite is done; keep as history | YYYY-MM-DD HH:MM MT | mv |
| 13 | plugin/PLUGIN_DEVELOPMENT_GUIDE.md | cross-team-docs/claude-code-plugin-guide/PLUGIN_DEVELOPMENT_GUIDE.md | 13402 | Generic CC plugin guide, not PCS-specific; belongs in cross-team docs | YYYY-MM-DD HH:MM MT | mv |
| 14 | plugin/hooks/hooks.json | phoenix-current-software/vestigial/hooks.json | 111 | Empty {"hooks": {}} stub | YYYY-MM-DD HH:MM MT | mv |
| 15 | packages/mcp-server/src/mcp-sdk.d.ts | phoenix-current-software/verify-before-archiving/mcp-sdk.d.ts | 665 | Hand-written SDK type shim; verify build still passes after removal before committing the archive | YYYY-MM-DD HH:MM MT | mv |

**Total bytes moved:** 5,562,014 (~5.3 MB)

## Post-archive verification

Echo: after the moves, run from the source repo root:

- `find references/ -type f` — should be empty (or only a stub README if you left one)
- `find plugin/skills/servicefusion-operations/references/ -type f | wc -l` — should be 2 (api-reference.md, browser-fallback.md)
- `find plugin/commands/ -type f | wc -l` — should be 4 (sf-briefing.md, sf-jobs.md, sf-estimate.md, sf-schedule.md)
- `cat plugin/.mcp.json 2>&1` — should fail (file moved). **Replace with a working version before declaring the plugin functional.**
- `npm run build --workspaces` (after the design step adds the root package.json) — should still succeed if mcp-sdk.d.ts removal was safe; if it fails, restore item 15 from the archive.

## Sign-off

- [ ] All 15 items moved
- [ ] Manifest filled in with timestamps
- [ ] No item was deleted (per Phoenix golden rules)
- [ ] phoenix-archive/ is committed/synced to wherever Phoenix archives live
- [ ] Replacement .mcp.json written (or design step explicitly deferred)

Phoenix Echo, signed: ____________________  Date: ____________________
```

---

*Archive list complete. No files moved. No deletions proposed. Awaiting Phoenix Echo execution.*
