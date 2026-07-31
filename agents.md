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
**Status:** Operational (Rebuilt May 19, 2026 | Expanded to 8 Files May 21, 2026 | Langfuse Wired May 21, 2026 | Verified May 21, 2026 | Emergency Network Migration July 5, 2026 | Deck Sync Design July 8, 2026 | Documentation Audit July 9, 2026 | Monitoring Bug Fix July 14, 2026 | Palworld Troubleshooting July 17, 2026 | Alertmanager iowait Investigation July 23, 2026 | Kinmoon RAID Incident Response July 24, 2026 | Kinmoon NAS Recovery & Storage Pool 1 Rebuild July 30-Aug 1, 2026)  
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

### Known Limitations
- Nextcloud Deck 9th pipeline step: implementation blocked pending manual sync-id backfill of ~30 existing Homelab board cards
- agents.md token budget (8,000) now handling 6 full agent sections plus Agent Design Principles — requires monitoring for truncation signals
- Observability: alerts on CT 205 (Alertmanager) cannot currently distinguish iowait from genuine CPU load; "CPU usage" alerts may be storage/I/O symptoms rather than compute-bound issues (diagnosed July 23, 2026; rule-level fix pending)

### Recent Updates
**Kinmoon NAS Storage Pool 1 Rebuild & Emergency Recovery (July 24, 2026 → July 30-Aug 1, 2026 — Multi-session Incident)**

**Session 1 Context (July 24, 2026):**
- Storage Pool 1 physical recovery: Hard Drive 1 replaced with 3TB Seagate IronWolf; confirmed detected by UGOS as healthy
- Two consecutive RAID 1 rebuild attempts failed (2026-07-24 01:32:29 and auto-resumed ~07:06)
- Root cause identified: UGREEN DXP2800 SATA link-speed compatibility issue — Hard Drive 2 (ST3000DM008) throws "failed command: WRITE FPDMA QUEUED" deterministically ~18-19 seconds after every boot cycle, misidentified as drive failure but actually a SATA 6.0Gbps timing issue
- Fix identified: force SATA link speed down to 3.0Gbps via kernel boot parameter `libata.force=3.0Gbps` in GRUB config (deferred pending emergency backup completion)

**Session 2 Context (July 30-Aug 1, 2026 — Current Session):**
- Emergency rsync backup launched July 24 (Kinmoon's proxmox-backups share → Kuromoon's `/mnt/hdd-backup-2/kinmoon-emergency-backup/`, 1.3TB) had died from signal interruption, verified incomplete on July 30
- Relaunched backup copy in tmux for session-disconnect resilience; multiple bad-sector read stalls on Hard Drive 2 during transfer required file-by-file exclusion strategy
- SSH access to Kinmoon resolved: original password stale, reset via UGOS web UI; correct casing is `Muzakkir` (capital M)
- Applied `libata.force=3.0Gbps` GRUB fix to both `/boot/EFI/debian/grub.cfg` (live config) and `/boot/EFI/debian/grub.am` (template for future regen); zero `WRITE FPDMA QUEUED` errors observed after applying
- Three consecutive failed RAID 1 rebuild/recovery attempts on July 30-31 (each interrupted at different points); fourth attempt also failed even with `storage_serv` stopped, root cause never definitively identified
- **Discovery of UGOS firmware bug:** `storage_serv`'s RAID monitor throws `strconv.Atoi: parsing "-": invalid syntax` during `RebuildFinished` event mishandling, causes kernel-level `md: recover interrupted` — mitigated (not fixed) by stopping `storage_serv` during rebuild attempts (known issue, worth reporting upstream to UGREEN)
- **Abandoned incremental rebuild troubleshooting** and instead destroyed Storage Pool 1 entirely and recreated from scratch (accepted permanent loss of July 12 LXC backup files for containers 202, 203, etc., already unreadable due to bad sectors)
- **Full pool/volume deletion, fresh RAID 1 creation, shared folder recreation, and full data restore from Kuromoon emergency backup copy** completed and verified matching (1.2TB both sides)

**Current state (as of 2026-07-31 01:10 UTC):**
- Storage Pool 1: RAID 1, both drives (Hard Drive 1 = Seagate IronWolf 3TB SN:W3FXXXZY, Hard Drive 2 = Seagate ST3000DM008 SN:Z505511Z), fresh array UUID `5bb187d0:b14f67a3:9f4d8ab9:16d079f8`, status **Normal/clean**, both drives `active sync`, zero spares
- Volume 1: ext4, 2.6TB, status Normal
- Shared folder `proxmox-backups` recreated with Read/Write permission for user Muzakkir, Recycle Bin enabled (Admin only)
- **libata.force=3.0Gbps GRUB fix confirmed working** — zero `WRITE FPDMA QUEUED` errors observed since applying, across fresh boot and full data restore
- `storage_serv` restarted and stable post-rebuild, 1+ day uptime, no errors in recent logs
- `backup-daily` vzdump job re-enabled (was disabled since July 24 during rebuild attempts); targets `kinmoon-smb` storage, schedule 02:00 daily, retention `keep-daily=7,keep-weekly=4`
- CIFS storage `kinmoon-smb` credential (Muzakkir password) updated in Proxmox after underlying account password was changed
- Emergency backup copy still present at `/mnt/hdd-backup-2/kinmoon-emergency-backup/` (1.2TB, pending deletion once array is trusted long-term)

**Errors & Root Causes (Session 2):**
- rsync backup copy died from SIGINT/SIGTERM/SIGHUP despite nohup — resolved by relaunching in tmux
- Repeated rsync stalls (D-state I/O wait) on specific files during backup — confirmed via UGOS event log as bad-sector critical medium error on Hard Drive 2 during READ operations; resolved by excluding affected files and continuing
- SSH login rejected for both `root` and `muzakkir` — root cause was stale password; resolved via UGOS web UI password reset; discovered correct username casing is `Muzakkir` (capital M)
- `NT_STATUS_LOGON_FAILURE` on Proxmox's `kinmoon-smb` CIFS storage after password reset — resolved via `pvesm set kinmoon-smb --username Muzakkir --password`
- UGOS `storage_serv` daemon bug: `mdadm --monitor` mishandles `RebuildFinished` event, throws `strconv.Atoi: parsing "-": invalid syntax`, causes kernel-level recovery interrupt (mitigated by stopping service during rebuilds)
- Cloudflare Error 522 on passwords.najhin-gaming.com — caused by broken CNAME DNS record with no tunnel binding; fixed by deleting and recreating route via tunnel's Published Application Routes UI
- Cloudflare Error 502 on passwords.najhin-gaming.com (after 522 fix) — caused by Service Type incorrectly saved as HTTPS instead of HTTP; fixed by correcting to HTTP
- Vaultwarden browser extension login failure (404 on `/identity/accounts/prelogin/password`) — known incompatibility between newer Bitwarden extension and older Vaultwarden server; fixed by updating Docker image to `vaultwarden/server:latest` and redeploying CT 214

**Decision Made:** Abandon RAID rebuild troubleshooting after four failed attempts and perform full destroy-and-recreate of Storage Pool 1, restoring from secured emergency backup, rather than continue diagnosing intermittent cause resistant to multiple mitigation attempts

**Decision Made:** Accept permanent loss of July 12 LXC backup files (containers 202, 203, others) and one CT211 file; rationale: already unreadable before pool wipe, newer backups exist for same containers, low-impact

**Decision Made:** Retain Kuromoon emergency backup copy (`/mnt/hdd-backup-2/kinmoon-emergency-backup/`, 1.2TB) as safety net until Kinmoon array is trusted long-term

**Decision Made:** Disable both HDD sleep settings in UGOS Hardware & Power (precaution, though did not resolve interruption issue; UGOS UI reported "Operation failed" when setting applied, likely due to `storage_serv` being stopped)

**Decision Made:** Abandon further root-cause investigation of fourth rebuild failure (occurred with `storage_serv` confirmed stopped throughout, ruling out software bug for that specific attempt)

**Cloudflare Tunnel & Vaultwarden Access:**

**Tunnel Configuration (newly documented detail):**
- Tunnel name: `homelab-tunnel`, connector runs as systemd service (`cloudflared`) **inside CT 220 (Nextcloud container)**, not on Proxmox host or in NPM
- Installed 2026-03-07 during Nextcloud initial external-access setup; additional service routes added to same tunnel/connector rather than deploying separate connectors

**Published Application Routes:**
| Hostname | Service |
|---|---|
| cloud.najhin-gaming.com | http://localhost:80 (same container as connector) |
| ntfy.najhin-gaming.com | http://192.168.30.222:2586 |
| finance.najhin-gaming.com | http://192.168.30.224:8080 |
| langfuse.najhin-gaming.com | http://192.168.30.223:3000 |
| passwords.najhin-gaming.com | http://192.168.30.214:8080 (**newly added this session, now operational**) |

**passwords.najhin-gaming.com Issue Resolution:**
- Root cause: stray broken CNAME DNS record pointing to bare root domain (`najhin-gaming.com`) instead of tunnel CNAME, causing Cloudflare Error 522 (connection timed out)
- Likely artifact from abandoned earlier setup attempt, unrelated to Kinmoon work
- Fixed by: deleting broken CNAME and re-adding route through tunnel's Published Application Routes UI (auto-created correct CNAME pointing to `.cfargotunnel.com` target)
- Secondary issue: route initially saved with Service Type HTTPS instead of HTTP, causing Error 502 (`tls: first record does not look like a TLS handshake`) since Vaultwarden serves plain HTTP internally on port 8080
- Fixed by: correcting Service Type to HTTP
- Domain-vs-protocol clarification: tunnel's internal "Service Type" setting governs how `cloudflared` talks to backend service on LAN (Vaultwarden = HTTP, no TLS) — unrelated to and does not need to match external URL scheme (always HTTPS via Cloudflare edge termination)

**CT 214 (password-vaultwarden) Update:**
- Docker image updated from stale version to `vaultwarden/server:latest` (pulled and redeployed 2026-08-01) to resolve login-flow incompatibility with current Bitwarden browser extension (extension calls `POST /identity/accounts/prelogin/password`, which old server returned 404 for)
- Container recreated via existing compose file at `/opt/vaultwarden/docker-compose.yml`; data volume untouched
- CT 220 (nextcloud) confirmed as host of `cloudflared` tunnel connector service

**Kingmoon Hard Drive 2 SMART Profile (Additional Detail):**
- Model: Seagate ST3000DM008 SN:Z505511Z (Barracuda 3TB, CMR)
- WRITE FPDMA QUEUED failure signature: deterministic ~18-19s after boot (not drive health issue per se — SMART data healthy, Reallocated Sector Count present value 99 vs threshold 10)
- Root cause attribution: known UGREEN DXP2800 SATA link-speed compatibility issue with certain drives (confirmed via UGREEN DACH community forum 30+ page thread; narrow signal timing tolerance at SATA 6.0Gbps); mitigated by `libata.force=3.0Gbps` kernel parameter
- This signature present intermittently in Kinmoon logs since March 2026 (multiple entries); raises possibility original Hard Drive 1's "failure" may have been accelerated by SATA instability rather than pure old-age wear
- Bad-sector READ errors also observed on Hard Drive 2 during emergency rsync backup (critical medium error per UGOS event log); likely pre-existing media degradation compounded by SATA stress

**Kinmoon Recycle Bin & Data Loss:**
- Old array #recycle folder: 1.3TB of deleted-backup churn (double-counting, already pruned by Proxmox retention policy)
- Action item: set sane recycle-bin retention policy on fresh `proxmox-backups` share to avoid repeating bloat
- Files permanently lost from old array: July 12 LXC backup files (containers 202, 203, others) and one CT211 file; previously unreadable due to bad sectors before pool wipe; newer backups exist for same containers; assessed as low-impact

**Action Items:**
- [ ] Delete `/mnt/hdd-backup-2/kinmoon-emergency-backup/` on Kuromoon once Kinmoon array is trusted long-term (not urgent)
- [ ] Report UGOS `storage_serv` `RebuildFinished`/`strconv.Atoi` bug to UGREEN
- [ ] Decide whether to pursue further recovery of July 12 LXC backups / CT211 file, or formally write off (currently informally accepted as lost)
- [ ] Set sane recycle-bin retention policy on fresh `proxmox-backups` share
- [ ] Verify `backup-daily` job's first scheduled run (2026-08-02 02:00) completes successfully against rebuilt array

**Alertmanager iowait Alert Investigation (July 23, 2026 — Session Infrastructure Audit)**
- CRITICAL "CPU usage 100%" alert on CT 205 (Alertmanager, 192.168.30.205:9100), 2026-07-20 03:46-03:47 AM, investigated and root-caused
- Root cause: Prometheus's CPU-usage alert rule sums all non-idle CPU time, which includes iowait — `top` confirmed 100.0% `%wa` with 0.0% `us`/`sy` (zero actual compute load, pure I/O wait)
- Correlated to backup-daily vzdump job running 02:00-~03:35 nightly, writing to kinmoon-smb CIFS network share sitting at 94% capacity, causing lingering I/O pressure past job completion (~03:46 alert vs ~03:35 job finish)
- Confirmed backup-daily vzdump job configuration: `/etc/pve/vzdump.cron`, schedule 02:00 daily, compress zstd, mode snapshot, prune-backups keep-daily=7/keep-weekly=4, storage kinmoon-smb, VMID list includes 201,202,203,204,205,206,207,208,211,213,214,220,221,222,223,302,303,304,305,400
- **CT 306 (Enshrouded) and CT 307 (Palworld) NOT included in