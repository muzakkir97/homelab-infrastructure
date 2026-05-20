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

### Active Workflows

#### Da Vinci Update Pipeline
**Status:** Operational (Rebuilt May 19, 2026)  
**Type:** 3 separate Haiku API calls with immediate cost logging  
**Trigger:** Workflow execution via TriggerRun or manual invoke  

**File Coverage (3 core files):**
1. AI-CONTEXT.md — Full rewrite | 20,000 token budget
2. changelog.md — Append | 6,000 token budget
3. troubleshoot.md — Append | 4,000 token budget

**Node Architecture:**
- Fetch GitHub Files (retrieves all 3 files) → Cost Logger (pre-fetch)
- Haiku API Call 1: AI-CONTEXT Update → Cost Log (immediate)
- Parse AI-CONTEXT response
- Haiku API Call 2: changelog Update → Cost Log (immediate)
- Parse changelog response
- Haiku API Call 3: troubleshoot Update → Cost Log (immediate)
- Parse troubleshoot response
- Push to GitHub (commits all 3 files) → Final Cost Logger

**Node Count:** 18 total  
- 3 sequential Haiku API calls (separate, per-file)
- 4 Log Cost nodes (1 pre-fetch, 3 immediate post-API)
- 3 Parse nodes
- Fetch GitHub Files
- Push to GitHub
- Push to Nextcloud
- Send Confirmation

**Cost Pattern:**
- 3 cost log rows per pipeline execution (1 per API call, fires immediately after)
- Expected cost per run: ~$0.06 USD
- Logs fire immediately after each Haiku API call, before parse

**Technical Notes:**
- Pipeline rebuilt May 19, 2026 to address looping error caused by stuck file in staging-inbox
- Reverted from 8-file sequential pipeline (Phase 16.4) to 3-file separate API calls (improved error isolation)
- Claude API key hardcoded in each update node (consistent with existing pattern)
- GitHub token hardcoded in Fetch GitHub Files and Push to GitHub nodes
- Backtick template literals avoided in all Code nodes; single-quoted strings with concatenation used instead (backticks cause 400 errors on Anthropic API)
- Fetch GitHub Files must hardcode token directly; does not reference trigger payload fields that are not explicitly defined in trigger schema
- Cost logging integration: fires immediately after each of 3 Haiku API calls, before parse nodes
- Runtime: ~2-3 minutes per session update cycle

#### Cost Logging
**Status:** Operational  
**Type:** Real-time cost tracking  
**Frequency:** 3 logs per session update cycle  
**Pricing:** Haiku $0.80/1M input tokens, $4.00/1M output tokens

**Technical Implementation:**
- Fires immediately after each of 3 Haiku API calls in Update Pipeline
- Integrates with gilgamesh_costs table
- Tracks cost per file update, aggregated per session
- Expected monthly cost at daily frequency: ~$1.80/month

### Recent Updates
**Phase 16.5 — Pipeline Rebuild and Error Resolution (Complete)**
- Rebuilt Da Vinci Update Pipeline from 8-file sequential to 3-file separate API calls
- Resolved looping error: deleted stuck validation file from staging-inbox
- Implemented immediate cost logging (fires before parse, not after)
- Corrected Haiku pricing in cost calculations: $0.80/1M input, $4.00/1M output
- Reduced node count from 35 to 18 (improved pipeline clarity and error isolation)
- Reduced expected cost per run from $0.14 USD to $0.06 USD
- Reduced expected monthly cost from $4.20/month to $1.80/month

### Known Limitations
- Pipeline covers 3 core files (AI-CONTEXT, changelog, troubleshoot); other files handled separately
- File update sequence is strictly sequential (no parallelization) — maintains GitHub race condition safety
- Token budgets are fixed per file — no dynamic reallocation between updates

### Dependencies
- Haiku API (Claude) — 3 calls per pipeline run
- GitHub API — file fetch and push operations
- Cost logging database (gilgamesh_costs)
- Session summary document (input)

### Monitoring
**Active Observation (May 19, 2026 onwards):**
- Monitor gilgamesh_costs for 3 new rows per pipeline run (1 per API call)
- Observe API usage for 24+ hours to confirm cost is under target ($0.06 USD per run)
- Validate immediate cost logging fires correctly before parse nodes