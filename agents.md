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
**Status:** Operational  
**Type:** Sequential Haiku API chain (8 files)  
**Trigger:** Workflow execution via TriggerRun or manual invoke  

**File Coverage (8 total):**
1. AI-CONTEXT.md — Full rewrite | 20,000 token budget
2. changelog.md — Append/update | 6,000 token budget
3. troubleshoot.md — Append/update | 4,000 token budget
4. ROADMAP.md — Full rewrite | 8,000 token budget
5. agents.md — Full rewrite | 8,000 token budget
6. current-state.md — Append/update | 4,000 token budget
7. service-catalog.md — Append/update | 4,000 token budget
8. decisions.md — Append | 3,000 token budget

**Node Architecture:**
- Fetch GitHub Files (retrieves all 8 files) → Cost Logger (pre-fetch)
- Sequential Haiku nodes: AI-CONTEXT Update → Cost Log → changelog Update → Cost Log → troubleshoot Update → Cost Log → ROADMAP Update → Cost Log → agents Update → Cost Log → current-state Update → Cost Log → service-catalog Update → Cost Log → decisions Update → Cost Log
- Push to GitHub (commits all 8 files, null SHA handling for decisions.md) → Final Cost Logger

**Cost Pattern:**
- 8 cost log rows per pipeline execution
- Expected cost per run: ~$0.25–0.35 USD
- Logs fire immediately after each Haiku API call

**Technical Notes:**
- Claude API key hardcoded in each update node (consistent with existing pattern)
- GitHub token hardcoded in Fetch GitHub Files and Push to GitHub nodes
- decisions.md created with null SHA on first pipeline run (new file handling)
- All 8 files pushed sequentially to GitHub with error handling
- File coverage expanded from 3-file pipeline (Phase 16.3) to 8-file pipeline (Phase 16.4)

#### Cost Logging
**Status:** Operational  
**Type:** Real-time cost tracking  
**Frequency:** 8 logs per session update cycle  

**Technical Implementation:**
- Fires immediately after each of 8 Haiku API calls in Update Pipeline
- Integrates with gilgamesh_costs table
- Tracks cost per file update, aggregated per session

### Recent Updates
**Phase 16.4 — Documentation Pipeline Expansion (Complete)**
- Expanded Update Pipeline from 3 files to 8 files
- Added ROADMAP.md, agents.md, current-state.md, service-catalog.md to core pipeline
- Promoted decisions.md from Phase 3 backlog to Phase 2 priority (core Update Pipeline)
- Updated token budgets per file for balanced coverage
- Resolved 3 bugs: null GitHub token, null API keys in 5 new nodes, Push to GitHub file array
- Updated Claude project instructions with explicit per-file session summary sections

### Known Limitations
- decisions.md initially created with null SHA (GitHub API requirement for new files) — resolved with conditional null SHA handling
- File update sequence is strictly sequential (no parallelization) — maintains GitHub race condition safety
- Token budgets are fixed per file — no dynamic reallocation between updates

### Dependencies
- Haiku API (Claude) — 8 calls per pipeline run
- GitHub API — file fetch and push operations
- Cost logging database (gilgamesh_costs)
- Session summary document (input)

---