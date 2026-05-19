# 📝 Changelog

All notable changes to the homelab infrastructure project.

---

### [May 19, 2026] - Da Vinci Update Pipeline Rebuild: 3-Call Architecture

#### Changed
- **Da Vinci Update Pipeline** — Rebuilt with 3 separate Claude Haiku API calls instead of single batched call
- **Architecture:** Sequential calls for AI-CONTEXT.md, changelog.md, troubleshoot.md with individual cost logging
- **Cost logging timing** — Now fires immediately after each API call, before Parse Response and GitHub Push nodes
- **Pipeline flow:** Execute Workflow → Call 1 (AI-CONTEXT) + Log Cost → Call 2 (changelog) + Log Cost → Call 3 (troubleshoot) + Log Cost → Parse/Push

#### Fixed
- **Da Vinci staging inbox stuck file** — File failed validation repeatedly every 15 minutes, causing pipeline loop errors
- **Resolution:** Deleted problematic stuck file from staging-inbox, rebuilt pipeline with per-file API architecture
- **Cost logging verification:** Three separate cost rows now fire per pipeline execution (one per API call)

#### Technical
- **Haiku pricing corrected:** input_tokens $0.80/1M, output_tokens $4.00/1M (previously incorrect values)
- **Immediate cost tracking:** Log Cost node executes right after each HTTP Request completion
- **Expected cost per run:** ~$0.03-0.05 per complete 3-file pipeline execution (3× Haiku calls)

#### Verification
- End-to-end pipeline test passed with new 3-call architecture
- Cost logging confirmed firing immediately after each Claude API request
- GitHub document updates validated

#### Action Items
- [ ] Monitor gilgamesh_costs table for 3 new rows per pipeline run (verify cost logging fires 3x)
- [ ] Observe API usage over 24 hours to confirm total cost under target (~$0.05/run)
- [ ] If stable, mark "Da Vinci pipeline rebuild" complete in Pending Tasks

---

### [May 18, 2026] - qwen3.5 Test + Concurrency Fix

#### Added
- **qwen3.5:latest model** — Selected as primary model for Da Vinci documentation pipeline (6.6GB, 256K context, local inference)
- **Concurrency lock** — Check Running node queries n8n API before List Inbox node to prevent duplicate executions
- **Schedule adjustment** — Inbox Watcher changed from every 1 minute to every 15 minutes

#### Technical
- **Workflow order:** Schedule Trigger → Check Running → If → List Inbox → rest of pipeline
- **Concurrency check:** GET /api/v1/executions?status=running&workflowId={id} with X-N8N-API-KEY header
- **qwen3.5 configuration:** num_ctx 65536, temperature 0.3, local via Ollama at http://192.168.30.221:11434/api/chat
- **Cost logging:** qwen3.5 cost_usd always 0 (local inference), model logged as qwen3.5:latest

#### Fixed
- **Da Vinci concurrent executions** — Three Update Pipeline instances ran simultaneously because qwen3.5 local inference exceeded 1-minute watcher interval. File not yet archived when next watcher fired, triggering duplicate runs
- **Duplicate processing** — Resolved by increasing schedule interval + adding concurrency lock node

#### Verification
- End-to-end test passed with qwen3.5:latest
- GitHub document updates validated
- Cost tracking confirmed (local inference = $0)

#### Action Items
- [ ] Validate test file processed correctly by qwen3.5 — check GitHub for proper document updates
- [ ] If qwen3.5 output quality confirmed — update Gilgamesh routing from qwen3:14b to qwen3.5:latest
- [ ] Add validation guard to Parse Response node (reject placeholder outputs before GitHub push)
- [ ] Wire Langfuse into all agent workflows

---

### [May 18, 2026] - Da Vinci Cost Optimization & Pipeline Fix

#### Fixed
- **Da Vinci cost tracking** — Identified two-layer issue: Documentation Pipeline dropping usage data and Update Pipeline having no cost logging
- **Massive credit burn** — Traced $5.53/day usage to Da Vinci Update Pipeline running Sonnet on every session summary
- **sessionSummary property bug** — Claude API Merge was reading fileContent instead of sessionSummary from Fetch GitHub Files

#### Changed
- **Da Vinci Update Pipeline** — Switched from claude-sonnet-4-20250514 to claude-haiku-4-5-20251001 for 10x cost reduction
- **Cost logging** — Added Log Cost and Insert row nodes to Da Vinci Update Pipeline after Send Confirmation
- **max_tokens setting** — Confirmed 32000 required for three complete document outputs (16000 caused truncation)

#### Removed
- **Da Vinci Documentation Pipeline** — Deactivated orphaned workflow superseded by Update Pipeline + Inbox Watcher

#### Technical
- **Pipeline architecture clarified** — Inbox Watcher (5-min schedule) → Execute Workflow → Update Pipeline → Haiku API
- **Cost optimization achieved** — Expected reduction from ~$5.53/day to ~$0.55/day with Haiku switch
- **Property name fix** — Claude API Merge now reads $input.first().json.sessionSummary correctly
- **End-to-end test passed** — Session summary → GitHub update → cost logging working

#### Planning
- **Langfuse integration deferred** — Wait for cost stabilization before wiring into workflows
- **API credit monitoring** — Track burn rate over next few days to confirm under $10/month target

---

### [December 19, 2024] - Documentation Pipeline Test

#### Technical
- Tested documentation pipeline integration via Telegram /update command
- Verified end-to-end workflow functionality from Telegram → n8n → Claude → GitHub
- Documentation merging process confirmed operational
- No infrastructure changes made during this session

#### Verification
- Pipeline ready for regular use in future sessions
- No follow-up actions required from this test
- End-to-end workflow confirmed working correctly

---

### [December 19, 2024] - Documentation Pipeline Verification

#### Technical
- Tested documentation pipeline integration via Telegram /update command
- Verified end-to-end workflow functionality from Telegram → n8n → Claude → GitHub
- Documentation merging process confirmed fully operational
- No infrastructure changes made during this verification session

---

### [December 19, 2024] - Documentation Pipeline Test

#### Added
- Tested documentation pipeline integration via Telegram /update command
- Verified end-to-end workflow from Telegram → n8n → Claude → GitHub

#### Technical
- No infrastructure changes made during this session
- Documentation merging process confirmed operational

---

### [April 24, 2026] - Phase 38/39 Model Upgrade + Phase 41: Hybrid Routing (COMPLETE)

#### Added
- **Phase 41 Hybrid Routing** in Gilgamesh Telegram Agent with intelligent query classification
- **Ollama model upgrade** from qwen2.5:14b to qwen3:14b (9.3GB, optimized for 12GB VRAM)
- **Complex keyword detection** for routing: API, authentication, security, deployment, configuration, troubleshooting → Claude Sonnet
- **Word count routing** - queries >50 words automatically routed to Claude Sonnet for better handling
- **Fallback system** - Ollama primary → Haiku fallback if Ollama down → Sonnet for complex queries
- **Open WebUI updates** to latest version with improved JSON streaming support

#### Technical
- **Route Check If node** with Call Ollama (Code) and Call Claude (HTTP Request) branches
- **Merge node** combining responses from both routing paths before Extract Response
- **SSH access fix** - VM 400 uses muzakkir user with sudo access, not direct root login
- **NPM configuration** - proxy_buffering off in advanced config for streaming JSON responses

#### Fixed
- **SSH authentication to VM 400** - corrected user credentials (muzakkir vs root)
- **Open WebUI JSON parse error** - NPM proxy_buffering setting for streaming responses
- **httpRequestWithAuthentication limitation** - replaced with separate HTTP Request node with credentials
- **Extract Response node reference** - explicitly reference $('Merge').first().json for proper data flow

#### Architecture
- **Primary routing:** Simple queries → Ollama qwen3:14b (local, fast, cost-effective)
- **Complex routing:** Complex keywords OR >50 words → Claude Sonnet (cloud, advanced reasoning)
- **Fallback routing:** Ollama unavailable → Claude Haiku (cloud, basic queries)

#### Gilgamesh Evolution
- **Phase 1 Foundation** now complete with local LLM + hybrid routing
- **Cost optimization** achieved - most queries now processed locally on Ollama
- **Quality assurance** maintained - complex queries still use Claude's advanced capabilities

#### Planned Next
- **Phase 7E:** Extended Memory (20+ message conversations via RAG integration)
- **Phase 16.3:** Da Vinci Documentation Pipeline (append-only /update redesign)
- **MERLIN reminder agent** (highest priority for memory management)

---

### [April 24, 2026] - Phase 38 + 39: Ollama ROCm + Open WebUI (COMPLETE)

#### Added
- **VM 400 ollama-gpu** with Ubuntu 22.04, 6 CPU cores, 8GB RAM, 100GB disk on VLAN 30 (192.168.30.221)
- **RX 6700 XT PCIe passthrough** successful GPU acceleration for AI inference workloads
- **ROCm 6.1.3 runtime** installed and configured for AMD GPU compute support
- **Ollama service** running with 100% GPU utilization confirmed
- **AI models deployed:** qwen2.5:14b (8.7GB primary), llama3.2 (2.0GB backup)
- **Open WebUI** Docker container on port 3000 for web-based model interaction
- **Subdomain access:** ollama.najhin-gaming.com via NPM SSL termination
- **Cloudflare Access protection** (email OTP) for external security

#### Technical
- **GPU passthrough:** IOMMU Group 18 configured in Proxmox VM settings
- **ROCm compatibility:** HSA_OVERRIDE_GFX_VERSION=10.3.0 for RX 6700 XT support
- **External access:** OLLAMA_HOST=0.0.0.0 environment variable for network binding
- **vfio configuration:** Softdep modules loaded for GPU isolation

#### Fixed
- **VM disk space:** Resized root filesystem from 20GB to 100GB using lvextend + resize2fs
- **vfio module loading:** Added softdep configuration to /etc/modprobe.d/vfio.conf with initramfs update
- **ROCm GPU detection:** Environment override for RX 6700 XT compatibility (10.3.0 GFX version)
- **Open WebUI accessibility:** Network binding configuration for external container access

#### Milestone
- **Gilgamesh Evolution Phase 1 Foundation:** Ollama + Open WebUI infrastructure complete
- **Local AI infrastructure:** Ready for extended memory and hybrid routing development
- **Cost reduction path:** Foundation for reducing $30-40/month Claude dependency

#### Planned Next
- Phase 7E: Extended Memory (20+ message conversations via RAG)
- MERLIN reminder agent (highest priority for memory management)

---

### [April 24, 2026] - Phase 22: Obsidian Knowledge Base (COMPLETE)

#### Added
- **Obsidian installation** on Minimoon (Gaming PC) with vault at C:\Users\muzak\Nextcloud\Obsidian\second-brain
- **6 essential plugins:** Dataview, Tasks, Templater, Calendar, Excalidraw, Kanban
- **Structured folder system:** 00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 05-templates, 06-archive
- **Subscription tracker dashboard:** Individual notes per subscription using Dataview queries
- **Session summary template:** Standardized template for homelab session documentation

#### Technical
- **Auto-sync via Nextcloud:** Local vault syncs automatically to CT 220 cloud storage
- **Dashboard queries:** Dataview showing subscription costs and category totals
- **Template system:** 05-templates/session-summary.md for consistent documentation

#### Fixed
- **Nextcloud quota exceeded:** CT 220 root disk resized from 20GB to 100GB (99% full → 20% usage)
- **Dataview query errors:** Individual subscription notes required in separate folder structure

#### Infrastructure
- **CT 220 storage expansion:** Root disk 20GB → 100GB via `pct resize 220 rootfs +80G`
- **Thin pool overprovisioning:** Warning identified on local-lvm storage (deferred to infrastructure cleanup)

#### Planned
- **Phase 24.1:** Service Update Manager (Docker + apt + Proxmox updates with Telegram approval)
- **Data migration:** Move Nextcloud data directory to /mnt/data-storage (7.3TB HDD) in future phase

#### Action Items
- Add remaining subscriptions to Obsidian vault
- Fix Total by Category query (backtick parsing issue)
- Update Nextcloud to version 33.0.2
- Begin Phase 38 (Ollama + ROCm) next session

---

### [April 24, 2026] - Phase 15: Gilgamesh Additional Slash Commands (COMPLETE)

#### Completed
- **Phase 15:** Gilgamesh Additional Slash Commands deployed and tested

#### Added
- **6 new slash commands:** /help, /clear, /memory, /cost, /alerts, /backup
- **Command documentation:** Complete reference guide for all Gilgamesh commands
- **Memory management:** /clear command to reset conversation history
- **Cost tracking display:** /cost command shows token usage and estimated costs
- **Alert monitoring:** /alerts command displays active Alertmanager alerts
- **Backup status:** /backup command shows last backup times for all containers

#### Technical
- **Always Output Data enabled:** Get Alerts and Clear Memory DB nodes handle empty responses
- **Cost calculation:** Uses estimated token rates (Sonnet $3/$15 per 1M) due to cost_usd bug
- **Error handling:** Added proper array checks and chat ID references
- **Workflow optimization:** Removed redundant /temps and /storage commands (available in menu)

#### Fixed
- Clear Memory DB "at least one condition required": Added role not empty condition
- Send Clear Confirmation wrong chat_id: Use $('Get Chat ID').first().json.chat_id pattern
- Format Cost SyntaxError: Removed toLocaleString() and emojis from Code node
- Get Alerts/Format Alerts undefined handling: Added Array.isArray() checks

#### Known Issues
- **cost_usd column bug:** gilgamesh_costs.cost_usd always returns 0 (pre-existing from earlier phases)
- **Cost estimation:** Using token count approximation until Save Cost node investigation

#### Scope Changes
- **/logs command deferred:** Not practical without proper filtering, saved for future menu action
- **Command consolidation:** /temps and /storage removed (redundant with existing menu system)

---

### [April 24, 2026] - Phase 7D-Menu + Phase 14: Complete Gilgamesh Menu System

#### Completed
- **Phase 7D-Menu:** Complete inline keyboard menu system for Gilgamesh bot
- **Phase 14:** Secrets Management & Integration phase finalized

#### Added
- **Homelab → Metrics:** Real-time Proxmox system metrics (CPU, RAM, storage usage)
- **Homelab → Temps:** Live temperature monitoring via SSH to Kuromoon (lm-sensors)
- **Homelab → Storage:** Complete storage overview with usage percentages
- **Gaming submenu:** Direct Docker container status from CT 302
- **Gilgamesh info:** Bot capabilities and feature overview
- **Tools submenu:** Quick access links to all services
- **Help menu:** Complete command reference and usage guide

#### Technical
- **SSH access configured:** CT 211 → Kuromoon (pfSense rule TCP 22 added)
- **Container SSH enabled:** CT 302 root access with PermitRootLogin yes
- **Progress bars:** ASCII characters (= and -) for mobile compatibility
- **Storage fix:** kinmoon-nfs identified as correct Proxmox storage ID
- **Credentials added:** SSH Password account (Kuromoon), SSH CT302 Wings

#### Fixed
-