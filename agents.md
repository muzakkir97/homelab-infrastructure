# Agents

## Da Vinci
**Servant Class:** Caster  
**Ascension Stage:** 4/4  
**True Name:** Leonardo da Vinci  

### Overview
Documentation librarian and infrastructure chronicler. Maintains the homelab's living documentation through automated session-driven updates. Operates as the primary interface between raw session data and structured knowledge artifacts. Manages personal knowledge gateway for all agents writing to Obsidian. Sole writer to Nextcloud Deck (kanban board) — other agents report through Da Vinci, never write directly.

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
- Nextcloud Deck synchronization (9th pipeline step, pending manual card backfill)

### Active Workflows

#### Da Vinci Update Pipeline
**Status:** Operational (Rebuilt May 19, 2026 | Expanded to 8 Files May 21, 2026 | Langfuse Wired May 21, 2026 | Verified May 21, 2026 | Emergency Network Migration July 5, 2026 | Deck Sync Design July 8, 2026 | Documentation Audit July 9, 2026 | Monitoring Bug Fix July 14, 2026 | Palworld Troubleshooting July 17, 2026)  
**Type:** 8 sequential Haiku API calls with immediate cost logging and Langfuse observability, plus planned 9th step (Nextcloud Deck sync)  
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

**Planned 9th Step: Nextcloud Deck Sync**
**Status:** Design Complete (July 8, 2026) | Implementation Blocked on Manual Backfill  
**Trigger:** Every pipeline run — items written across any of the 8 files get reflected as Deck cards  
**Matching Mechanism:** Hidden `sync-id` tag in card description footer (e.g. `---\n🔖 sync-id: phase-24.10`), human-visible but structured for parsing  
**Update Behavior:** if a matching sync-id exists, update that card silently (title/description/stack); if no match, create a new card with a new sync-id  
**Stack Routing:** status-keyword based (e.g. "Complete" → Done, "In Progress" → In Progress), not a fixed default stack  
**Scope:** Homelab board (ID 4) only, for now — Career/Personal boards not yet in scope  
**API Base:** http://192.168.30.220/index.php/apps/deck/api/v1.0  
**Precondition:** All ~30 existing cards on the Homelab board must be manually backfilled with sync-id tags by the user before this automation goes live (chosen deliberately over an automated "adopt" pass, due to known conflicts on the board)  

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
- **[Planned] Deck Sync (extract changed items, match sync-ids, create/update cards in Homelab board)**

**Node Count:** 36 total (current) | 37+ total (after Deck sync implementation)  
- 8 sequential Haiku API calls (separate, per-file)
- 9 Log Cost nodes (1 pre-fetch, 8 immediate post-API)
- 8 Parse nodes
- Fetch GitHub Files
- Push to GitHub
- Trigger Reindex (webhook POST to Knowledge Indexer)
- Langfuse — Da Vinci (observability node)
- Push to Nextcloud
- Send Confirmation
- **[Planned] Deck Sync node(s) — TBD architecture after backfill completes**

**Cost Pattern:**
- 8 cost log rows per pipeline execution (1 per API call, fires immediately after)
- Expected cost per run: ~$0.14-0.16 USD
- Logs fire immediately after each Haiku API call, before parse
- Deck sync will add minimal cost (Nextcloud API calls, no LLM)

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
- Deck sync design complete (July 8, 2026); implementation blocked on manual sync-id backfill of ~30 existing Homelab board cards (deliberately chosen over automated matching due to known board conflicts)
- Runtime: ~5 minutes per session update cycle (includes ~1s triggered reindex, Deck sync TBD)
- Trace verification confirmed May 21, 2026: da-vinci-update traces appear in ClickHouse with 8 visible generations per trace, accessible via public API and UI list
- agents.md now handles 6 full agent sections (Da Vinci, MERLIN, Midas, EMIYA, Cu Chulainn, Scathach) plus Agent Design Principles section (July 9, 2026); 8,000 token budget is now handling larger volume — requires close monitoring this run to confirm no truncation

#### Da Vinci Personal Knowledge Gateway
**Status:** Operational (Deployed May 22, 2026 | Verified May 22, 2026 | Integrated Phase 24.10 May 25, 2026)  
**Type:** Separate n8n workflow (Da Vinci — Personal Knowledge)  
**Trigger:** Webhook POST /davinci-personal-knowledge (CT 211 internal: http://192.168.30.211:5678/webhook/davinci-personal-knowledge)  

**Purpose:**
Receives personal facts from all agents (currently Jeanne Alter, future EMIYA/Midas). Assesses facts via Claude Haiku. Merges with existing profile. Writes to Obsidian muzakkir-profile.md. Triggers Knowledge Indexer reindex. Logs costs to gilgamesh_costs.

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
- Jeanne Alter: sends all messages >20 chars (excluding commands starting with /) via async fire-and-forget to personal gateway
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
- Known issue: duplicate muzakkir-profile.md.md file exists in Nextcloud 04-personal/ (WebDAV write-race artifact) — requires cleanup

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
- 04-personal/ now included per decision to include all internal VLAN 30 data in RAG (Jeanne Alter needs to read profile)
- Scheduled full rebuild at 3am UTC for off-peak indexing; triggered partial reindex active since Phase 24.10 (May 25, 2026)
- Filters by file extension (.md only) at download stage
- Webhook trigger detects partial reindex via body.reindexType === 'partial'; Define Folders node conditionally indexes 04-personal/ only (fast path) vs all 10 folders (slow path)
- Known issue: Qdrant obsidian_knowledge collection in degraded/empty state (needs investigation — flagged July 8, 2026)

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
**Palworld Troubleshooting Session (July 17, 2026 — Session Palworld PublicIP & Settings)**
- External friend unable to connect to Palworld server (CT 307) — root cause: PalWorldSettings.ini PublicIP was set to container's internal LAN IP (192.168.30.219) instead of public WAN IP
- Discovered Pelican egg "Public IP" variable was not user-editable by default; enabled via Admin → Eggs → Palworld → Egg Variables (set both User Editable and User Viewable permissions)
- Pelican's PalworldServerConfigParser auto-populates PublicIP from local container allocation (192.168.30.219) on every boot when the egg's Public IP variable is empty — confirmed source of silent ini reverts before permissions were enabled
- PalCaptureRate setting change had no effect in-game despite correct-looking file contents; diagnosed corrupted PalWorldSettings.ini (13 lines instead of 2) caused by Pelican's web-based file editor introducing hard line breaks into OptionSettings=(…) block
- Palworld requires OptionSettings block to remain a single unbroken line; broken line causes server to silently ignore ENTIRE OptionSettings block (not just edited field) and fall back to defaults — confirmed root cause of PalCaptureRate not applying
- Fixed ini corruption via `pct exec` commands from Proxmox host, bypassing Pelican's web editor
- Verified PalCaptureRate=100 change took effect in-game after line-break fix; subsequently reverted to default (1.000000)
- Established safe editing method: PalWorldSettings.ini should be edited via `pct exec 307 -- sed` from Proxmox host, never via Pelican web file editor, to avoid line-break corruption
- Attempted file download via Pelican panel (Files tab → Archive → Download) failed with "resource not found" 404, reproducible on internal IP (ruling out Cloudflare); root cause not identified (suspected Wings/node FQDN misconfiguration) — not investigated further due to time constraints
- Established reliable backup method: `pct exec <CTID> -- tar -czf /tmp/backup.tar.gz -C <path> <folder>` + `pct pull <CTID> /tmp/backup.tar.gz <destination>` — bypasses Wings/panel entirely
- Created Palworld SaveGames backup: /mnt/hdd-backup-1/palworld-backup-20260716/palworld-save-backup.tar.gz (22M, verified contents)
- Confirmed no WorldOption.sav exists in current world save (CT 307 using Palworld version that does not lock settings into save file)
- Diagnosed in-game stutter with 3 concurrent players: NOT a Palworld resource allocation issue; caused by nightly vzdump backup job (sequential, all-container) running concurrently with active gameplay (CPU/disk I/O contention observed during backup of CT 222/CT 223)
- Discussed consolidating Palworld/Terraria restart schedules with nightly vzdump backup window into single off-peak maintenance block
- WAN IP instability confirmed: 3 changes in ~1 week (202.184.35.79 → 202.184.101.136 → 202.184.103.49 → 202.184.109.124 during session); manual PublicIP update required on each change
- Elevated DDNS automation priority on roadmap given repeated WAN IP instability
- Updated AI-CONTEXT.md: Palworld PublicIP corrected to 202.184.109.124, new known issues for Pelican ini editing and file download documented, safe editing method established
- Updated current-state.md: CT 307 PublicIP updated, PalCaptureRate reverted to default, SaveGames backup location recorded

**Monitoring Bug Fix (July 14, 2026 — Session Monitoring Bug Fix)**
- Resolved 8-day stale `MountpointMissing_hddbackup1` Alertmanager alert by fixing node_exporter configuration
- Root cause: Debian `prometheus-node-exporter` package ships with default `--collector.filesystem.mount-points-exclude` regex containing `mnt`, making all `/mnt/*` mountpoints (hdd-backup-1, hdd-backup-2, ssd-storage, pve/kinmoon-smb) invisible to Prometheus since at least May 16, 2026
- Fixed by removing `mnt` from ARGS exclude regex in `/etc/default/prometheus-node-exporter` and restarting `prometheus-node-exporter.service`
- Confirmed fix: metrics now reporting for all three /mnt mountpoints; Alertmanager alert auto-resolved
- Storage clarification: hdd-backup-1 is primary live Nextcloud storage (bind-mounted data directory), not a backup copy; Kinmoon NAS receives nightly rsync copy only (one-way, time-lagged at 03:00)
- Action item: verify which storage target Proxmox vzdump jobs are actually configured against (local hdd-backup-* mount vs kinmoon-smb SMB); check Grafana dashboards for panels that may have been silently empty since ~May 16 due to same exclusion bug

**Email Management Pipeline Design (July 9, 2026 — Session Email Architecture)**
- Decided email account access belongs to Jeanne Alter (PA/user-facing interface), not Da Vinci (documentation-only writer)
- Audited personal accounts: 4 personal accounts scoped for pipeline (muzakkir.kholil06@gmail.com, muzakkirkholil97@icloud.com, hyperjhin00@gmail.com, business.najhin@gmail.com)
- Excluded from automation: work accounts (muzakkir.rahimi@f-secure.com, work26@gmail.com — company policy risk), NSFW account (adamlil1997@gmail.com — separate)
- Architecture: 4× per-account trigger workflows (3× Gmail OAuth2 native, 1× IMAP for iCloud) → shared Email Classifier sub-workflow → Telegram notify + staging store → permanent-category facts (bills/payments/subscriptions) to Da Vinci Personal Knowledge gateway
- Behavior tier: Read + Notify (proactive extraction + alert filtering, not full inbox ownership or passive-only)
- Categorization: fixed categories + sender whitelist combined (rejects LLM-only judgment, accepts new senders from whitelist)
- Permanent vs staged facts: bills/payments/subscriptions only become second-brain entries; all other extracted content held in staging store (~7 day auto-clear)
- Model routing: qwen3:14b primary, Claude Haiku API fallback (consistent with Jeanne Alter's existing pattern, not stricter local-only)
- Schedule: 3x/day (morning/afternoon/evening, exact times TBD at build)
- Credentials: Gmail OAuth2 (n8n native) + iCloud IMAP app-specific password in n8n credential store (first concrete ecosystem-wide credential store migration step)
- Status: Design Complete, 0/6 rollout steps implemented

**Documentation Completion Session (July 9, 2026 — Session 2 agents.md Completion)**
- Added Agent Design Principles section (establishing funnel/non-funnel/hybrid classification pattern ecosystem-wide)
- Added full MERLIN section (Caster, 2/4, funnel agent — sources from Uptime Kuma for SSL cert tracking; urgent July 14 SSL expiry flagged; design decision made to migrate from broken Cloudflare API integration to Uptime Kuma native monitoring)
- Added full Midas section (Caster, 2/4, funnel agent — sources from Langfuse Metrics API for Haiku/Claude costs; design decision made to migrate from duplicate gilgamesh_costs Data Table to Langfuse as single source of truth)
- Added full EMIYA section (Archer, partial 3/8 — corrects prior status conflict; documents 8 sub-phases (24.1–24.8), 3 complete (24.1/24.7/24.8 as infrastructure deployments, not yet unified as running agent); notes planned dependencies Firefly III, ntfy, Langfuse, Alertmanager)
- Added full Cu Chulainn section (Lancer, 0/4, funnel agent — renamed from Guardian May 16, 2026; no implementation started; overlaps with EMIYA's Alert Translation on Alertmanager source)
- Added full Scathach section (Lancer, 0/4, not a funnel — career research/reasoning generative work; build priority 1st; LangGraph evaluation sequenced behind Phase 24.12 Jeanne Alter refactor to n8n AI Agent node)
- Updated Da Vinci Known Limitations: restored agents.md structural completeness note, flagged that EMIYA and Cu Chulainn are documented concepts but not yet unified running workflows

**Documentation Audit — Cross-Session Gap Analysis (July 9, 2026 — Session Documentation Audit)**
- Performed full audit of AI-CONTEXT.md, ROADMAP.md, decisions.md, changelog.md, agents.md, current-state.md, and service-catalog.md against conversation history dating back to January 2026
- Identified seven items discussed in past sessions never made it into project documentation files
- Confirmed agents.md structural gap: missing full sections for MERLIN, Midas, EMIYA, Scathach, and Cu Chulainn despite being listed in AI-CONTEXT.md agent table and decisions.md — now completed July 9, 2026
- Confirmed Cu Chulainn rename (from Guardian, renamed May 16, 2026) has not propagated — all references still show "Guardian" in agents.md, AI-CONTEXT.md agent table, decisions.md — now completed July 9, 2026
- Confirmed Scathach (Lancer-class career growth/research agent, 1st build priority) documented informally but missing from agents.md proper sections — now completed July 9, 2026
- Confirmed Nightingale (health pipeline agent concept, noted May 17, 2026) has no documentation — deferred indefinitely, not prioritized
- Confirmed MERLIN Cloudflare SSL expiry check hardcoded to July 14, 2026 remains unresolved (now 5 days out, carried from July 8 session) — design decision made to migrate to Uptime Kuma monitoring
- New content added to AI-CONTEXT: Interest-Capture Loop design concept (July 7 session), Four Blind Spots gap analysis (July 7 session), domain correction (two domains: najhin-gaming.com permanent for game servers; muzakkir.tech professional/portfolio domain)
- New Phase 27 identified: Domain Migration & Infrastructure Audit (27.1 audit, 27.2 migration) — status Planned, Cloudflare zone setup for muzakkir.tech directed to begin July 1 but completion unconfirmed, requires verification next infrastructure session

**Planning & Architecture Session (July 8, 2026 — Session Planning Phase)**
- Ecosystem renamed: "Kuromoon" (physical hardware/homelab only) vs "Chaldea" (agents ecosystem) — clarified going forward
- Gilgamesh renamed to Jeanne Alter ("The Corrupted Ruler") — full propagation pending (bot identity, system prompt, Telegram username, all 8 docs)
- Da Vinci's scope formally extended to include Nextcloud Deck — same "sole writer" discipline applies (no direct agent writes)
- Designed Da Vinci → Nextcloud Deck sync mechanism (9th pipeline step): match-and-update cards via hidden sync-id tags, status-keyword stack routing, Homelab board only
- Deck sync implementation blocked on manual backfill of ~30 existing Homelab board cards with sync-id tags (deliberately chosen over automated matching)
- Full agent anatomy audit performed: Da Vinci most architecturally complete (real "hands": GitHub, WebDAV, Deck), MERLIN/Midas gap is no "hands" (report-only) and no memory, Jeanne Alter needs refactor (MCP tools, Mem0 memory, personality file, AI Agent node orchestration)
- Career deadline dropped: September 2026 job transition no longer in scope; Chaldea reframed as indefinite long-term project
- New research-stage concepts: Solomon (overseer/growth agent), voice organ (Whisper STT + local TTS), universal time/date awareness ecosystem-wide
- CrewAI selected as target framework for future multi-agent discussion protocol (role-based model, human arbitration on disagreements, not consensus-forced)
- Credential store migration identified as ecosystem-wide priority (move from hardcoded n8n values to n8n credential store)
- Known board conflicts surfaced: Phase 7E appears in two Deck states + ROADMAP.md duplicate; Phase Da Vinci S2 RAG System marked Done on Deck (~2 months ago) but stale in docs; EMIYA status conflict across agents.md (Planned) vs historical completion records (partial deployment already happened)

**Emergency Network Migration (July 5, 2026 — Session Ad-Hoc Phase)**
- Called TIME ISP and activated true bridge mode on Huawei HG8145B7N, eliminating double-NAT topology permanently
- pfSense WAN reconfigured from DHCP to PPPoE (pppoe0 interface, credentials: muzakkir655@timebb via TIME Self Care portal)
- Public IP changed: 202.184.35.79 → 202.184.101.136 (stable IP assigned directly to pfSense WAN post-bridge)
- ISP router's Wi-Fi and LAN ports permanently non-functional post-bridging (expected behavior, not a fault)
- Deployed TP-Link AX1800 access point (SSID `A21-22A`, Password `Muzakkir_2110`) as replacement Wi-Fi source
- TL-SG108E switch ports 7-8 VLAN misconfiguration identified and fixed (were on legacy default VLAN 1, now correctly on VLAN20_MAIN/PVID 20)
- **Action Items Pending:** Update Cloudflare DNS A records (enshrouded.najhin-gaming.com, mc.najhin-gaming.com, terraria.najhin-gaming.com) from 202.184.35.79 to 202.184.101.136; Retest Enshrouded's UDP forwarding with eliminated double-NAT; Continue Phase 26 (Legacy Network Cleanup) for switch management IP migration

**Phase 24.10 — Triggered Qdrant Re-indexing (Complete — Deployed May 25, 2026)**
- Added webhook trigger (/davinci-reindex-personal) to Knowledge Indexer workflow
- Integrated Trigger Reindex node into Da Vinci Personal Knowledge Gateway (fires after Obsidian write)
- Integrated Trigger Reindex node into Da Vinci Update Pipeline (fires after Push to GitHub)
- Partial reindex path active: When webhook body contains reindexType: 'partial', Knowledge Indexer indexes only 04-personal/ folder (~1s)
- Full rebuild path (3am UTC schedule) unchanged: indexes all 10 folders (~21s)
- Profile updates now immediately reflected in Qdrant obsidian_knowledge collection
- Node count increased from 35 to 36 (added Trigger Reindex node)