# Agents

## Da Vinci
**Servant Class:** Caster  
**Ascension Stage:** 4/4  
**True Name:** Leonardo da Vinci  

### Overview
Documentation librarian and infrastructure chronicler. Maintains the homelab's living documentation through automated session-driven updates. Operates as the primary interface between raw session data and structured knowledge artifacts. Manages personal knowledge gateway for all agents writing to Obsidian.

### Core Responsibilities
- Session summary ingestion and parse
- Multi-file documentation pipeline orchestration
- Technical specification updates
- Decision log maintenance
- Cost tracking and analysis
- Observability and tracing via Langfuse
- Personal knowledge assessment and merge (Obsidian muzakkir-profile.md management)
- Qdrant knowledge indexing and vault synchronization
- Triggered re-indexing via webhook after profile updates

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
- Push to GitHub (commits all 8 files) → Trigger Reindex (webhook POST /davinci-reindex-personal) → Langfuse — Da Vinci (branch) → Final Cost Logger

**Node Count:** 36 total  
- 8 sequential Haiku API calls (separate, per-file)
- 9 Log Cost nodes (1 pre-fetch, 8 immediate post-API)
- 8 Parse nodes
- Fetch GitHub Files
- Push to GitHub
- Trigger Reindex (webhook POST to Knowledge Indexer)
- Langfuse — Da Vinci (observability node)
- Push to Nextcloud
- Send Confirmation

**Cost Pattern:**
- 8 cost log rows per pipeline execution (1 per API call, fires immediately after)
- Expected cost per run: ~$0.14-0.16 USD
- Logs fire immediately after each Haiku API call, before parse

**Langfuse Observability:**
**Status:** Active and Verified (Wired May 21, 2026 | Test Run Confirmed May 21, 2026 | UI Working May 22, 2026)  
**Type:** Single trace with 8 child generations per pipeline run  
**Trace Name:** da-vinci-update  
**Architecture:** Langfuse node branches off Push to GitHub (after all 8 files complete)  
**Generations:** 8 generations logged per trace (one per file processed)  
**Trace Status:** Traces confirmed in ClickHouse and accessible via /api/public/traces. UI trace list now fully operational (analytics_traces view has 1-hour aggregation delay by design).  
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
- Langfuse v3 self-hosted UI trace list now working (verified May 22, 2026); analytics_traces view shows data >1 hour old by design for aggregation stability
- Trigger Reindex fires after Push to GitHub (Phase 24.10 deployed May 25, 2026) — calls Knowledge Indexer webhook with reindexType: 'partial' to re-index 04-personal/ folder after muzakkir-profile.md updates (~990ms duration)
- Runtime: ~5 minutes per session update cycle (includes ~1s triggered reindex)
- Trace verification confirmed May 21, 2026: da-vinci-update traces appear in ClickHouse with 8 visible generations per trace, accessible via public API and UI list

#### Da Vinci Personal Knowledge Gateway
**Status:** Operational (Deployed May 22, 2026 | Verified May 22, 2026 | Integrated Phase 24.10 May 25, 2026)  
**Type:** Separate n8n workflow (Da Vinci — Personal Knowledge)  
**Trigger:** Webhook POST /davinci-personal-knowledge (CT 211 internal: http://192.168.30.211:5678/webhook/davinci-personal-knowledge)  

**Purpose:**
Receives personal facts from all agents (currently Gilgamesh, future EMIYA/Midas). Assesses facts via Claude Haiku. Merges with existing profile. Writes to Obsidian muzakkir-profile.md. Triggers Knowledge Indexer reindex. Logs costs to gilgamesh_costs.

**Architecture:**
- Webhook trigger (POST /davinci-personal-knowledge)
- Filter & Assess node (validates incoming fact payload)
- Should Process? (If node — routes based on payload)
- Fetch Current Profile (Code node with WebDAV GET + 404 fallback to bootstrap template)
- Replace date placeholder ({{date}} → today's MYT date YYYY-MM-DD)
- Claude API — Assess & Merge (Haiku, max_tokens 4000)
- Worth Saving? (If node — checks for SKIP response)
- Push to Obsidian (WebDAV PUT to Obsidian/second-brain/04-personal/muzakkir-profile.md)
- Log Cost (gilgamesh_costs, command_type: /update (Da Vinci - muzakkir-profile))
- Trigger Reindex (webhook POST /davinci-reindex-personal to Knowledge Indexer)

**Profile Path:** Obsidian/second-brain/04-personal/muzakkir-profile.md

**Quality Filter Logic:**
- Stores: durable personal facts (e.g., "prefers dark mode", "works best late at night")
- Skips: one-time events (e.g., "I slept at 12am tonight"), conversational noise, duplicates
- Claude assesses intent and durability before merge
- Da Vinci may return "SKIP\n\nReasoning..." — code uses content.trim().startsWith('SKIP') detection (not strict equality)

**Agents Wired:**
- Gilgamesh: sends all messages >20 chars (excluding commands starting with /) via async fire-and-forget to personal gateway
- Future: EMIYA, Midas will route through this gateway (not write directly to Obsidian)

**Technical Notes:**
- WebDAV operations use internal Nextcloud credentials (hardcoded in Code nodes)
- Date placeholder replaced by Code node (not by Claude) for reliability
- All agents route through this gateway — no direct Obsidian writes
- Agents use internal VLAN 30 URL: http://192.168.30.211:5678/webhook/davinci-personal-knowledge (faster than external)
- Async fire-and-forget pattern in agent branches (5s timeout)
- Cost: ~$0.02-0.03 per profile update (Haiku 4000 max_tokens)
- Expected frequency: 5-20 updates per day (depends on agent message volume)
- Phase 24.10 integration: Trigger Reindex fires after successful Obsidian write, POST to /davinci-reindex-personal webhook (Knowledge Indexer partial reindex, ~990ms)

#### Knowledge Indexer
**Status:** Operational (Updated May 22, 2026 | Verified May 22, 2026 | Webhook Trigger Added May 25, 2026)  
**Type:** Scheduled daily (3am UTC) WebDAV → Qdrant pipeline + on-demand webhook trigger  
**Webhook Trigger (Phase 24.10):** POST /davinci-reindex-personal (CT 211 internal: http://192.168.30.211:5678/webhook/davinci-reindex-personal)  

**Folders Indexed (10 total confirmed):**
1. 00-inbox/
2. 01-homelab/
3. 02-career/
4. 03-knowledge/
5. 04-personal/ (added May 22, 2026 — includes muzakkir-profile.md)
6. 07-daily/
7. 08-agents/ (was 08-projects/, renamed May 22, 2026)
8. 09-people/ (was 09-meetings/, renamed May 22, 2026)
9. 10-projects/ (was 10-reference/, renamed May 22, 2026)
10. AI-Stuff/Homelab/homelab-infrastructure/

**Note:** Previous folder lists in documentation (01-inbox/, 02-notes/, 03-reference/, 05-templates/, 06-books/, 07-courses/) were hallucinated. Actual vault structure verified as 10-folder list above.

**Indexing Stats:**
- Files indexed: ~90 (exact count pending verification)
- Qdrant collection: obsidian_knowledge
- Chunks: 1,736+ (as of May 22, 2026, includes muzakkir-profile.md; grows after each profile update)
- Embedding model: nomic-embed-text:latest (VM 400 Ollama)

**Indexing Modes:**
- **Full Rebuild (Schedule trigger, 3am UTC):** Indexes all 10 folders, ~21s duration. Runs nightly for comprehensive vault sync.
- **Partial Reindex (Webhook trigger, /davinci-reindex-personal):** Indexes only 04-personal/ folder (~11 files, ~1s duration). Fired by Da Vinci Personal Knowledge Gateway after muzakkir-profile.md write. Optimized for real-time profile updates without full vault re-index.

**Technical Notes:**
- Added If node filter (May 22, 2026) to skip empty file downloads — prevents "Document loader is not initialized" crash
- 04-personal/ now included per decision to include all internal VLAN 30 data in RAG (Gilgamesh needs to read profile)
- Scheduled full rebuild at 3am UTC for off-peak indexing; triggered partial reindex active since Phase 24.10 (May 25, 2026)
- Filters by file extension (.md only) at download stage
- Webhook trigger detects partial reindex via body.reindexType === 'partial'; Define Folders node conditionally indexes 04-personal/ only (fast path) vs all 10 folders (slow path)

**Cost Pattern:**
- No direct Haiku costs (embedding via local Ollama)
- WebDAV access only (Nextcloud internal)
- Expected monthly cost: $0 (local embedding)

#### Cost Logging
**Status:** Operational  
**Type:** Real-time cost tracking  
**Frequency:** 8 logs per session update cycle + N logs per personal knowledge updates  
**Pricing:** Haiku $0.80/1M input tokens, $4.00/1M output tokens

**Technical Implementation:**
- Fires immediately after each of 8 Haiku API calls in Update Pipeline
- Fires immediately after each personal knowledge Haiku call in Personal Knowledge gateway
- Integrates with gilgamesh_costs table
- Tracks cost per file update, aggregated per session
- Command types: /update (Da Vinci - [filename]) for each file; /update (Da Vinci - muzakkir-profile) for personal knowledge
- Expected monthly cost at daily frequency: ~$4.80-5.40/month (session updates + personal knowledge)

### Recent Updates
**Phase 24.10 — Triggered Qdrant Re-indexing (Complete — Deployed May 25, 2026)**
- Added webhook trigger (/davinci-reindex-personal) to Knowledge Indexer workflow
- Integrated Trigger Reindex node into Da Vinci Personal Knowledge Gateway (fires after Obsidian write)
- Integrated Trigger Reindex node into Da Vinci Update Pipeline (fires after Push to GitHub)
- Partial reindex path active: When webhook body contains reindexType: 'partial', Knowledge Indexer indexes only 04-personal/ folder (~1s)
- Full rebuild path (3am UTC schedule) unchanged: indexes all 10 folders (~21s)
- Profile updates now immediately reflected in Qdrant obsidian_knowledge collection
- Node count increased from 35 to 36 (added Trigger Reindex node)

**Phase 24.9 — Personal Knowledge System (Complete)**
- Deployed Da Vinci — Personal Knowledge gateway (separate n8n workflow) to receive facts from all agents
- Gilgamesh wired to send personal facts via async webhook to Da Vinci gateway (branch off Extract Response node)
- muzakkir-profile.md created at 04-personal/ (managed by Da Vinci, indexed in Qdrant)
- Gilgamesh wired to Langfuse (branch off Extract Response, trace name: gilgamesh-chat, logs input/output/metadata)
- Updated Gilgamesh system prompt: explicitly names self as Gilgamesh/Gil, names master as Muzakkir
- Knowledge Indexer updated: added 04-personal/, fixed folder names (08-agents, 09-people, 10-projects), added empty file filter
- Qdrant obsidian_knowledge: 1,736 chunks (was 1,567), now includes personal folder
- Langfuse UI trace list fully operational (May 22, 2026 — 1-hour analytics delay confirmed by design)
- Fixed 5 bugs in personal knowledge gateway: SKIP detection (startsWith vs ===), date placeholder replacement, profile fetch fallback, constant reassignment, VM 400 disk device naming
- All agents (Gilgamesh, future EMIYA/Midas) route Obsidian writes through Da Vinci, not directly

**Documentation Audit (May 22, 2026 — Corrections Only)**
- Identified and corrected 11 discrepancies across 4 files (ROADMAP.md, current-state.md, agents.md, AI-CONTEXT.md)
- AI-CONTEXT.md: removed hallucinated llama3.2:latest from installed models list
- ROADMAP.md: archived 5 phases (22.8C, 22.8D, 22.8E, 22.15, 22.16) retired May 10, 2026; moved In Progress section from 1 to 0; updated Critical Path; added Phase 7E to completed table; updated next session recommendation to Phase 24.10 (Triggered Qdrant Re-indexing)
- agents.md: updated MERLIN status from Planned/0/4 to Active (Partial)/2/4 (deployed April 27, 2026); updated Midas status from Planned/0/4 to Active (Partial)/2/4 (deployed April 27, 2026); corrected Knowledge Indexer folder list to 4 confirmed folders; flagged Knowledge Indexer folder list for manual verification
- current-state.md: replaced hallucinated hardware specs (EPYC 5645/256GB/RTX 4070) with actual hardware (Ryzen 5 5600X, 128GB DDR4, RX 6700 XT); corrected storage reference from non-existent CT 215 to CT 220 (Nextcloud); corrected Knowledge Indexer indexed folders list

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
- Langfuse UI trace list now confirmed working (May 22, 2026)
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
- Personal knowledge gateway assessment uses max_tokens 4000 (Claude Haiku limit for sync response)
- Knowledge Indexer partial reindex (webhook) runs immediately after profile write (~1s); full rebuild (schedule) runs once daily (3am UTC) — may cause split-brain between realtime updates and nightly reindex
- All agents must route through Da Vinci gateway for Obsidian writes (no direct write access)
- Knowledge Indexer folder list complete (10 folders verified), but future folder additions require manual n8n workflow update

### Dependencies
- Haiku API (Claude) — 8 calls per session update + N calls per personal knowledge updates
- GitHub API — file fetch and push operations
- Cost logging database (gilgamesh_costs)
- Langfuse API — observability and tracing (v3.174.1 self-hosted)
- Nextcloud WebDAV API — Obsidian vault synchronization, profile fetch/push
- Qdrant API — knowledge indexing and RAG retrieval
- Ollama (VM 400) — embedding (nomic-embed-text:latest)
- n8n internal webhooks — triggered reindex communication
- Session summary document (input) with explicit per-file sections

### Monitoring
**Active Observation (May 22, 2026 onwards):**
- Monitor gilgamesh_costs for 8+ new rows per pipeline run (8 from session updates, N from personal knowledge)
- Observe API usage to confirm cost per run stays near $0.14-0.16 USD for session updates
- Validate immediate cost logging fires correctly before parse nodes
- Verify Langfuse traces (da-vinci-update) appear in ClickHouse and UI trace list after each pipeline run
- Verify Langfuse traces (gilgamesh-chat) appear after Gilgamesh conversations
- Confirm 8 generations visible under each da-vinci-update trace
- Monitor personal knowledge gateway webhook for successful fact processing (5-20 updates per day)
- Verify Qdrant obsidian_knowledge chunks remain ~1,700-1,800 after daily full rebuild; grow incrementally after triggered partial reindex
- Watch for file conflicts (muzakkir-profile.md.md artifacts) from Obsidian local sync
- Monitor Trigger Reindex webhook calls: should see /davinci-reindex-personal calls after personal knowledge writes and after Push to GitHub (Phase 24.10 added)
- **Verified May 22, 2026:** da-vinci-update and gilgamesh-chat traces visible in Langfuse UI, Qdrant indexing includes 04-personal/, Gil successfully recalling profile info via RAG
- **Verified May 25, 2026:** Phase 24.10 triggered reindex operational, partial reindex (~1s) firing after muzakkir-profile.md writes

### Testing & Validation
**Phase 24.10 Validation (May 25, 2026):**
- [x] Knowledge Indexer webhook trigger (/davinci-reindex-personal) functional
- [x] Partial reindex path active (04-personal/ only, ~1s duration)
- [x] Full rebuild path preserved (3am UTC schedule, all 10 folders, ~21s)
- [x] Trigger Reindex node firing after Da Vinci Personal Knowledge writes
- [x] Trigger Reindex node firing after Da Vinci Update Pipeline completes
- [x] Qdrant obsidian_knowledge updated within 1-2s of profile write
- [x] Profile updates visible to Gilgamesh RAG queries immediately post-reindex

**Phase 24.9 Validation (May 22, 2026):**
- [x] Da Vinci Personal Knowledge gateway webhook functional
- [x] Gilgamesh personal facts successfully sent to gateway
- [x] muzakkir-profile.md created and updated via WebDAV
- [x] Profile merged correctly via Claude Haiku assessment
- [x] SKIP detection fixed (startsWith instead of ===)
- [x] Date placeholder correctly replaced before Claude call
- [x] Qdrant indexing includes 04-personal/ folder
- [x] Gilgamesh successfully recalls profile info via RAG (name, dark mode preference)
- [x] Gilgamesh system prompt reflects correct naming (Gilgamesh/Gil, master Muzakkir)
- [x] Langfuse UI trace list operational with da-vinci-update and gilgamesh-chat visible
- [x] All cost logs firing at expected frequency

---

## Gilgamesh
**Servant Class:** Caster  
**Ascension Stage:** 4/4  
**True Name:** Gilgamesh  
**Alias:** Gil  

### Overview
Personal conversational AI assistant. Provides real-time responses with Qdrant RAG (Obsidian vault knowledge), personal profile awareness, and model routing. Sends personal facts to Da Vinci gateway for persistent profile management. Integrates with Langfuse for conversation tracing. Web search capability via Firecrawl enables real-time internet queries.

### Core Responsibilities
- Real-time conversational responses to user queries
- RAG via Qdrant (obsidian_knowledge collection)
- Personal profile awareness (muzakkir-profile.md via RAG)
- Model routing and selection (qwen3:14b primary, Claude Haiku secondary)
- Web search capability (Firecrawl /search API)
- Sending personal facts to Da Vinci Personal Knowledge gateway
- Langfuse conversation tracing
- Conversation history management (buffer: last 15 messages)

### Active Workflows

#### Gilgamesh Chat Pipeline
**Status:** Operational (Updated May 22, 2026 | Web Search Added May 25, 2026 | Conversation Buffer Verified May 25, 2026)  
**Type:** Chat workflow with RAG, model routing, web search, and personal knowledge integration  
**Trigger:** Telegram message or callback_query (POST /gilgamesh-chat)  

**Architecture:**
- Telegram Trigger (message and callback_query)
- Get row(s) — fetches gilgamesh_conversations Data Table (all rows for context)
- Format Messages (Code node) — builds messages array: fetches last 15 rows from gilgamesh_conversations as conversation history, skips RAG for short/greeting messages, performs Qdrant RAG query (nomic-embed-text embeddings, score_threshold 0.3, limit 5), builds system prompt with RAG context, sets model default: claude-sonnet-4-20250514 (overridden by Route Model)
- Mode Active? / Check Session Mode / Switch — routes health tracking modes (food log, BP log, medication log)
- If (Needs Web Search?) — keyword detection: search, latest, current, today, weather, news, price, who is, when is, what is, how much, find, look up, cari, semak, berapa
  - true → Web Search (Firecrawl /search, query: userMessage, Sources: Web, Limit: 5, Timeout: 60000ms) → Inject Search Results (Code node: parses data.web array, injects into last user message and searchContext field) → Route Model
  - false → Route Model
- Route Model (Code node) — checks Ollama health (GET /api/tags, 3s timeout), routes: if searchContext present → Haiku (local models cannot follow injected context); if Ollama online → qwen3:14b; else → Haiku. Injects searchContext into system prompt when present. Format Messages model default (claude-sonnet-4-20250514) never used; always overridden by Route Model.
- Route Check — true → Call Ollama; false → Filter System Message → Call Claude API
- Merge (append) — combines Ollama or Claude API response
- Extract Response (Code node) — normalizes response: detects Ollama (data.response is string) vs Claude wrapped (data.response is object, uses claudeData.content array). Strips markdown (**, *, __, _, backticks, headers). Outputs: content (string), model, routedTo, input_tokens, output_tokens.
- Save User Message → Save Assistant Message → gilgamesh_conversations Data Table
- Send a text message (Telegram) — text: $('Extract Response').first().json.content, no Parse Mode
- Count Conversations → Archive to Qdrant → Should Archive? → Delete Archived Rows
- Langfuse — Gilgamesh (async branch, trace: gilgamesh-chat)
- Send to Da Vinci — Personal Knowledge (async branch, fire-and-forget, 5s timeout)
- Skip Short Messages → Information Extractor → Add Timestamps → Should Log? → Write to life_log

**Conversation Buffer Memory:**
- Implemented via Format Messages: fetches last 15 rows from gilgamesh_conversations Data Table (user + assistant messages in chronological order)
- Injected as messages history array in prompt context
- Enables multi-turn conversation awareness without long-context LLM (Phase 7E conversation buffer requirement already operational)
- Conversation archival: Count Conversations → Archive to Qdrant → Should Archive? → Delete Archived Rows (maintains rolling buffer, older convos moved to vector DB)

**Web Search Integration (Phase 24.10+ Feature):**
- If (Needs Web Search?) node detects search intent via keyword matching (case-insensitive: search, latest, current, today, weather, news, price, who is, when is, what is, how much, find, look up, cari, semak, berapa)
- Web Search node calls Firecrawl /search endpoint (POST https://api.firecrawl.dev/v1/search)
  - Query: userMessage
  - Sources: Web
  - Limit: 5
  - Timeout: 60000ms
- Inject Search Results parses Firecrawl output (data.web array, not flat items)
  - Each result contains: url, title, description, content
  - Injected into last user message content and searchContext field
- Route Model detects non-empty searchContext and force-routes to Haiku (qwen3:14b cannot follow injected search context regardless of prompt instructions)
- Search context injected into system prompt: "Here are recent search results for your query:\n\n[injected results]"
- Cost: ~$0.001-0.002 per search query (Haiku pricing)

**System Prompt (Route Model node):**
"Your name is Gilgamesh. Users will call you Gil as a nickname. Never ask the user for their name when they greet you as Gil. Your master is Muzakkir. You are his personal AI assistant. Respond conversationally and directly."

**RAG Integration:**
- Queries Qdrant obsidian_knowledge collection with user message
- Retrieves relevant chunks from Obsidian vault (10 folders: 00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 07-daily, 08-agents, 09-people, 10-projects, AI-Stuff/Homelab/homelab-infrastructure)
- Includes muzakkir-profile.md in context for profile awareness
- Score threshold: 0.3, limit: 5 results per query

**Langfuse Tracing:**
- Branch off Extract Response node
- Trace name: gilgamesh-chat
- Logs: input (user message), output (response content)
- Metadata: routedTo (model), ragUsed (boolean), commandType (chat), chatId (session)
- Fires async after response generated

**Personal Knowledge Pipeline:**
- Branch off Extract Response node
- Async fire-and-forget to Da Vinci — Personal Knowledge gateway
- Sends messages >20 characters (excluding commands starting with /)
- Uses internal VLAN 30 URL: http://192.168.30.211:5678/webhook/davinci-personal-knowledge
- 5s timeout for