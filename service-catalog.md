# Service Catalog

## Overview
Central registry of all homelab services, APIs, and infrastructure components. Last updated: 2026-05-21.

## Da Vinci Documentation Pipeline
**Status:** Active  
**Phase:** 24.8 — Langfuse Wiring (Da Vinci)

### File Coverage
The Da Vinci Update Pipeline now handles 8 files per session update run:

| File | Update Strategy | max_tokens | Da Vinci Action |
|------|----------------|------------|-----------------|
| AI-CONTEXT.md | Full rewrite | 20000 | LLM merges session into master doc |
| changelog.md | Append | 6000 | New entry added at top |
| troubleshoot.md | Append | 4000 | New errors/resolutions added |
| ROADMAP.md | Full rewrite | 8000 | Phase statuses updated |
| agents.md | Full rewrite | 8000 | Agent roster and status updated |
| current-state.md | Append/update | 4000 | Container/hardware state updated |
| service-catalog.md | Append/update | 4000 | Service list updated |
| decisions.md | Append | 3000 | New decisions added at top |

**Previously:** 3 files (AI-CONTEXT.md, changelog.md, troubleshoot.md)

### Architecture
- **Pipeline Type:** Sequential Haiku API chain (8 separate calls, one per file)
- **API Calls per Run:** 8 (one per file)
- **Observability:** Langfuse wiring active — single trace (da-vinci-update) with 8 child generations logged per run
- **Langfuse Node Location:** Branched off Push to GitHub (after all 8 files complete)
- **Langfuse Internal URL:** http://192.168.30.223:3000 (CT 223 on VLAN 30, same as n8n CT 211)
- **Cost Logging:** Fires immediately after each API call (before parse/push); generates 8 cost rows per session update
- **Cost per Run:** ~$0.14 (previously ~$0.11 for 3-file pipeline)
- **Runtime:** ~5 minutes per run (previously ~4 minutes)
- **Haiku Pricing:** $0.80/1M input tokens, $4.00/1M output tokens

### New Files Added (Phase 16.4)
- **ROADMAP.md** — Full rewrite via Haiku API (8000 tokens)
- **agents.md** — Full rewrite via Haiku API (8000 tokens)
- **current-state.md** — Append/update via Haiku API (4000 tokens)
- **service-catalog.md** — Append/update via Haiku API (4000 tokens)
- **decisions.md** — New file; appended via Haiku API (3000 tokens); promoted from Phase 3 to Phase 2 priority

### GitHub Push Integration
- All 8 files pushed to GitHub after pipeline completion
- decisions.md handled with null SHA on first creation
- Push to GitHub node updated to include all 8 files in filesToPush array

### Langfuse Observability Integration (Phase 24.8)
- **Status:** Active
- **Trace Name:** da-vinci-update
- **Trace Structure:** 1 parent trace with 8 child generations (one per file: AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)
- **Node Architecture:** Single Langfuse node branched off Push to GitHub, executing after all 8 files complete and are pushed
- **Node Name:** Langfuse — Da Vinci (in agents.md workflow diagram)
- **Internal URL:** http://192.168.30.223:3000 (n8n CT 211 → Langfuse CT 223 on VLAN 30)
- **Batch Delivery:** All 8 generations sent in single request to Langfuse
- **Design Rationale:** Cleaner pipeline, fewer nodes, all generations grouped in single trace for better observability
- **Alternatives Rejected:** 8 individual Langfuse nodes after each Claude API call (too many nodes, marginal benefit)

### Recent Bug Fixes (Phase 16.4)
1. **Fetch GitHub Files:** Null githubToken → hardcoded token directly in node
2. **5 new Claude API nodes:** Null apiKey from trigger → hardcoded apiKey const at top of each node
3. **Push to GitHub:** Only 3 files in array → updated to all 8 files with null SHA handling
4. **sessionSummary reference:** Changed to read fileContent from trigger payload
5. **Claude API nodes:** Backtick template literals → replaced with single-quoted strings using concatenation
6. **Log Cost — service-catalog node:** Fixed command_type from `/update (Da Vinci - current-state)` to `/update (Da Vinci - service-catalog)`

### Pipeline Rebuild (2026-05-19)
- **Issue:** Pipeline looping on error due to stuck file in staging-inbox that failed validation repeatedly (every 15 minutes)
- **Resolution:** Deleted stuck file; rebuilt pipeline with per-file API calls and immediate cost logging
- **Testing:** Verified cost logging fires immediately after each Claude API call, before parse/push operations
- **Verification:** Monitor gilgamesh_costs for 3 new rows per pipeline run; observe API usage for 24 hours to confirm cost is under target

### Key Technical Notes
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; use single-quoted strings with concatenation instead
- Fetch GitHub Files must hardcode token directly; do not reference trigger payload fields that are not explicitly defined in trigger schema
- decisions.md on first creation uses null SHA path in GitHub push to create file fresh; subsequent runs use fetched SHA
- Cost logging must fire immediately after each API call, not after parse or push operations
- Langfuse integration uses internal VLAN URL to avoid unnecessary external routing; trace batching consolidates all 8 file generations into single observability record

### Dependencies
- Haiku 3.5 API (8 sequential calls, one per file)
- GitHub API (fetch files, push updates)
- Langfuse API (http://192.168.30.223:3000) for trace logging
- Workflow trigger payload (session summary via fileContent field, chatId)
- Telegram notification service (confirmation)
- n8n workflow runtime (~5 minutes per session)

## Monitoring & Verification
- **Cost Tracking:** 8 rows logged per run in gilgamesh_costs table
- **Observability Tracking:** 1 trace (da-vinci-update) with 8 child generations per run in Langfuse
- **File Updates:** All 8 files updated on GitHub after each session update
- **Telegram Alerts:** Confirmation message sent on completion
- **Langfuse UI:** Accessible at https://langfuse.najhin-gaming.com; pipeline traces visible with full generation details
- **Expected Cost Range:** ~$0.14 per full pipeline run (~$4.20/month at once daily)
- **Files Pushed:** AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md

---

*Catalog maintained by Da Vinci Documentation System. Last updated: 2026-05-21. Next review: Phase 25 planning.*