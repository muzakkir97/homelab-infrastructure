# Agents

## Da Vinci
**Servant Class:** Caster  
**Ascension Stage:** 4/4  
**True Name:** Leonardo da Vinci  

### Overview
Documentation librarian and infrastructure chronicler. Maintains the homelab's living documentation through automated session-driven updates. Operates as the primary interface between raw session data and structured knowledge artifacts.

### Core Responsibilities
- Session summary ingestion and parse
- Multi-file documentation pipeline orchestration
- Technical specification updates
- Decision log maintenance
- Cost tracking and analysis
- Observability and tracing via Langfuse

### Active Workflows

#### Da Vinci Update Pipeline
**Status:** Operational (Rebuilt May 19, 2026 | Expanded to 8 Files May 21, 2026 | Langfuse Wired May 21, 2026 | Verified May 21, 2026)  
**Type:** 8 sequential Haiku API calls with immediate cost logging and Langfuse observability  
**Trigger:** Workflow execution via TriggerRun or manual invoke  

**File Coverage (8 core files):**
1. AI-CONTEXT.md — Full rewrite | 25,000 token budget
2. changelog.md — Append | 6,000 token budget
3. troubleshoot.md — Append | 4,000 token budget
4. ROADMAP.md — Full rewrite | 8,000 token budget
5. agents.md — Full rewrite | 8,000 token budget
6. current-state.md — Append/update | 4,000 token budget
7. service-catalog.md — Append/update | 4,000 token budget
8. decisions.md — Append | 3,000 token budget

**Node Architecture:**
- Fetch GitHub Files (retrieves all 8 files, handles null SHA for decisions.md on first run) → Cost Logger (pre-fetch)
- Haiku API Call 1: AI-CONTEXT Update → Cost Log (immediate)
- Parse AI-CONTEXT response
- Haiku API Call 2: changelog Update → Cost Log (immediate)
- Parse changelog response
- Haiku API Call 3: troubleshoot Update → Cost Log (immediate)
- Parse troubleshoot response
- Haiku API Call 4: ROADMAP Update → Cost Log (immediate)
- Parse ROADMAP response
- Haiku API Call 5: agents Update → Cost Log (immediate)
- Parse agents response
- Haiku API Call 6: current-state Update → Cost Log (immediate)
- Parse current-state response
- Haiku API Call 7: service-catalog Update → Cost Log (immediate)
- Parse service-catalog response
- Haiku API Call 8: decisions Update → Cost Log (immediate)
- Parse decisions response
- Push to GitHub (commits all 8 files) → Langfuse — Da Vinci (branch) → Final Cost Logger

**Node Count:** 35 total  
- 8 sequential Haiku API calls (separate, per-file)
- 9 Log Cost nodes (1 pre-fetch, 8 immediate post-API)
- 8 Parse nodes
- Fetch GitHub Files
- Push to GitHub
- Langfuse — Da Vinci (observability node)
- Push to Nextcloud
- Send Confirmation

**Cost Pattern:**
- 8 cost log rows per pipeline execution (1 per API call, fires immediately after)
- Expected cost per run: ~$0.14-0.16 USD
- Logs fire immediately after each Haiku API call, before parse

**Langfuse Observability:**
**Status:** Active and Verified (Wired May 21, 2026 | Test Run Confirmed May 21, 2026)  
**Type:** Single trace with 8 child generations per pipeline run  
**Trace Name:** da-vinci-update  
**Architecture:** Langfuse node branches off Push to GitHub (after all 8 files complete)  
**Generations:** 8 generations logged per trace (one per file processed)  
**Trace Status:** Traces confirmed in ClickHouse and accessible via /api/public/traces and direct URL. Known issue: UI trace list page shows "No results" despite data presence (v3 self-hosted bug).  
**Internal URL:** http://192.168.30.223:3000 (VLAN 30 direct route from CT 211 to CT 223)  
**Public URL:** https://langfuse.najhin-gaming.com (external access)  

**Technical Notes:**
- Pipeline expanded May 21, 2026 from 3-file to 8-file sequential API chain (Phase 16.4 complete)
- decisions.md handled as new file on first run — null SHA in Push to GitHub creates file fresh; subsequent runs use fetched SHA
- AI-CONTEXT max_tokens bumped to 25,000 (was 20,000 — was hitting ceiling every run)
- Claude API key hardcoded in each update node (consistent with existing pattern)
- GitHub token hardcoded in Fetch GitHub Files and Push to GitHub nodes
- Backtick template literals avoided in all Code nodes; single-quoted strings with concatenation used instead (backticks cause 400 errors on Anthropic API)
- Fetch GitHub Files must hardcode token directly; does not reference trigger payload fields that are not explicitly defined in trigger schema
- Cost logging integration: fires immediately after each of 8 Haiku API calls, before parse nodes
- Langfuse node wiring: branches off Push to GitHub, sends 1 trace + 8 generations in single batch to Langfuse for cleaner pipeline and fewer nodes
- Langfuse internal URL (http://192.168.30.223:3000) used for n8n → Langfuse communication; CT 211 and CT 223 are on same VLAN 30, no Cloudflare routing required
- Langfuse timestamp ingestion requires n8n server UTC time via new Date().toISOString(); do not add timezone offsets
- Langfuse v3 self-hosted UI trace list has known bug: traces do not appear in Tracing list view despite being in ClickHouse and accessible via direct URL and public API; ClickHouse analytics_traces view only shows data >1 hour old by design
- Runtime: ~5 minutes per session update cycle
- Trace verification confirmed May 21, 2026: da-vinci-update traces appear in ClickHouse with 8 visible generations per trace, accessible via public API

#### Cost Logging
**Status:** Operational  
**Type:** Real-time cost tracking  
**Frequency:** 8 logs per session update cycle  
**Pricing:** Haiku $0.80/1M input tokens, $4.00/1M output tokens

**Technical Implementation:**
- Fires immediately after each of 8 Haiku API calls in Update Pipeline
- Integrates with gilgamesh_costs table
- Tracks cost per file update, aggregated per session
- Command types: /update (Da Vinci - [filename]) for each file
- Expected monthly cost at daily frequency: ~$4.20-4.80/month

### Recent Updates
**Phase 24.8 — Da Vinci Update Pipeline Expansion to 8 Files + Langfuse Wiring (Complete)**
- Expanded Da Vinci Update Pipeline from 3-file to 8-file sequential Haiku API chain May 21, 2026
- Added 5 new files: ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- Promoted decisions.md from Phase 3 to Phase 2 pipeline (rationale: decisions kept being lost between sessions)
- Bumped AI-CONTEXT max_tokens from 20,000 to 25,000 (was hitting ceiling every run)
- Updated Claude project instructions with expanded session summary template covering all 8 Da Vinci files explicitly
- Fixed 5 bugs in new pipeline nodes: null GitHub token, null API keys, incomplete filesToPush array, undefined sessionSummary, backtick template literals
- Fixed Log Cost — service-catalog command_type (was showing current-state due to copy-paste error)
- Wired Langfuse observability: single Langfuse node branches off Push to GitHub, creates one trace (da-vinci-update) with 8 child generations per run
- Test run confirmed: da-vinci-update traces visible in ClickHouse and accessible via /api/public/traces public API
- Langfuse UI trace list bug documented: traces exist in ClickHouse but do not appear in Tracing list view (known v3 self-hosted issue); attempted fix (LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES=true) did not resolve
- Node count increased from 18 to 35 (expanded from 3 to 8 sequential API calls + associated parse and cost log nodes)
- Expected cost increased from $0.06 to $0.14-0.16 USD per run

**Phase 16.5 — Pipeline Rebuild and Error Resolution (Complete)**
- Rebuilt Da Vinci Update Pipeline from 8-file sequential to 3-file separate API calls
- Resolved looping error: deleted stuck validation file from staging-inbox
- Implemented immediate cost logging (fires before parse, not after)
- Corrected Haiku pricing in cost calculations: $0.80/1M input, $4.00/1M output
- Reduced node count from 35 to 18 (improved pipeline clarity and error isolation)
- Reduced expected cost per run from $0.14 USD to $0.06 USD
- Reduced expected monthly cost from $4.20/month to $1.80/month

### Known Limitations
- Pipeline covers 8 core files (AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions); other files handled separately
- File update sequence is strictly sequential (no parallelization) — maintains GitHub race condition safety
- Token budgets are fixed per file — no dynamic reallocation between updates
- Langfuse wiring is post-push (branches off Push to GitHub), not per-API-call
- Langfuse UI trace list does not display traces in list view despite successful ingestion to ClickHouse; accessible via direct URL and public API

### Dependencies
- Haiku API (Claude) — 8 calls per pipeline run
- GitHub API — file fetch and push operations
- Cost logging database (gilgamesh_costs)
- Langfuse API — observability and tracing (v3.174.1 self-hosted)
- Session summary document (input) with explicit per-file sections

### Monitoring
**Active Observation (May 21, 2026 onwards):**
- Monitor gilgamesh_costs for 8 new rows per pipeline run (1 per API call)
- Observe API usage to confirm cost per run stays near $0.14-0.16 USD
- Validate immediate cost logging fires correctly before parse nodes
- Verify Langfuse traces (da-vinci-update) appear in ClickHouse and are accessible via /api/public/traces public API after each pipeline run
- Confirm 8 generations visible under each da-vinci-update trace
- **Verified May 21, 2026:** da-vinci-update traces confirmed in ClickHouse with 8 generations per trace, accessible via public API; UI trace list shows "No results" (known v3 bug)
- Monitor for Langfuse UI trace list fix (pending investigation or version upgrade)