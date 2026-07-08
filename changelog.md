# Changelog

### 2026-07-08 — Planning & Architecture Session — Chaldea Rename, Deck Sync Design, Agent Architecture Research
- Investigated and fixed false "hdd-backup-2 not mounted" Discord alert (Prometheus rule copy-paste bug: `MountpointMissing_hddbackup2` was checking `/mnt/hdd-backup-1` instead of `/mnt/hdd-backup-2` on CT 202); rule fully removed per decision rather than retained
- Renamed agents ecosystem from "Kuromoon" (overloaded with hardware name) to **Chaldea** (FGO-lore fit: the organization that houses and coordinates Servants); Kuromoon now refers only to physical homelab infrastructure
- Renamed **Gilgamesh → Jeanne Alter ("The Corrupted Ruler")** — propagation required across: bot identity, n8n system prompt node, Telegram username (@JhinGilgamesh_bot), all 8 documentation files; not yet executed
- Career context updated: September 2026 job-transition deadline dropped; homelab/Chaldea reframed as indefinite long-term project; user still applying for Cloud/DevOps/SysAdmin roles at slower pace
- Reviewed Nextcloud Deck (Homelab board, ID 4) directly for first time in months; confirmed real drift against GitHub docs (Phase Da Vinci S2 marked Done ~2 months ago; Phase 7E appears in two different card states simultaneously)
- Designed Da Vinci → Nextcloud Deck sync mechanism (9th pipeline step): hidden `sync-id` tags in card description footer; match-and-update existing cards silently; create new cards only for genuinely new items; stack routing via status keywords (Complete → Done, In Progress → In Progress)
- Deck sync scope: Homelab board only; trigger: every Da Vinci pipeline run (broadest option, chosen with explicit acknowledgment of board-noise risk); precondition: all ~30 existing cards must be manually backfilled with sync-id tags before automation goes live (human judgment required due to known conflicts)
- Deep-dive "agent anatomy" comparison: Gilgamesh/Jeanne Alter, Da Vinci, MERLIN, Midas vs. open-source frameworks (Hermes Agent, OpenClaw); selected specific components to adopt: MCP (tool execution/"hands"), Mem0 (memory), n8n AI Agent node (orchestration/"skeleton"), personality-file pattern (persona)
- Da Vinci confirmed as most architecturally complete agent in ecosystem: has real "hands" (GitHub push, WebDAV write, soon Deck API) and functions as ecosystem's long-term memory system; MERLIN/Midas having no LLM "brain" is intentional (deterministic threshold checks don't need one) — actual gap is no "hands" (report-only) and no memory (re-discovers same known issue daily instead of tracking prior report)
- EMIYA status conflict identified: agents.md lists as Planned / 0/4, but documented history of partial deployment (App Management, Universal Notifications, Langfuse phases marked complete elsewhere) requires audit to reconcile before scoping further work
- CrewAI selected as target framework for future multi-agent discussion protocol (supersedes earlier LangGraph decision); complexity-based trigger (quick topics live via Telegram, complex/research topics run in background); disagreement resolution: Jeanne Alter presents both sides, user arbitrates — explicitly not consensus-forced and not auto-decided by agent; reporting: summary-only by default, full transcript available on request; **explicitly deferred, research priority only**
- New agent concept approved at research stage: **Solomon** (overseer/growth agent, FGO-lore fit as Chaldea's administrator); weekly cron-triggered review of all agents' activity/logs/decisions; proposes fixes for user approval; deliberately human-in-the-loop, not autonomous self-modification
- Voice organ concept approved at research stage: **Whisper STT + local TTS** for Jeanne Alter; chosen over prioritizing vision capability first due to VRAM constraints (single RX 6700 XT 12GB cannot comfortably run vision model + main chat model concurrently)
- Identified two new ecosystem-wide gaps: web search quality (current Firecrawl implementation fires single query; target: iterative multi-query search closer to Gemini-style behavior) and universal time/date awareness (inject current date/time into every agent's system prompt via `{{date}}` pattern, applied ecosystem-wide instead of one gateway)
- Vision model deployment and live browser vision/control both explicitly deferred due to VRAM contention risk on current single-GPU hardware — not just priority call, a hardware-constraint call
- Newly surfaced issues from 2026-07-05 docs upload: GitHub `docs/` folder contains stale artifacts (changelog.md, troubleshoot.md) from old pre-8-file pipeline; duplicate `muzakkir-profile.md.md` in Nextcloud `04-personal/` (WebDAV write-race artifact); Jeanne Alter assistant messages not saving to `gilgamesh_conversations` Data Table (conversation memory broken); deferred network follow-ups from July 5 emergency migration still outstanding (Enshrouded UDP retest, DNS record updates)
- Phase 24.10 (Triggered Qdrant Re-indexing) confirmed already deployed May 25, 2026; remove from pending if cached copy still lists as upcoming
- Da Vinci Stage 2 (RAG System) confirmed complete; remove from Planned phases if still listed there
- Phase 7E three-way conflict identified: Deck board shows two different states across two cards, ROADMAP.md has duplicate listing from prior resolution — requires human audit to resolve, not automated fix
- New phases planned for future scoping: Chaldea rename propagation (multi-session), ecosystem-wide credential store migration (multi-session, all agents), Jeanne Alter architecture refactor with MCP/Mem0/personality file/native AI Agent node (multi-session), Jeanne Alter web search quality improvement (one session), universal time/date awareness (quick one-sitting fix), Deck sync manual backfill (one sitting, tedious but bounded)
- Research/deferred concepts: multi-agent discussion protocol (CrewAI-based, background complexity tier), Solomon overseer agent (weekly reviews, human-in-the-loop), shared MCP tool layer + shared Mem0 instance across all agents (consolidate duplicated architecture)
- Decision: Jeanne Alter architecture refactor uses DIY approach (not re-platforming onto OpenClaw or Hermes Agent runtime) but scoped around proven external components (MCP, Mem0, n8n AI Agent node) for resume/portfolio value
- Decision: Nextcloud Deck falls under Da Vinci's existing scope — same "sole writer" discipline as Obsidian (Jeanne Alter or other agents do not write to Deck directly, only report through Da Vinci)
- Decision: Deck sync matching mechanism uses hidden `sync-id` tags in card description footer (human-visible if inspected, but structured for reliable parsing by Da Vinci) rather than relying on title/description fuzzy matching
- Decision: Deck card stack routing is status-keyword based rather than defaulting all new cards to Backlog
- Decision: Backfill approach for existing Deck cards is full manual tagging (user responsibility) rather than automated "adopt" pass, due to known conflicts requiring human judgment
- Errors resolved: hdd-backup-2 Prometheus rule copy-paste bug (removed rule entirely); Nextcloud Deck drift audit completed; EMIYA status conflict surfaced; Phase 7E three-way conflict surfaced; agent anatomy audit completed with framework component selection finalized
- Action items: Manually backfill sync-id tags on ~30 existing Homelab Deck cards; full documentation audit (ROADMAP staleness, EMIYA status, Phase 7E conflicts); MERLIN Cloudflare token expiry approaching (Jul 14); investigate Qdrant `obsidian_knowledge` degraded state; retest Enshrouded UDP and update DNS records; clean up GitHub `docs/` stale artifacts; resolve duplicate `muzakkir-profile.md.md`; fix Jeanne Alter assistant message saving; begin Jeanne Alter rename (bot, prompt, Telegram, 8 docs); begin ecosystem credential store migration; hdd-backup-1 SATA fix still blocked on physical access

### 2026-07-05 — Gaming Platform Deployment + Emergency Network Migration (unnumbered, ad-hoc)
- Called TIME ISP and activated true bridge mode on HG8145B7N, permanently eliminating double-NAT topology
- pfSense WAN reconfigured from DHCP to PPPoE (credentials: muzakkir655@timebb); public IP changed from 202.184.35.79 to 202.184.101.136 as direct result of bridge mode
- ISP router's Wi-Fi and LAN ports now provide no internet access post-bridging (expected behavior of true bridge mode, not a fault)
- Deployed TP-Link EAP610/AX1800 access point as replacement Wi-Fi source after ISP router's own Wi-Fi became unavailable
- Root cause of AX1800 setup failure identified: TL-SG108E switch ports 7-8 were still on legacy default VLAN 1, never migrated to VLAN20_MAIN when VLAN architecture was originally built — this gap was already flagged in ROADMAP.md Phase 26 ("Legacy Network Cleanup")
- Fixed VLAN/PVID assignment for TL-SG108E ports 7-8: both moved from VLAN 1 to VLAN 20 (untagged)
- AX1800 access point (SSID `A21-22A`, password `Muzakkir_2110`) confirmed fully working after VLAN fix; clients receive real 192.168.20.x DHCP leases
- Enshrouded dedicated server (CT 306) research, deployment, and configuration completed; server password auto-generated as `4TGU-MHE` (not yet corrected to blank)
- Steam relay/friend-invite networking confirmed as actual mechanism allowing external friends to connect, masking that direct UDP port forwarding was broken (caused by double NAT)
- Minecraft server logs used as evidence that TCP forwarding through double-NAT worked while UDP did not
- Enshrouded external connectivity not yet retested post-bridge-mode migration — pending item to verify UDP forwarding now works with single-NAT topology
- Cloudflare DNS records for `enshrouded.najhin-gaming.com`, `mc.najhin-gaming.com`, `terraria.najhin-gaming.com` still point to old IP 202.184.35.79 and require update to 202.184.101.136
- Decision: Activate true bridge mode on ISP router rather than continue troubleshooting double-NAT workarounds — permanently eliminates category of networking friction, not just for Enshrouded but for any future inbound service
- Decision: Deploy AX1800 access point immediately post-bridging rather than defer — household/friend Wi-Fi access needed immediately after ISP router's services disabled
- Decision: Use switch's Easy Smart Configuration Utility + temporary static IP to fix VLAN assignments rather than permanently renumber switch's management IP — kept as separate Phase 26 cleanup item
- Decision: Defer Enshrouded server password fix (carried over) — server in active/stable use, low-priority cosmetic issue
- Errors resolved: double NAT confirmed via TCP/UDP evidence (Minecraft logs); bridge mode activation; PPPoE reconfiguration; VLAN port assignment fix; temporary static IP method for switch management access
- Action items: Update Cloudflare DNS records to 202.184.101.136; retest Enshrouded UDP connectivity post-bridge-mode; clean up dead ISP router port forwards; verify pfSense NAT rules still correct; fix Enshrouded password; confirm AX1800 Operation Mode; finish Phase 26 switch management IP migration; consider static IP request to TIME; test Enshrouded with full friend group

### 2026-05-25 — Phase 24.10 (Triggered Qdrant Re-indexing) + Web Search (Gilgamesh)
- Memory audit: Claude memory vs project files — identified 7 discrepancies, all corrected
- Phase 24.10 completed: triggered Qdrant re-indexing after Da Vinci writes to muzakkir-profile.md
- Phase 52 (DOCP Memory Optimization) confirmed complete — done independently by Muzakkir
- Da Vinci Stage 2 clarification: RAG foundation is complete (Phase 24.9), conversation buffer memory already implemented in Format Messages (last 15 rows from gilgamesh_conversations). Phase 7E planned ("20+ message conversations") is distinct from Stage 2.
- Gilgamesh feature brainstorm: full wishlist compiled (16 features across High/Medium/Long-term categories), web search added from internet research
- Web Search feature built and deployed: Firecrawl API wired into Gilgamesh — Gil can now answer real-time questions
- Web Search architecture: keyword detection If node (search, latest, current, today, weather, news, price, who is, when is, what is, how much, find, look up, cari, semak, berapa) → Firecrawl /search → inject results into user message context → force-route to Haiku (qwen3:14b ignores injected context regardless of instruction strength)
- Search query cost: ~$0.001-0.002 per query at Haiku pricing (acceptable for personal use volume)
- Firecrawl integration: community node @mendable/n8n-nodes-firecrawl v2.1.1, free tier active (2 credits per search, no credit card required)
- Documentation audit finding: Knowledge Indexer actually indexes 10 folders (not 4 as documented), model is Claude Sonnet 4 in Format Messages (overridden by Route Model to qwen3:14b/Haiku)
- Corrected model routing: Format Messages sets model to claude-sonnet-4-20250514 as default, but Route Model overrides: primary = qwen3:14b (Ollama), fallback = claude-haiku-4-5-20251001. Search queries force-route to Haiku.
- Corrected Knowledge Indexer folder list: 10 folders indexed (00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 07-daily, 08-agents, 09-people, 10-projects, AI-Stuff/Homelab/homelab-infrastructure)
- Da Vinci — Knowledge Indexer: added Webhook trigger (POST /davinci-reindex-personal) for partial reindex. Trigger type detection: body.reindexType === 'partial' → indexes only 04-personal/ (~1s). Schedule trigger (3am UTC) → full 10-folder rebuild (~21s).
- Phase 24.10 deployed: Da Vinci — Personal Knowledge Gateway fires HTTP POST to /davinci-reindex-personal webhook after successful Obsidian write. Reindex completes in ~990ms.
- Conversation buffer memory: already operational — Format Messages fetches last 15 rows from gilgamesh_conversations as history
- Extract Response bug fixed: Call Claude API wraps response in data.response object. Fixed with typeof check: if Ollama (typeof data.response === 'string') else uses claudeData = data.response (object) or data (direct)
- Firecrawl output structure: results nested under data.web array (item.data?.web). Inject Search Results reads web array, not flat items.
- If node "Needs Web Search?" fixed: return `.toString()` and compare against string "true" in condition (was comparing boolean true to string)
- Send a text message: Parse Mode must NOT be set (leave blank). Extract Response strips all markdown before output. Content always string, never JSON object.
- ROADMAP bug noted: Phase 7E appears in both Completed and Planned tables — remove from Planned
- Decision: Search queries force-route to Haiku, not qwen3:14b. Reason: qwen3:14b ignores injected search context regardless of prompt strength; consistently responds "I don't have real-time data" even with actual results in context. Haiku correctly follows injected Firecrawl results.
- Decision: Firecrawl chosen over Brave Search API, Serper, SearXNG. Reason: community node already installed (v2.1.1), free tier without credit card, no new containers needed.
- Decision: Web search results injected into last user message (not system prompt). Reason: local models ignore system additions but respond better to user-turn context.
- Decision: Da Vinci Stage 2 considered complete — no separate deployment needed. RAG pipeline fully operational via Phase 24.9. Remaining work (Phase 7E conversation buffer) already working in Format Messages.
- Decision: Phase 7E duplicate resolved — keep in Completed only. Remove from Planned (conversation buffer already working in Format Messages).
- Decision: Gilgamesh feature wishlist documented as 16 planned features across 3 priority tiers (High/Medium/Long-term). To be tracked in ROADMAP as individual planned phases.
- Action items: Test Gil with varied search queries (news, prices, exchange rates); test non-search queries to confirm Ollama routing works; fix assistant messages not appearing in conversation history (Save Assistant Message may not be firing); decide next session priority (Midas v2 vs Smart Morning Briefing vs Guardian)

### 2026-05-22 — Phase 24.11 — Documentation Audit & Corrections (No deployment — corrections only)
- Full audit of all 8 Da Vinci documentation files against full conversation history
- Found 11 discrepancies across 4 files: ROADMAP.md, current-state.md, agents.md, AI-CONTEXT.md
- Root cause identified: Da Vinci hallucinated hardware specs (EPYC 5645/256GB/RTX 4070) and folder names in current-state.md and agents.md; ROADMAP.md stale since May 10 (Homepage retirement decision never propagated); MERLIN/Midas status never updated in agents.md after April 27 deployment
- Corrected AI-CONTEXT.md: removed llama3.2:latest from installed models list (removed May 21 due to hallucination)
- Corrected ROADMAP.md: removed phases 22.8C, 22.8D, 22.8E, 22.15, 22.16 from In Progress and Planned tables (archived May 10 when Homepage retired, replaced by Pulse Dashboard CT 208); updated Guardian dependency from Phase 22.8C to Phase 24.9; reorganized Recommended Next Session Order to prioritize Phase 24.10 (Triggered Qdrant Re-indexing), Da Vinci Stage 2, Guardian, then Phase 24.2
- Corrected current-state.md Hardware Infrastructure: Proxmox host corrected to Ryzen 5 5600X, 128GB DDR4-3200, RX 6700 XT 12GB (NOT EPYC 5645/256GB/RTX 4070 — hallucinated specs); VM 400 corrected to 86GB disk (expanded May 21), RX 6700 XT GPU with ROCm backend; corrected Storage section (CT 220 for Nextcloud, NOT CT 215 which does not exist); corrected Knowledge Indexer indexed folders (04-personal/, 08-agents/, 09-people/, 10-projects/)
- Corrected agents.md: MERLIN updated from Planned/0/4 to Active Partial/2/4 (deployed April 27, 2026); Midas updated from Planned/0/4 to Active Partial/2/4 (deployed April 27, 2026); corrected Knowledge Indexer indexed folders and flagged previous lists (01-05, 06-07, 11-13) as hallucinated
- Identified Knowledge Indexer folder list inconsistency: three different lists exist across agents.md, current-state.md, service-catalog.md; none fully match actual vault structure; requires manual verification against n8n Knowledge Indexer workflow node
- Decision: Hardware specs in documentation must use explicit REPLACE SECTION in session summaries to prevent drift from actual infrastructure
- Decision: Any phase retirement must include explicit REPLACE SECTION in ROADMAP.md section of session summary to prevent stale entries propagating
- Decision: Da Vinci should not extrapolate hardware from hypothetical "dream homelab" discussions without clear context marking
- Action items: Drop corrected files into Nextcloud staging-inbox for Da Vinci processing; manually verify Knowledge Indexer node in n8n for exact folder list; re-store truncated Cloudflare API token in Vault (blocks MERLIN SSL check); next session: Phase 24.10 (Triggered Qdrant Re-indexing)

### 2026-05-22 — Phase 24.9 — Personal Knowledge System (Gil → Da Vinci → Obsidian)
- Langfuse UI trace list confirmed working (1-hour analytics delay resolved overnight); Da Vinci traces visible in list with 8 generations each; Gilgamesh traces visible with input/output populated
- Wired Langfuse into Gilgamesh — traces fire async from Extract Response node (trace name: gilgamesh-chat); input = user message, output = response content; metadata: routedTo, ragUsed, commandType, chatId
- Fixed Gilgamesh system prompt (qwen3:14b confused "Gil" as name query) — now explicitly states "Your name is Gilgamesh. Users will call you Gil as a nickname. Never ask the user for their name when they greet you as Gil. Your master is Muzakkir."
- Built Da Vinci Personal Knowledge gateway (new n8n workflow) — webhook POST /davinci-personal-knowledge; flow: Filter & Assess → Fetch Current Profile (WebDAV GET with 404 fallback to template) → Claude Haiku — Assess & Merge → Worth Saving? → Push to Obsidian (WebDAV PUT) → Log Cost; max_tokens 4000
- Wired Gil to send personal facts to Da Vinci Personal Knowledge gateway (async fire-and-forget branch off Extract Response node; fires on all messages >20 chars that don't start with /); uses internal URL http://192.168.30.211:5678/webhook/davinci-personal-knowledge
- Da Vinci quality filter: stores durable personal facts (e.g., "prefers dark mode"), SKIPs one-time events and conversational noise (e.g., "I slept at 12am tonight"); SKIP detection uses startsWith not strict equality
- Created muzakkir-profile.md in Obsidian (04-personal/) managed by Da Vinci — Personal Knowledge gateway; currently contains dark mode preference
- Updated Knowledge Indexer: added 04-personal/, fixed 08-agents (was 08-projects), 09-people (was 09-meetings), 10-projects (was 10-reference); added empty file filter (If node) to prevent crash on empty downloads; now indexes 90 files (was ~70), 1,736 Qdrant chunks (was 1,567)
- Gil successfully recalling profile info via RAG (knows name Muzakkir, dark mode preference)
- Fixed SKIP detection bug — Da Vinci returns "SKIP\n\nReasoning" but code checked === 'SKIP'; fixed with startsWith('SKIP')
- Fixed date placeholder — {{date}} now replaced with real MYT date (YYYY-MM-DD) in Claude API code node before sending to Claude, not relying on Claude to replace it
- Fixed Fetch Current Profile 404 handling — replaced HTTP Request node with Code node that falls back to bootstrap template when muzakkir-profile.md not found on WebDAV
- Fixed "Assignment to constant variable" error — changed const currentProfile to let for date replacement
- All agents now write to Obsidian via Da Vinci — Personal Knowledge gateway (not directly); EMIYA, Midas, and future agents all route through Da Vinci; only Gil currently wired
- 04-personal/ folder now included in Qdrant Knowledge Indexer (was excluded for privacy — decision reversed since everything is internal VLAN 30)
- Decision: All agents route Obsidian writes through Da Vinci — Personal Knowledge gateway; Gil reads directly via RAG but only writes through Da Vinci (consistent formatting, no file conflicts, Da Vinci as quality gate)
- Decision: Gil uses internal URL for Da Vinci gateway instead of external Cloudflare (faster, no external routing, both on VLAN 30)
- Action items: Delete muzakkir-profile.md.md conflict artifact from Nextcloud; build triggered Qdrant re-indexing after Da Vinci writes; re-tell Gil "I work best late at night" (lost due to SKIP bug, now fixed); wire Langfuse into MERLIN and Midas

### 2026-05-21 — Phase 24.8 — Langf