# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** September 23, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Architecture redesign complete. 7-layer model finalized. Midas CFO Agent, MERLIN Reminders, Daily Note Creator, Morning Briefing, Health Tracking all active. Obsidian Phases 22.1, 22.2, and 22.8B complete. Phase 24.7 (ntfy), 24.1 (Firefly III), and 24.8 (Langfuse) complete. Nextcloud Deck integration complete with Da Vinci project management. Hardware upgraded to 128GB DDR4 with 3-tier storage architecture. 22 LXC containers + 1 KVM VM deployed. Da Vinci Stage 2 (RAG) complete with Qdrant + nomic embeddings. Phase 7E (Extended Memory) complete with conversation archival. Pelican panel migration complete with Minecraft/Terraria split. Da Vinci Documentation Pipeline rebuilt May 19, 2026 with 3 separate Haiku API calls and immediate cost logging. Concurrency protection and inbox watcher schedule finalized May 18-19, 2026. Infrastructure troubleshooting complete May 20, 2026: CT 207 Promtail crash loop resolved (53,649 restarts), CT 304 tModLoader CPU leak fixed with cpulimit + daily cron. Phase 16.4 (Documentation Pipeline Expansion — 8 files) complete May 21, 2026: expanded from 3-file to 8-file sequential Haiku chain. decisions.md promoted to Phase 2 priority. Pipeline tested and verified May 21, 2026: 3 separate API calls, immediate cost logging, per-file system prompts, all 8 files successfully pushed to GitHub. Phase 24.8 (Langfuse) wired to Da Vinci Update Pipeline May 21, 2026: single trace (da-vinci-update) with 8 child generations logged per run. Traces confirmed in ClickHouse and accessible via API; UI trace list has known v3 self-hosted bug where traces don't appear in list view (1-hour aggregation delay). VM 400 disk expanded from 56GB to 86GB (LVM thin resize, 34GB free). Model testing complete: qwen3:14b confirmed as primary (honest about limitations), gemma3 + phi4-mini removed (confident hallucination). AI-CONTEXT max_tokens bumped to 25000 (was hitting ceiling). Langfuse CT 223 LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES set to true. Phase 24.9 (Personal Knowledge System) complete May 22, 2026: Langfuse UI trace list working (1-hour analytics delay resolved), Gilgamesh wired to Langfuse, Da Vinci Personal Knowledge gateway deployed, muzakkir-profile.md created and indexed in Qdrant (1,736 chunks), Gilgamesh successfully recalling personal facts via RAG, Knowledge Indexer updated to include 04-personal/ and expanded folder set (90 files indexed). Documentation audit (May 22, 2026) — corrections only: ROADMAP.md had 5 archived phases still showing as active; current-state.md had hallucinated hardware (EPYC 5645/256GB/RTX 4070 vs actual Ryzen 5 5600X/128GB/RX 6700 XT); agents.md MERLIN/Midas status incorrect; Knowledge Indexer folder list inconsistent across files. All corrections applied. Phase 24.10 (Triggered Qdrant Re-indexing) complete May 25, 2026: Post-write webhook added to Da Vinci Personal Knowledge gateway. Partial reindex (/davinci-reindex-personal, ~1s) triggers after muzakkir-profile.md writes. Full daily reindex (3am, ~21s) still operational. Phase 52 (DOCP Memory Optimization) complete May 2026. Web Search (Gilgamesh) deployed May 25, 2026: Firecrawl API integrated. Keyword-based intent detection, search results injected into context, search queries force-routed to Haiku (local models cannot follow injected context). Firecrawl credits per search: 2. All changes verified May 25, 2026. **Gaming platform expanded July 5, 2026: Enshrouded server (CT 306) deployed in Pelican Panel. Network architecture migrated from double-NAT (ISP router + pfSense) to true bridge mode on Huawei HG8145B7N, eliminating all double-NAT routing conflicts permanently. pfSense WAN reconfigured from DHCP to PPPoE. Public IP changed to 202.184.101.136 (was 202.184.35.79). TP-Link EAP610 AX1800 access point (SSID A21-22A) deployed and configured — root cause of AP failure identified and fixed: TL-SG108E switch ports 7-8 were on legacy VLAN 1, not VLAN20_MAIN. All 22 containers + 1 VM running. DNS records require update. Enshrouded external UDP connectivity not yet retested post-bridging.** **Agents ecosystem renamed from "Kuromoon" to "Chaldea" (July 8, 2026) — Kuromoon now refers to physical hardware only, Chaldea is the agents system layer.** **Gilgamesh renamed to Jeanne Alter ("The Corrupted Ruler"), pending full propagation across bot, n8n, Telegram, docs (July 8, 2026).** **Career timeline updated (July 8, 2026): September 2026 job-transition deadline dropped. Cloud/DevOps roles pursued at slower pace. Chaldea project reframed as indefinite, long-term. hdd-backup-2 Prometheus alert removed (copy-paste bug fixed, then intentionally disabled per user decision). Duplicate muzakkir-profile.md.md file exists in Nextcloud. Jeanne Alter assistant messages not saving to Data Table (silent conversation memory breakage).** **Documentation audit completed (July 9, 2026): Cross-session gap analysis identified seven previously-undocumented items from past sessions: Interest-Capture Loop concept, Four Blind Spots analysis (time vs priorities, docs drift, bus factor, skill-market fit), two domains tracked (najhin-gaming.com for gaming, muzakkir.tech for portfolio), two previously-named agents (Cu Chulainn renamed from Guardian May 16, Scathach prioritized May 16, Nightingale health concept May 17), and Phase 27 Domain Migration & Infrastructure Audit. Cu Chulainn and Scathach rename propagation pending. muzakkir.tech Cloudflare zone completion unconfirmed. agents.md structural incompleteness addressed (July 9, 2026): full sections drafted for MERLIN, Midas, EMIYA, Cu Chulainn, Scathach with Agent Type classifications and Funnel Agent design principle. MERLIN Cloudflare SSL expiry check no longer hardcoded — migrated to Uptime Kuma source July 9, resolving the July 14 urgency. Midas cost tracking planned migration from duplicate Data Table to Langfuse Metrics API (low urgency). Cu Chulainn and Scathach sections added with concept-stage governance. Jeanne Alter assistant message table save bug remains critical, unresolved.** **Jeanne Alter Email Management Pipeline design complete (July 9, 2026): Read + Notify tier, 4 personal accounts (muzakkir.kholil06@gmail.com, muzakkirkholil97@icloud.com, hyperjhin00@gmail.com, business.najhin@gmail.com), work and NSFW accounts excluded. Architecture: per-account triggers → shared Email Classifier → branches to notification and/or Da Vinci Personal Knowledge gateway for permanent categories (bills/payments/subscriptions). Staging store (~7-day retention) for transient email. 3x/day schedule. Credentials (Gmail OAuth2, iCloud IMAP app-specific password) going to n8n credential store — first concrete step of ecosystem-wide credential store migration. Build effort: ~6-8h across 6 rollout steps. Design approved, implementation not yet started.** **Monitoring fix complete (July 14, 2026): node_exporter `/mnt` exclusion bug root-caused and fixed. Debian package's default `--collector.filesystem.mount-points-exclude` regex included `mnt`, making all `/mnt` mountpoints (hdd-backup-1, hdd-backup-2, ssd-storage, kinmoon-smb) invisible to Prometheus since ~May 16. Fixed by editing `/etc/default/prometheus-node-exporter` ARGS to remove `mnt` from exclude list and restarting service. MountpointMissing_hddbackup1 alert (stuck active for 8 days) auto-resolved. Confirmed hdd-backup-1 is primary live storage for Nextcloud (not a backup copy); Kinmoon NAS receives nightly rsync at 03:00. Physical SATA cable/port swap remains deferred (no budget). All 22 containers + 1 VM remain running and healthy.** **Palworld server added July 16, 2026 (CT 307): Deployed in Pelican Panel to test multiplayer gaming infrastructure. External connectivity initially broken due to PublicIP misconfiguration (was pointing to internal LAN IP 192.168.30.219). Root-caused and fixed: Pelican egg "Public IP" variable had User Editable + User Viewable permissions unchecked (blocking manual updates via Startup tab); enabled permissions and set PublicIP to current WAN IP (202.184.109.124). PalWorldSettings.ini found to require unbroken single-line format for OptionSettings block — Pelican's web file editor introduces line breaks, corrupting the file and causing Palworld to silently ignore ALL OptionSettings (not just edited field). Established safe editing method: use `pct exec` + sed from Proxmox host, never the Pelican web editor. Pelican's file Download function (404 error) not yet debugged. In-game stutter with 3 concurrent players traced to nightly vzdump backup job (sequential all-container) running concurrently with gameplay (~02:11 AM), causing CPU/disk I/O contention. Backup/restart consolidation and DDNS automation elevated in priority. WAN IP instability observed: 3 changes in one week (202.184.101.136 → 202.184.103.49 → 202.184.109.124); confirmed as TIME PPPoE session renegotiation behavior. All 22 containers + 1 VM running.** **Total capacity: 22 LXC containers + 1 KVM VM (CT 307 Palworld addition July 16-17).** **House internet outage resolved (July 20-23, 2026): ONT PON light offline / LOS blinking red (ISP-side fiber fault) confirmed recovered by July 23. No homelab-side action required; ISP-level issue. Backup infrastructure audit July 23, 2026 completed: `backup-daily` vzdump job verified running 02:00 daily (~93-96 min runtime to ~03:35), storing to kinmoon-smb CIFS share (NOT a separate nightly rsync), compressing zstd, pruning keep-daily=7/keep-weekly=4. CT 306 (Enshrouded) and CT 307 (Palworld) discovered with NO backup coverage — not included in VMID list. CRITICAL: Kinmoon NAS Storage Pool 1 degraded due to Hard Drive 1 SMART failure (reallocated sector count: 573→609→1,963 trend over March-May, present 133 below failure threshold 140). RAID 1 mirror running on single healthy drive (Hard Drive 2) with zero redundancy. Do NOT click Repair until Hard Drive 1 is physically replaced. kinmoon-smb disk usage (93.3%) root-caused to UGOS-level recycle bin (`#recycle`, 1.3TB) double-counting logically-freed backup churn — NAS cleanup deferred, Hard Drive 1 replacement prioritized. CT 205 (Alertmanager) "CPU 100%" alert July 20-22: root-caused to Prometheus's CPU query including iowait (`%wa`), not genuine compute load; correlated to backup job's lingering I/O pressure on network share at 94% capacity. Alert rule not yet modified (diagnosis only).** **MAJOR INCIDENT — Kinmoon NAS Storage Pool 1 RAID 1 Rebuild Failure & Root Cause Analysis (July 24, 2026): Hard Drive 1 physically replaced with 3TB Seagate IronWolf (was original SMART-failed drive). Two consecutive RAID 1 rebuild attempts failed (01:32:29 and ~07:06). Root cause identified via UGOS event log export: Hard Drive 2 throws deterministic "failed command: WRITE FPDMA QUEUED" (Serious level) ~18-19 seconds after every boot, NOT a drive health issue (SMART shows healthy Reallocated Sector Count 99/10 threshold). This is a known, documented UGREEN DXP2800 SATA link-speed compatibility issue (confirmed via UGREEN DACH forum + external sources). Fix identified: force SATA to 3.0Gbps via kernel boot parameter `libata.force=3.0Gbps` in `/boot/EFI/debian/grub.cfg` and `/boot/EFI/debian/grub.am`. Fix NOT YET APPLIED pending emergency data backup completion (1.3TB vzdump archive copy to Kuromoon hdd-backup-2 via rsync started 2026-07-24 21:05, PID 3211563). Historical analysis: this WRITE FPDMA QUEUED signature detected intermittently since March 2026 on both drives at different times, suggesting original Hard Drive 1 "failure" may have been accelerated by this same SATA link instability rather than pure media wear. backup-daily job disabled (enabled 0) during rebuild attempts to reduce write load on Hard Drive 2. Storage Pool 1 currently DEGRADED; rebuild NOT reattempted pending fix application. When applied and verified, retry Repair; then re-enable backup-daily and add CT 306/307 to VMID backup list.** **RESOLVED (August 1, 2026): Kinmoon NAS Storage Pool 1 fully destroyed and recreated from scratch after GRUB `libata.force=3.0Gbps` fix applied and confirmed working (eliminated boot-time `WRITE FPDMA QUEUED` errors on Hard Drive 2). Four consecutive rebuild/recovery attempts abandoned after discovering UGOS `storage_serv` firmware bug in `RebuildFinished` event handling (`strconv.Atoi` parsing error causing `md: recover interrupted`). Clean pool destruction + fresh RAID 1 creation + shared folder recreation + full data restore from Kuromoon emergency backup completed 2026-07-31 01:10. New array UUID: `5bb187d0:b14f67a3:9f4d8ab9:16d079f8`. Both drives status: active sync, Normal/clean. Volume 1 ext4, 2.6TB, fully restored. CT 214 (Vaultwarden) Docker image updated to latest, Cloudflare Tunnel route now correctly configured (previously broken with stray CNAME and incorrect Service Type HTTPS). `backup-daily` vzdump job re-enabled, destination kinmoon-smb storage re-authenticated. All 22 containers + 1 VM running.** **INCIDENT (September 21, 2026): Kuromoon host experienced full-system unresponsiveness at ~19:00 local time — pveproxy, sshd, and CT 203 (Grafana) all accepted TCP connections but never completed application-level responses, while ICMP ping remained normal. Root cause unconfirmed; leading theory is I/O stall related to kinmoon-smb CIFS mount at 95% capacity (2.6TB / 2.7TB) combined with previously-documented Kinmoon Hard Drive 1 failing SMART status (July 23, 2026). Hard power-cycle at 20:37 restored service. CT 220 (nextcloud) failed to autostart post-reboot with lxc.hook.pre-start error — manually started successfully. Confirmed as recurring pattern on 2026-05-16, 2026-07-06, 2026-08-11, and 2026-09-21 (all post-boot; suspected race condition with `/mnt/hdd-backup-1` bind-mount not ready during autostart sequence). journald on Kuromoon silently halted logging on 2026-09-03 (18 days prior) with unknown cause — no forensic evidence remains (Prometheus down during freeze, dmesg/pstore cleared post-reboot). pfSense VLAN20_MAIN interface: two temporary USER_RULE pass rules added to enable Minimoon (192.168.20.101) direct access to Kuromoon (192.168.10.5) on ports 8006 (Proxmox GUI) and 22 (SSH) — decision pending whether to keep, tighten, or revert these rules. WAN IP observed as 202.184.116.231 during console access (differs from July 17 documented value 202.184.109.124, consistent with known PPPoE renegotiation). Kinmoon Hard Drive 1 replacement elevated to active priority (was background item) due to plausible link to Kuromoon instability. kinmoon-smb space freeing also elevated to priority. CT 220 autostart startup delay mitigation planned. External alert for Kuromoon host-level unresponsiveness recommended (current Prometheus/Grafana monitoring blind during host freeze). All 22 containers + 1 VM confirmed running post-recovery.** **Phase 37 (Kuromoon Host Stability & Monitoring) — partial (September 22, 2026): SSH key-based authentication fully deployed across all 4 core hosts (Kuromoon, Kinmoon, VM 400, Pi-hole) via `~/.ssh/id_ed25519_homelab` on Minimoon with `~/.ssh/config` aliases (`kuromoon`, `kinmoon`, `vm400`, `pihole`). Tailscale subnet router (`pfsense-homelab`, advertising VLAN10_MGMT + VLAN30_SERVICES) renewed (auth key expired Aug 28, expiry now permanently disabled). Tailscale also installed directly on Kuromoon (100.89.254.28) and VM 400 (100.94.179.99) for redundancy. Kuromoon host-freeze watchdog deployed on Pi-hole at `/usr/local/bin/kuromoon-watchdog.sh` running every 2 minutes via cron — checks HTTPS response from Proxmox GUI on both LAN (192.168.10.5:8006) and Tailscale (100.89.254.28:8006) paths, fires Telegram alert after 3 consecutive failures (~6 min), alerts recovery when back online. Fully tested end-to-end (stopped `pveproxy` to simulate freeze, confirmed alerts fired and resolved correctly). Three temporary VLAN20_MAIN firewall rules from Sept 21 incident deleted (Tailscale covers access). Remaining Phase 37 sub-items: Kinmoon drive capacity freeing (now 96% used, 2.5TB/2.6TB), CT 220 autostart race mitigation, Sept 3 journald silent-logging investigation.** **CRITICAL DISCOVERY (September 22, 2026): `backup-daily` vzdump job scheduler silently ceased all job-start attempts for 34 days (Aug 18 - Sep 21, 2026), confirmed via `journalctl -u pvescheduler` showing zero entries in that window. This is NOT a job-failure condition (which would appear as failed task records) but a complete scheduler silence — every VMID (201-208, 211, 213, 214, 220-223, 302-305, 400, 306, 307) had NO backup coverage for over a month. Root cause unconfirmed; leading theory ties to the same host instability documented in Sept 21 freeze (pvescheduler service only confirmed alive as of Sept 21 20:38, the documented hard-reboot timestamp), but Sept 3 journald gap only covers the back half of this window — Aug 18-Sep 3 remains unexplained. Tonight's backup-daily run (Sep 22 02:00-03:45) completed successfully for all 22 VMIDs + 1 VM, confirming the job is healthy as of this session. VM 223 was the long pole (58 min, CPU-bound tar process, likely high file count — verified via `ps aux` showing active `R`-state process with climbing CPU time). Kinmoon Volume 1 capacity crisis (96% used) root-caused to UGOS `#recycle` folder accumulating 774G of stale backup chunks pruned by Proxmox's own retention policy (keep-daily=7, keep-weekly=4), double-counting space logically freed on-disk. Backlog manually purged via `sudo rm -rf "/volume1/proxmox-backups/#recycle"/*` on Kinmoon; capacity dropped from 96% (118GB free) to 73.76% used (752GB available) post-cleanup. Auto-purge retention policy (7-14 days) NOT yet configured on UGOS shared folder — without this, the capacity crisis will silently recur over the next 34-day cycle. CT 306 (Enshrouded) and CT 307 (Palworld) confirmed present in `/etc/pve/jobs.cfg` backup-daily VMID list — closes the backup-coverage gap documented since July 23, 2026. Exact date these two were added is unknown/undocumented.** **UPS selection (September 22, 2026): CyberPower CP1600EPFCLCD (Pure Sine Wave, RM 1,184) selected for RM 2,000 budget and added to Shopee cart (pending purchase). Evaluated and rejected APC Easy UPS BV1000I-MS (RM365, stepped waveform) and APC Back-UPS BX1200MI-MS (RM878, stepped waveform) due to battery-mode waveform compatibility risk with Kuromoon's Active PFC power supply and short realistic runtime (1-5 min). APC Back-UPS Pro (BR-series, pure sine wave, strong NUT support) was initial preference but unavailable on Shopee at acceptable cost. CyberPower's dedicated "PFC Sinewave" product line selected instead — explicitly designed for Active PFC, pure sine wave confirmed, one of three mainstream NUT-supported brands (alongside APC, Eaton), 1000W rating provides 3x headroom over realistic 200-350W combined load (Kuromoon + Kinmoon + core network). Rationale: UPS addresses failure category (dirty power, unclean shutdown) directly implicated in multi-day recovery incidents this year (Kinmoon RAID rebuild, Sept 21 host freeze), with near-zero ongoing maintenance burden. Deferred Kinmoon drive upgrades (capacity crisis resolved this session), RX 6700 XT upgrades (no bottleneck), and 10GbE networking (no evidence of network bottleneck).** **Research phase: Local coding LLM models for RX 6700 XT (12GB VRAM) shortlisted (Qwen2.5-Coder-14B-Instruct, DeepSeek-Coder-V2-Lite-16B) for future EMIYA execution engine; ruled out Qwen3-Coder-30B-A3B (MoE, too large). n8n-claw open-source project analyzed as design template for Jeanne Alter Architecture Refactor — adopted MCP Bridge pattern, `soul` personality-table pattern, Heartbeat notify pattern; explicitly rejected parallel Project Memory doc store (conflicts with Da Vinci sole-writer) and plaintext credential storage. Phase 24.11 (Credential Store Migration) sequenced before Phase 24.12 (Jeanne Architecture Refactor) per existing dependency chain. Decision: do not start EMIYA execution engine or MCP "hands" before Phase 24.11/24.12 complete, to avoid building MCP twice.** **Phase 27.1 (Infrastructure Audit — Cloudflare Security) complete (September 22, 2026): Full read-only Cloudflare audit performed covering zones (najhin-gaming.com + muzakkir.tech verification), DNS records, Zero Trust Access policies, Tunnel config, SSL/TLS settings. muzakkir.tech confirmed NOT to exist as a Cloudflare zone on the account (not active, pending, or paused) — Phase 27.2 (Domain Migration) cannot start until zone is created at registrar level. Vaultwarden Access misconfiguration identified and fixed: Bypass-Everyone policy (silently overriding Email-OTP Allow per Cloudflare's rule precedence) removed; Email-OTP now actually enforced on login. Nextcloud Access narrowed from whole-domain bypass to 4 path-scoped apps (`/remote.php`, `/ocs`, `/status.php`, `/public.php`); settings/admin/UI now requires Email OTP. Pulse (home.najhin-gaming.com) given new Access application (previously zero Cloudflare-layer auth). TLS hardened: min_tls_version raised from 1.0 to 1.2, always_use_https enabled. DNS drift fixed: 6 stale A-records updated to current WAN IP (202.184.116.231); `mc.` and `terraria.` game-server records remain stale (out of scope). CT 207 (network-ddns) actual mechanism corrected: favonia/cloudflare-ddns Docker container (not ddclient), API token had been invalid since 2026-07-04 21:10 UTC; new token issued, installed, restarted, confirmed working (no more error 9109). Decision made: fix najhin-gaming.com production issues now rather than wait for muzakkir.tech migration (Phase 27.2), since Zone 27.2 can't proceed regardless and najhin-gaming.com is permanent production infrastructure. Pending manual follow-up: n8n webhook secret-validation check (5 non-Telegram endpoints: `doc-update`, `doc-sync`, `da-vinci`, `midas-report`, Alertmanager routing), Obsidian WebDAV end-to-end sync test post-Nextcloud-bypass-narrowing.** **Phase 37 (Kuromoon Host Stability & Monitoring) — Continued (September 23, 2026): n8n webhook security review completed — investigation into the 5 flagged non-Telegram webhook endpoints from the Cloudflare audit discovered that Da Vinci — Sync Docs Pipeline and Midas — CFO Report had zero execution history ever (not triggered, no active consumers) and were archived rather than secured (unused public webhook = pure attack surface with no offsetting functional value). Da Vinci — Update Pipeline and Da Vinci — Inbox Watcher confirmed NOT internet-reachable (Execute-Workflow and Schedule triggers respectively, not Webhook triggers) — no action needed. Emiya — Service Down Alert workflow archived after confirming Alertmanager's live config (/etc/alertmanager/alertmanager.yml, unchanged since 2026-02-04) never actually contained a webhook_configs block referencing n8n; existing documentation describing this dependency was inaccurate. Investigation root-caused why Alertmanager's critical-alerts Telegram delivery was broken since at least Sept 21 (Sept 21 host-freeze critical alert only reached Discord, not Telegram, silently): invalid bot token. Created new dedicated Telegram bot via BotFather (separate from Jeanne Alter's bot, guaranteed distinct by construction, eliminates collision risk), moved token from inline plaintext in alertmanager.yml to permissioned secrets file `/etc/alertmanager/secrets/telegram_bot_token` (owner alertmanager:alertmanager, mode 600, directory mode 700, referenced via `bot_token_file` supported in Alertmanager 0.27.0+), confirmed via `amtool check-config`, reloaded via `systemctl reload alertmanager`, and end-to-end delivery verified via two synthetic critical alerts (both FIRING and RESOLVED Telegram messages confirmed received). n8n active workflow count reduced from 12 to 9 after archiving 3 workflows (Da Vinci — Sync Docs Pipeline, Midas — CFO Report, Emiya — Service Down Alert).**

---

## 👤 About Me

| Field            | Details                                              |
|------------------|------------------------------------------------------|
| **Name**         | Muzakkir Kholil                                      |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location**     | Petaling Jaya, Selangor, Malaysia                    |
| **Target Role**  | Cloud Engineering / DevOps                           |
| **Domains**      | najhin-gaming.com (Cloudflare, gaming servers permanent), muzakkir.tech (Cloudflare, portfolio/homelab — zone does NOT exist as of Sept 22, requires creation at registrar level) |
| **GitHub**       | github.com/muzakkir97                                |
| **Career Status** | Actively applying at measured pace (no deadline)     |

---

## 🎯 My Preferences (IMPORTANT)

### Learning Style

- **Prefer complex over simple** — I want to understand the "why", not just copy-paste commands
- **Explain like you're teaching** — Don't assume I know; explain concepts before implementation
- **No shortcuts** — I'd rather learn the hard way if it teaches more

### Documentation Requirements

- **GitHub docs must be sanitized** — Replace real IPs with `192.168.x.x` or `[PLACEHOLDER]`
- **LinkedIn posts for each phase** — Professional updates showing progress
- **Dual documentation** — Public (sanitized) + Private (full details)

### Communication

- **Don't use multiple-choice widgets during deployment** — Just give me the instructions
- **Be direct** — Tell me if my idea is bad and suggest alternatives
- **It's okay to say "I don't know"** — I prefer honesty over guessing
- **Make recommendations directly** — No re-asking confirmed decisions, single confirmation then proceed

### Naming Conventions

- **Function-based names:** `monitoring-prometheus`, `gaming-panel`, `network-ddns`
- **Moon theme for hardware:** Kuromoon (Proxmox), Kinmoon (NAS), Minimoon (Gaming PC)
- **Chaldea theme for agents:** Agents system as one cohesive ecosystem; individual agents use Fate/Grand Order servant theming

### Priority Framework

- **Health → Mental → Work → Gaming → n8n business → Homelab**
- Gaming pipeline is self-care, not distraction
- n8n business priority sits below gaming (health/mental focus)

---

## 🏗️ 7-Layer Architecture (v2.0)

### Layer 1: Input (Data Collection)
- **Telegram** → Jeanne Alter (primary interface)
- **Email** → Jeanne Alter (Read + Notify tier, design complete July 9, 4 personal accounts)
- **Health sensors** → Samsung Health/Lepulse scale → Health Connect bridge
- **Voice memos** → Claude API transcription
- **Photos** → Llama 3.2 Vision analysis
- **Web content** → Firecrawl scraping

### Layer 2: Brain (Intelligence)
- **Primary:** Ollama qwen3:14b (local, RX 6700 XT)
- **Fallback:** Claude Haiku (API)
- **Complex:** Claude Haiku (API for Da Vinci only)
- **Vision:** Llama 3.2 Vision 11B (VM 400)
- **Routing:** Smart complexity detection + web search routing

### Layer 3: App (Services)
- **Firefly III** → Personal finance tracking (CT 221)
- **ntfy** → Universal notification hub (CT 222)
- **Langfuse** → LLM observability (CT 223)
- **Nextcloud** → File sync + WebDAV
- **Vault** → Secrets management

### Layer 4: Knowledge (Storage + Recall)
- **Obsidian** → Human-readable notes
- **Qdrant** → Vector search (RAG)
- **n8n Data Tables** → Fast agent queries
- **Proactive recall** → Context-aware knowledge injection

### Layer 5: Memory (Agent State)
- **Conversation history** → 15 messages (jeanne_alter_conversations)
- **Session modes** → health_logging, etc.
- **Goal tracking** → progress + blockers
- **Agent coordination** → shared state

### Layer 6: Observability (Monitoring)
- **Langfuse** → LLM performance tracking (wired to Jeanne Alter and Da Vinci)
- **Midas** → Cost analysis + optimization
- **Grafana** → Infrastructure metrics
- **Da Vinci** → Agent behavior logs

### Layer 7: Notification (Output)
- **ntfy** → Universal push notifications
- **Telegram** → Interactive responses
- **Obsidian writes** → Structured logging
- **Discord** → Gaming server management

---

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                                    | IP Address     | Role                              |
|------------------|----------|------------------------------------------|----------------|-----------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 128GB DDR4-3200, RX 6700 XT 12GB | 192.168.10.5   | Hypervisor                        |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                 | 192.168.10.1   | Router, Firewall                  |
| Managed Switch   | —        | TP-Link TL-SG108E                        | 192.168.1.20   | Layer 2, VLANs                    |
| NAS              | Kinmoon  | UGREEN DXP2800, 5.4TB RAID 1            | 192.168.10.100 | Backups (RAID 1, ext4)           |
| DNS Server       | —        | Raspberry Pi 4                           | 192.168.30.10  | Pi-hole (~489K domains blocked)   |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB         | 192.168.20.101 | **Gaming only — never homelab**   |
| WiFi AP          | —        | TP-Link EAP610 AX1800                    | 192.168.20.x   | Deployed July 5, 2026 (SSID A21-22A) |

### GPU Notes (Kuromoon)

- RX 6700 XT passed through to VM 400 (ollama-gpu) for Ollama/ROCm AI inference
- PCIe addresses: GPU (0d:00.0), Audio (0d:00.1) — updated after motherboard change
- ASUS TUF B550M-E motherboard with AMD-V/SVM and IOMMU enabled
- Idle baseline: CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan
- HSA_OVERRIDE_GFX_VERSION=10.3.0 set via systemd override for gfx1031 compatibility

### RAM Upgrade (Complete - May 16, 2026)

- **Installed:** 4x32GB Corsair Vengeance LPX DDR4-3200 (128GB total)
- **Speed:** DDR4-3200 MT/s confirmed (manual BIOS tuning from 2666 to rated speed)
- **Purpose:** VM/LXC density, large AI models, multiple workloads

### Storage Architecture (3-Tier, Complete - May 16, 2026)

| Tier | Storage    | Device              | Capacity | Mount Point       | Purpose                          |
|------|------------|---------------------|----------|-------------------|----------------------------------|
| 1    | local-zfs  | NVMe ZFS RAID1      | 770GB    | /                | OS, VM disks, fast containers     |
| 2    | ssd-storage| WD Green 1TB SATA   | 927GB    | /dev/sdd         | Rootdir/images, medium workloads  |
| 3    | hdd-backup | 2x8TB SATA HDD      | 14.6TB   | /mnt/hdd-backup-* | Backups, Nextcloud data          |

### Health Sensors

- **Lepulse scale** → Fitdays app → Samsung Health → Health Connect bridge
- **Samsung Watch** → Samsung Health → Health Connect bridge
- **Data flow:** Sensors → Samsung Health → Health Connect webhook bridge → n8n → Obsidian + Data Tables

---

## 🌐 Network Architecture

### Topology: Single NAT (Bridge Mode — Permanent July 5, 2026)

```
Internet → TIME Fiber (GPON, bridged mode) → Huawei HG8145B7N (bridge mode, no NAT/routing/DHCP)
                                                      ↓
                                         pfSense WAN (pppoe0, 202.184.116.231 — see WAN IP Instability)
                                                      ↓
                                            802.1Q Trunk (igc2)
                                                      ↓
                                        TP-Link TL-SG108E
                                                      ↓
                    ┌─────────┬─────────┬─────────┬─────────┐
                  VLAN10   VLAN20   VLAN30   VLAN40   VLAN50
                  Mgmt     Main    Services   DMZ    Malware
```

**Major Change (July 5, 2026):** Eliminated double NAT topology by placing ISP router (Huawei HG8145B7N) into true bridge mode. pfSense now has direct PPPoE connection to the ISP. **This was the underlying root cause of the Enshrouded UDP forwarding failure** — double NAT was blocking inbound UDP traffic. Bridge mode is permanent and resolves this category of problem for all future services requiring inbound access.

### WAN IP Instability (Documented July 16-17, 2026; Updated September 23, 2026)

**Observed instability:** Five public IP address changes have occurred:
1. 202.184.35.79 → 202.184.101.136 (July 5, 2026, bridge mode migration)
2. 202.184.101.136 → 202.184.103.49 (July 16, 2026)
3. 202.184.103.49 → 202.184.109.124 (July 16, 2026, second change same day)
4. 202.184.109.124 → 202.184.116.231 (September 21, 2026)

**Current WAN IP (as of September 23, 2026):** 202.184.116.231

**Root cause:** TIME Fiber PPPoE session renegotiation behavior — IP reassignment occurs on PPPoE reconnect, not strictly predictable intervals. ISP-level DHCP behavior (IPs appear to be DHCP-assigned rather than static), not a bridge-mode artifact.

**Impact on gaming servers:** Palworld (CT 307) PublicIP field and Cloudflare DNS records require manual updates after each WAN IP change. Impact low for established games (friend-invite/Steam relay mechanisms work regardless), but external port-forwarding-based connectivity (raw IP join) breaks until manual update.

**Mitigation strategy:** DDNS automation elevated to high priority (previously queued roadmap item). Should implement automatic Cloudflare DNS update on WAN IP change (detect via pfSense/Uptime Kuma, trigger Cloudflare API update, optionally trigger Palworld PublicIP field update via Pelican API if feasible).

### ISP Router Configuration (Huawei HG8145B7N)

- **Mode:** True bridge mode (all routing, NAT, DHCP, Wi-Fi disabled on this device)
- **Function:** Pure pass-through for single PPPoE session only
- **Wi-Fi/LAN ports on ISP router:** Now provide NO internet to directly-connected devices — this is expected, permanent behavior of bridge mode, not a fault
- **Reason for bridge mode:** To eliminate double-NAT layer and allow pfSense direct internet access for proper port forwarding and external service connectivity

### pfSense Configuration (Post-Bridge Mode)

- **WAN interface:** Changed from DHCP to **PPPoE (pppoe0)**
- **PPPoE credentials:** Username `muzakkir655@timebb`, password stored in Vault
- **Public IP:** `202.184.116.231` (current as of September 23, 2026; subject to ISP-initiated changes on PPPoE renegotiation — see WAN IP Instability section above)
- **Note:** ISP router's port forwarding table is now irrelevant (dead) — all port forwarding handled exclusively by pfSense

### VLAN Design (OPERATIONAL)

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                |
|---------|-----            |-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices + Wi-Fi (AX1800)       |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)     |

### TP-Link TL-SG108E VLAN Port Assignments (Fixed July 5, 2026)

Fixed ports 7-8 which were on legacy VLAN 1 (blocking AX1800 access point traffic):

| Port | PVID | Untagged VLAN | Purpose |
|------|------|---------------|---------|
| 1-2  | trunk | Tagged, all VLANs | Uplinks (to pfSense / other switches) |
| 3    | 20   | VLAN20_MAIN | Client device |
| 4    | 20   | VLAN20_MAIN | Client device |
| 5    | 30   | VLAN30_SERVICES | Homelab service device |
| 6    | 10   | VLAN10_MGMT | Management device |
| 7    | 20   | VLAN20_MAIN | TP-Link EAP610 AX1800 access point uplink (fixed from legacy VLAN 1 this session) |
| 8    | 20   | VLAN20_MAIN | Available for client wired connections (fixed from legacy VLAN 1 this session) |

**Note:** Switch's own management IP (192.168.1.20) remains on legacy 192.168.1.0/24 range — deferred cleanup (Phase 26).

### Remote Access via Tailscale (September 23, 2026)

**Tailscale tailnet overview:**
- **Subnet router:** `pfsense-homelab` (pfSense appliance) — advertises VLAN10_MGMT (192.168.10.0/24) and VLAN30_SERVICES (192.168.30.0/24), auth key expiry permanently disabled
- **Direct Tailscale nodes:** Kuromoon (100.89.254.28), VM 400 (100.94.179.99), Minimoon (Windows), Android phone (Samsung S25 Ultra)
- **Not on Tailscale:** Kinmoon (UGOS appliance OS, no native support) — relies on pfSense's advertised subnet route instead, confirmed working
- **DNS resolution:** Tailscale Magic DNS handles `.0x0.dev` domain names (tailnet's reverse DNS)

**SSH Access via Tailscale (All from Minimoon):**

| Hostname | Tailscale IP | Alias  | User    | Command |
|----------|--------------|--------|---------|---------|
| Kuromoon | 100.89.254.28 | kuromoon | root | ssh kuromoon |
| Kinmoon  | (via pfSense route) | kinmoon | Muzakkir | ssh kinmoon |
| VM 400   | 100.94.179.99 | vm400  | muzakkir | ssh vm400 |
| Pi-hole  | (via pfSense route) | pihole | pi | ssh pihole |

**SSH Config (Minimoon `~/.ssh/config`):**
```
Host kuromoon
    HostName 100.89.254.28
    User root
    IdentityFile ~/.ssh/id_ed25519_homelab

Host kinmoon
    HostName kinmoon.0x0.dev
    User Muzakkir
    IdentityFile ~/.ssh/id_ed25519_homelab

Host vm400
    HostName 100.94.179.99
    User muzakkir
    IdentityFile ~/.ssh/id_ed25519_homelab

Host pihole
    HostName pihole.0x0.dev
    User pi
    IdentityFile ~/.ssh/id_ed25519_homelab
```

**SSH Key:** `~/.ssh/id_ed25519_homelab` (ed25519, copied to `authorized_keys` on all 4 hosts)

### Kuromoon Host-Freeze Watchdog (September 22, 2026)

**Deployment:** Pi-hole (`/usr/local/bin/kuromoon-watchdog.sh`)
**Trigger:** Cron every 2 minutes (`*/2 * * * *`)
**Monitoring approach:** HTTPS response check (not ICMP) — tonight's Sept 21 incident proved ICMP ping remains healthy during full application-layer freeze

**Logic:**
- Check Proxmox GUI response on LAN path (192.168.10.5:8006)
- Check Proxmox GUI response on Tailscale path (100.89.254.28:8006)
- If both fail 3 consecutive times (~6 min): send 🔴 failure alert via Telegram Bot API
- When response returns: send ✅ recovery alert via Telegram Bot API
- Track state in `/var/tmp/kuromoon-watchdog-state` and `/var/tmp/kuromoon-watchdog-failcount`

**Alert delivery:** Direct Telegram Bot API call (not ntfy) — ntfy runs on Kuromoon itself, so it would be unavailable during the very failure being reported

**Testing:** Fully tested end-to-end Sept 22 — stopped `pveproxy` on Kuromoon, confirmed 🔴 alert fired after 3 checks; restarted `pveproxy`, confirmed ✅ recovery alert fired correctly

---

## 📦 Infrastructure Inventory (Proxmox)

> **Note:** CTID 201–307 are LXC containers. VMID 400 is a KVM virtual machine with PCIe GPU passthrough — it is NOT an LXC container.

| ID  | Type | Name                    | IP             | Subdomain                     | Autostart | Status    |
|-----|------|-------------------------|----------------|--------------------------------|-----------|-----------|
| 201 | LXC  | nginx-proxy-manager     | 192.168.30.201 | —                             | ✅         | ✅ Running |
| 202 | LXC  | monitoring-prometheus   | 192.168.30.202 | —                             | ✅         | ✅ Running |
| 203 | LXC  | monitoring-grafana      | 192.168.30.203 | grafana.najhin-gaming.com     | ✅         | ✅ Running |
| 204 | LXC  | monitoring-loki         | 192.168.30.204 | —                             | ✅         | ✅ Running |
| 205 | LXC  | monitoring-alertmanager | 192.168.30.205 | —                             | ✅         | ✅ Running |
| 206 | LXC  | monitoring-uptime       | 192.168.30.206 | —                             | ✅         | ✅ Running |
| 207 | LXC  | network-ddns            | 192.168.30.207 | —                             | ✅         | ✅ Running |
| 208 | LXC  | dashboard-pulse         | 192.168.30.208 | home.najhin-gaming.com        | ✅         | ✅ Running |
| 211 | LXC  | automation-n8n          | 192.168.30.211 | n8n.najhin-gaming.com         | ✅         | ✅ Running |
| 213 | LXC  | vault                   | 192.168.30.213 | vault.najhin-gaming.com       | ✅         | ✅ Running |
| 214 | LXC  | password-vaultwarden    | 192.168.30.214 | passwords.najhin-gaming.com   | ✅         | ✅ Running |
| 220 | LXC  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com       | ✅         | ⚠️ Recurring autostart issue |
| 221 | LXC  | finance-firefly         | 192.168.30.224 | finance.najhin-gaming.com     | ✅         | ✅ Running |
| 222 | LXC  | notification-ntfy       | 192.168.30.222 | ntfy.najhin-gaming.com        | ✅         | ✅ Running |
| 223 | LXC  | observability-langfuse  | 192.168.30.223 | langfuse.najhin-gaming.com    | ✅         | ✅ Running |
| 302 | LXC  | gaming-wings-1          | 192.168.30.212 | —                             | ✅         | ✅ Running |
| 303 | LXC  | minecraft-server        | 192.168.30.215 | —                             | ✅         | ✅ Running |
| 304 | LXC  | terraria-server         | 192.168.30.216 | —                             | ✅         | ✅ Running |
| 305 | LXC  | gaming-panel-pelican    | 192.168.30.217 | panel.najhin-gaming.com       | ✅         | ✅ Running |
| 306 | LXC  | enshrouded-server       | 192.168.30.218 | —                             | ✅         | ✅ Running |
| 307 | LXC  | palworld-server         | 192.168.30.219 | —                             | ✅         | ✅ Running |
| 400 | VM   | ollama-gpu              | 192.168.30.221 | ollama.najhin-gaming.com      | ✅         | ✅ Running |

**Current Total: 22 LXC containers + 1 KVM VM**
**Planned Total: 22 LXC containers + 1 KVM VM (all on VLAN 30, all autostart enabled)**

### Pulse Dashboard (CT 208)

- **Container:** rcourtman/pulse:latest on port 7655 (resized to 8GB storage)
- **Purpose:** Proxmox-native monitoring dashboard replacing Homepage
- **Features:** Real-time metrics, container status, zero configuration required
- **Agents:** Installed on Kuromoon (Proxmox host) and CT 302 (Docker monitoring)
- **Access:** home.najhin-gaming.com (Websockets Support enabled in NPM)
- **Network:** pfSense rules added for CT 208 → Proxmox API (8006), Kuromoon → CT 208 (7655)
- **Cloudflare Access:** Email OTP (muzakkir.kholil06@gmail.com) added September 22, 2026 — previously had zero Access protection

### Nextcloud Hub (CT 220)

- **Root disk:** 100GB (resized from 20GB to resolve quota issues)
- **Data directory:** /mnt/ncdata (bind mount from /mnt/hdd-backup-1/nextcloud-data)
- **Version:** 33.0.2
- **Storage:** Data on Tier 3 HDD, app on NVMe (1.3GB after migration)
- **WebDAV base URL:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/
- **Enabled apps:** Deck (project management), Tasks, Calendar
- **Backup:** Now backed up via Proxmox's native vzdump job (backup-daily, 02:00 daily) to kinmoon-smb CIFS share
- **Known issue:** Duplicate `muzakkir-profile.md.md` file exists in 04-personal/ (WebDAV write-race artifact between Obsidian sync and the Da Vinci Personal Knowledge gateway) — pending cleanup
- **Cloudflare Tunnel:** `cloudflared` systemd service runs inside this container (not on Proxmox host), connectorID tracked
- **Autostart issue (September 21, 2026):** Failed to autostart post-reboot with error "Failed to run lxc.hook.pre-start for container '220'" / "TASK ERROR: startup for container '220' failed". This is a recurring pattern across 4+ reboots (2026-05-16, 2026-07-06, 2026-08-11, 2026-09-21), always immediately post-boot, never during normal runtime. **Root cause hypothesis:** Race condition where the `/mnt/hdd-backup-1` bind-mount source (backing CT 220's `/mnt/ncdata`) isn't ready yet when Proxmox's bulk autostart sequence reaches CT 220. **Mitigation pending:** Set startup delay on CT 220 via Options → "Start at boot" (deferred to future session).
- **Cloudflare Access:** Narrowed from whole-domain Bypass to 4 path-scoped Bypass apps (`/remote.php`, `/ocs`, `/status.php`, `/public.php`) on September 22, 2026; all other paths (settings, admin, file browser UI) now require Email OTP

### Firefly III (CT 221)

- **Container:** fireflyiii/core:latest + MariaDB backend
- **IP:** 192.168.30.224 (NOTE: not .221, which is VM 400)
- **Purpose:** Personal finance tracking (MYR currency)
- **Access:** finance.najhin-gaming.com (Cloudflare Access + Email OTP)
- **API:** Token stored in Vaultwarden (name: gilgamesh)
- **Backup:** Included in backup-daily vzdump job (CT 221 in VMID list)

### ntfy (CT 222)

- **Container:** binwiederhier/ntfy:latest on port 2586
- **Purpose:** Universal notification hub for all agents
- **Auth:** Built-in username/password (not Cloudflare Access for phone app compatibility)
- **Access:** ntfy.najhin-gaming.com (Cloudflare Tunnel without Access)
- **Integration:** Uptime Kuma → ntfy alerts configured
- **Backup:** Included in backup-daily vzdump job (CT 222 in VMID list)

### Langfuse (CT 223)

- **Stack:** Langfuse v3.174.1 (6-container stack: web, worker, ClickHouse, PostgreSQL, Redis, MinIO)
- **Container specs:** 2 cores, 4GB RAM, 16GB disk
- **Purpose:** LLM observability and performance tracking
- **Access:** langfuse.najhin-gaming.com (Cloudflare Access + Email OTP)
- **Organization:** Kuromoon, Project: Gilgamesh Agents (name reflects historical identity; will update to Chaldea post-propagation)
- **Internal URL:** http://192.168.30.223:3000
- **Health endpoint:** /api/public/health
- **Status:** Wired to Da Vinci Update Pipeline and Jeanne Alter — traces active and visible in UI (1-hour analytics delay resolved)
- **Tracing:** Two trace types:
  - **da-vinci-update:** One trace with 8 child generations logged per Da Vinci pipeline run (files: AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)
  - **gilgamesh-chat:** One trace per Jeanne Alter message with input/output/metadata (routedTo, ragUsed, commandType, chatId) — NOTE: assistant messages currently not saving to Data Table, breaking memory persistence (known issue)
- **UI Status:** Working — trace list now displays traces with 1-hour analytics aggregation delay (traces appear after 1 hour, not immediately). Direct API (/api/public/traces) returns results immediately. Direct URL access works immediately.
- **Environment variable:** LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES=true
- **Backup:** Included in backup-daily vzdump job (CT 223 in VMID list)

### network-ddns (CT 207)

- **Container:** LXC Ubuntu 22.04
- **IP:** 192.168.30.207
- **Services:** Promtail (log forwarding), favonia/cloudflare-ddns Docker container
- **Promtail Status:** Fixed May 20, 2026 — crash loop resolved (53,649 restarts), YAML corrupted with shell commands pasted into config file
- **Loki URL:** http://192.168.30.204:3100/loki/api/v1/push (CT 204, VLAN 30)
- **DDNS Mechanism:** Docker container `favonia/cloudflare-ddns` running at `/opt/cloudflare-ddns/docker-compose.yml` (NOT ddclient as previously documented)
- **DDNS Status:** API token had been invalid since 2026-07-04 21:10 UTC (error 9109); new token issued, installed, restarted, and confirmed working on September 22, 2026. Token is scoped to Zone:DNS:Edit for najhin-gaming.com only.
- **DDNS Scope:** `DOMAINS` env var: `najhin-gaming.com,*.najhin-gaming.com`
- **Note:** node_exporter in this container reads host /proc/stat, so CPU alerts are host-level metrics, not CT 207-specific
- **Backup:** Included in backup-daily vzdump job (CT 207 in VMID list)

### minecraft-server (CT 303)

- **Container:** Paper 1.21.4 on port 25570 (192.168.30.215)
- **Panel:** Pelican Wings
- **CPU limit:** Set via Pelican panel
- **Note:** Watch for CPU ramp-up issues; set cpulimit in Proxmox if needed
- **Backup:** Included in backup-daily vzdump job (CT 303 in VMID list)

### terraria-server (CT 304)

- **Container:** tModLoader on port 7777 (192.168.30.216)
- **Panel:** Pelican Wings
- **CPU Management:** cpulimit 1.5 (hard ceiling at Proxmox hypervisor level) set via `pct set 304 --cpulimit 1.5`
- **Daily Cron:** `0 4 * * * pct exec 304 -- bash -c "kill $(pgrep -f tModLoader)" 2>/dev/null` — restart process daily at 4am to clear CPU leak
- **Issue Fixed (May 20, 2026):** tModLoader idles high (70%+) and ramps to 97%+ CPU over hours with heavy mods. cpulimit + daily cron restart resolves the issue.
- **Panel CPU Limit:** Set to 150% (= 1.5 cores, matching Proxmox cpulimit) in Pelican panel
- **Backup:** Included in backup-daily vzdump job (CT 304 in VMID list)

### enshrouded-server (CT 306)

- **Container:** Enshrouded dedicated server in Pelican Panel (Docker, Proton-wrapped)
- **IP:** 192.168.30.218
- **Port:** 15636 (LAN: 192.168.30.218:15636)
- **Panel:** Pelican Wings (panel.najhin-gaming.com)
- **Config file location:** `/home/container/enshrouded_server.json` inside Docker container
- **Server log location:** `/home/container/logs/enshrouded_server.log` — reliable for verifying real player activity
- **Server password:** Auto-generated as `4TGU-MHE` in userGroups[0].password field (not yet fixed to be genuinely blank — carried over pending item)
- **External connectivity status (July 5, 2026):**
  - **Prior issue:** UDP forwarding blocked by double-NAT topology — root cause identified and resolved via ISP bridge mode
  - **LAN connectivity:** Confirmed working via Pelican panel (reachable from internal network)
  - **External connectivity:** NOT YET RETESTED post-bridge-mode. Double NAT eliminated; DNS records still point to old IP (202.184.35.79) and require update to new IP (202.184.116.231). Requires manual retest with packet capture once DNS updated.
  - **Friend connectivity method:** Steam relay/friend-invite mechanism confirmed working (masks that direct UDP forwarding was previously broken) — now should work directly via fixed port forwarding with bridge mode
- **Note on Pelican stats:** Panel's CPU/Memory/Disk display cosmetically broken for this Proton-wrapped egg (shows 0%/0 bytes/Unavailable regardless of actual server activity) — use server log for ground truth
- **Backup:** Included in backup-daily vzdump job (CT 306 now in VMID list as of August 1, 2026)

### palworld-server (CT 307)

- **Container:** Palworld dedicated server in Pelican Panel (Docker, Proton-wrapped)
- **IP:** 192.168.30.219
- **Port:** 8211 UDP (LAN: 192.168.30.219:8211)
- **Panel:** Pelican Wings (panel.najhin-gaming.com)
- **Deployed:** July 16, 2026 (new gaming platform expansion)
- **Configuration:**
  - **PalWorldSettings.ini location:** `/home/container/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini`
  - **World save location:** `/home/container/Pal/Saved/SaveGames/0/6376E22F11CD4588A54E2EB0E7B1CD1F/` (note: no WorldOption.sav present — settings remain in ini file only)
  - **PublicIP field (for external connectivity):** 202.184.116.231 (current as of September 23, 2026; subject to WAN IP changes — see Network Architecture WAN IP Instability section)
  - **Pelican egg variable "Public IP":** Now has User Editable + User Viewable permissions enabled (fixed July 17 — previously unchecked, blocking manual IP updates via Startup tab)
  - **PalCaptureRate:** Default (1.000000) — was tested at 100.000000 but reverted to default after testing

**Critical Known Issues (July 16-17, 2026):**
- **PalWorldSettings.ini requires unbroken single-line format for OptionSettings block:** Pelican's web file editor (Files tab) introduces line breaks into `OptionSettings=(...)`, corrupting the file and causing Palworld to silently ignore ALL OptionSettings (fallback to defaults). Root cause identified: Pelican doesn't preserve multiline field formatting.
  - **Safe editing method established:** Use `pct exec 307 -- sed` commands from the Proxmox host to edit ini files directly, never use the Pelican web file editor.
  - **Example (safe):** `pct exec 307 -- sed -i 's/PalCaptureRate=.*/PalCaptureRate=1.000000/g' /home/container/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini`
  
- **Pelican panel's file Download function (Files tab → Archive → Download) returns 404 "resource not found" error:** Reproducible even when accessing the panel via internal IP (192.168.30.217), ruling out Cloudflare/tunnel as the cause. Root cause not yet identified (suspected Wings/node FQDN mismatch in signed URL generation or archive path issue). Not debugged further this session (time-sensitive save backup was instead completed via `pct exec` + `pct pull` method from Proxmox host).
  - **Reliable backup workaround:** `pct exec <CTID> -- tar -czf /tmp/backup.tar.gz -C <path> <folder>` followed by `pct pull <CTID> /tmp/backup.tar.gz <destination>` — bypasses Wings/panel entirely, works directly at Proxmox host level.

- **WAN IP instability requires manual PublicIP updates:** Five IP changes observed to date due to TIME Fiber PPPoE renegotiation. Each IP change requires: (1) update PalWorldSettings.ini PublicIP field, (2) update Cloudflare DNS records, (3) potentially restart Palworld. **DDNS automation elevated to high priority** to automate PublicIP and DNS updates on IP change.

- **In-game stutter with 3 concurrent players:** Traced to nightly vzdump backup job (sequential all-container, currently ~02:11 AM) running concurrently with gameplay, causing CPU/disk I/O contention. NOT a Palworld resource or hardware capacity issue.
  - **Mitigation in planning:** Consolidate Palworld restart schedule, Terraria restart schedule, and vzdump backup job into a single off-peak maintenance block (time window not yet finalized).

**Backup information:**
- SaveGames backup created July 17, 2026 and stored at: `/mnt/hdd-backup-1/palworld-backup-20260716/palworld-save-backup.tar.gz` (22M, verified contents include Level.sav, LevelMeta.sav, Players/ data for world 6376E22F11CD4588A54E2EB0E7B1CD1F)
- Pre-fix (corrupted, 13-line) PalWorldSettings.ini backed up as `PalWorldSettings.ini.bak` inside the container (pending cleanup/deletion once stability confirmed)
- **Backup:** Included in backup-daily vzdump job (CT 307 now in VMID list as of August 1, 2026)

**External connectivity status (as of September 23, 2026):**
- **LAN connectivity:** Confirmed working via Pelican panel
- **External connectivity:** NOT YET RETESTED post-bridge-mode and post-ini-fix. Friend connectivity mechanism (Steam relay/invite) presumed working. Direct port forwarding (8211 UDP) should now work with PublicIP correctly set, but not yet verified with packet capture.

### ollama-gpu (VM 400)

- **Type:** KVM VM with PCIe passthrough — NOT an LXC
- **GPU:** RX 6700 XT 12GB (0d:00.0) + audio (0d:00.1) passed through
- **PCIe config:** hostpci0=0000:0d:00.0,pcie=1,rombar=0 hostpci1=0000:0d:00.1,pcie=1
- **OS:** Ubuntu 22.04
- **SSH:** `ssh vm400` (via Tailscale config alias on Minimoon) — user `muzakkir`, key-based auth
- **Disk:** Expanded from 56GB to 86GB (May 21, 2026) via Proxmox resize + LVM extension (device: /dev/vda, not /dev/sda). 34GB free post-expansion.
- **Ollama models:** qwen3:14b (primary, 9.3GB), qwen3.5:latest (secondary), nomic-embed-text (768 dims). Removed (May 21-22): gemma3:4b, gemma3:12b, phi4-mini, llama3.2:latest (all hallucinated factual data in testing).
- **Coding models (research phase, Sept 22):** Shortlisted Qwen2.5-Coder-14B-Instruct and DeepSeek-Coder-V2-Lite-16B for future EMIYA execution engine (12GB VRAM budget); ruled out Qwen3-Coder-30B-A3B (MoE model, too large). A/B testing pending once one is selected for deployment.
- **Open WebUI:** Docker container, connects via 172.17.0.1:11434
- **Qdrant:** Port 6333 (REST), 6334 (gRPC), storage at /opt/qdrant/storage
- **Backup:** Included in backup-daily vzdump job (VMID 400 in VMID list); VM 400 backup size trending upward: 32G (May) → 69G (July) → 85-90G (August/September)
- **Note:** VM 400 actual disk usage growing in real time, not a backup-mechanism artifact. Worth monitoring for continued growth.

### Gaming Servers

- **Pelican Panel (CT 305):** Next-gen game server panel (192.168.30.217), SQLite + Redis + PHP 8.3, access: panel.najhin-gaming.com
- **Minecraft Server (CT 303):** Paper 1.21.4 on port 25570 (192.168.30.215), Pelican Wings
- **Terraria Server (CT 304):** tModLoader on port 7777 (192.168.30.216), Pelican Wings
- **Enshrouded Server (CT 306):** Dedicated server on port 15636 (192.168.30.218), Pelican Wings (backed up as of August 1, 2026)
- **Palworld Server (CT 307):** Dedicated server on port 8211 UDP (192.168.30.219), Pelican Wings, deployed July 16, 2026 (backed up as of August 1, 2026)
- **Windrose (CT 302):** Deployed at /opt/windrose, 4 max players, Medium difficulty, invite code NAJHINWINDROSE

---

## 📊 Monitoring & Alerting

### node_exporter (Host-Level, Proxmox bare metal)

**Fixed July 14, 2026:** Default Debian package `prometheus-node-exporter.service` shipped with `--collector.filesystem.mount-points-exclude` regex containing `mnt` in the excluded path list, making ALL mountpoints under `/mnt` invisible to Prometheus since at least May 16, 2026. This caused the `MountpointMissing_hddbackup1` alert to remain stuck in "active" state for 8 days despite the drive being mounted and healthy.

**Details:**
- **Correct systemd unit name:** `prometheus-node-exporter.service` (Debian package naming — NOT `node_exporter.service`)
- **Binary location:** `/usr/bin/prometheus-node-exporter`, runs as user `prometheus` (uid 108, gid 110)
- **Listens on:** :9100
- **Config file:** `/etc/default/prometheus-node-exporter`

**Fix Applied:**
- Edited `/etc/default/prometheus-node-exporter` ARGS parameter
- **Before:** `^/(dev|proc|run|sys|mnt|media|var/lib/docker/.+|var/lib/containers/storage/.+)($|/)`
- **After:** `^/(dev|proc|run|sys|media|var/lib/docker/.+|var/lib/containers/storage/.+)($|/)`
- Removed `mnt` from the exclusion list to expose `/mnt/hdd-backup-1`, `/mnt/hdd-backup-2`, `/mnt/ssd-storage`, and `/mnt/pve/kinmoon-smb`
- Restarted service: `systemctl restart prometheus-node-exporter.service`
- Verified metrics returned: `curl localhost:9100/metrics | grep node_filesystem`
- Prometheus query confirmed all three /mnt mountpoints now reporting
- Alertmanager alert (fingerprint fc914336cd6ed15b) auto-resolved

**Note on per-container exporters:** Multiple other `/usr/local/bin/node_exporter` processes visible via `ps aux` on the host (uid 100000, 101000, etc.) are per-LXC-container exporters visible through host's process namespace due to unprivileged container UID mapping — these are NOT the host-level exporter and restarting them has no effect on host-level Prometheus alerts.

### CPU Usage Alerts — iowait Misreporting (July 23, 2026)

**Discovered:** CT 205 (Alertmanager, 192.168.30.205:9100) fired a CRITICAL "CPU usage 100%" alert at ~03:46-03:47 on July 20, 2026, and re-triggered on July 21-22. Root cause is **NOT genuinely high compute load.**

**Root Cause (Verified):**
- Prometheus's CPU-usage alert rule queries the sum of all non-idle CPU time: `100 - (idle % + steal %)`
- This calculation includes **iowait** (`%wa` in `top`) as non-idle time, conflating I/O wait with actual compute load
- Actual investigation via `top` showed **100.0% `%wa` (I/O wait) and 0.0% `us`/`sy` (user/system compute time)**
- **Diagnosis:** Pure I/O bound, zero compute load — alert fired due to an incorrect classification in the query logic

**Correlation to Backup Job:**
- `backup-daily` vzdump job runs 02:00 daily, typically completes by ~03:33-03:36
- CT 205 alert fired ~03:46 (after job completion), indicating lingering I/O pressure
- Backup destination: kinmoon-smb CIFS share at 93-94% capacity
- **Conclusion:** Prolonged I/O wait from backup process writing to near-full network share, persisting ~10+ minutes past nominal job completion time

**Alert Rule Status:**
- Not yet modified this session — documented as diagnosis only
- Future fix should exclude `%wa` from CPU-usage alert calculation or create separate "I/O pressure" alert

**Action for future sessions:** Consider updating the Prometheus CPU-usage alert rule to exclude iowait, or create a separate `node_load_high` or `disk_io_wait` alert for I/O-bound conditions.

### Alertmanager (CT 205) — Critical-Alerts Telegram Delivery (Fixed September 23, 2026)

**Incident:** Critical alert delivery via Telegram silently failed on September 21, 2026 (Kuromoon host-freeze event). Investigation revealed the critical-alerts Telegram bot token (`/etc/alertmanager/alertmanager.yml` inline plaintext) had been invalid for at least since Sept 21 — exact date token expired unknown, but failure confirmed. Alert correctly reached Discord receivers (all 3: default, warning-alerts, critical-alerts) but only Discord, leaving no Telegram notification during infrastructure outage.

**Root Cause (Verified):**
- **Token storage method:** Alertmanager's `alertmanager.yml` contained Telegram `bot_token` as inline plaintext string
- **Monitoring gap:** No alerting exists on credential expiry/validity itself — only on Alertmanager service health (which passed because the service was running; it was only the auth that failed)
- **Discovery method:** Followed up on Sept 22's open item "verify n8n webhook-validation secrets are actually checked before treating as Bypass" → audit of all critical credentials → discovered multiple recently-failed tokens across the infrastructure (DDNS: 2.5 months, backup scheduler: 34 days, this token: unknown but at least since Sept 21)

**Fix Applied (September 23, 2026):**
1. **New dedicated Telegram bot:** Created fresh bot via BotFather `/newbot` (distinct from existing Jeanne Alter bot) to guarantee unique token by construction, eliminating any collision risk and providing clean separation of "agent chat" vs. "infrastructure alerts"
2. **Token storage migration:** Moved from inline plaintext in `/etc/alertmanager/alertmanager.yml` to permissioned secrets file `/etc/alertmanager/secrets/telegram_bot_token` (owner `alertmanager:alertmanager`, mode 600, directory mode 700)
3. **Config update:** Modified alertmanager.yml to reference secrets file via `bot_token_file: /etc/alertmanager/secrets/telegram_bot_token` (supported in Alertmanager 0.27.0+, verified via `amtool check-config`)
4. **Service reload:** Applied config via `systemctl reload alertmanager` (no restart needed, hot-reload supported)
5. **End-to-end verification:** Generated two synthetic critical alerts via `amtool alert add` with FIRING and RESOLVED states — confirmed **4 total Telegram messages received** (one each for FIRING, one each for RESOLVED state, two alerts total), confirming bidirectional delivery, deduplication, and grouping logic all working correctly

**Alertmanager Config Details:**
- **Service location:** CT 205 (192.168.30.205)
- **Config file:** `/etc/alertmanager/alertmanager.yml` (unchanged otherwise)
- **Secrets storage:** `/etc/alertmanager/secrets/telegram_bot_token` (new, perms 600, owner alertmanager:alertmanager)
- **Version confirmed:** Alertmanager 0.27.0
- **Receiver:** critical-alerts (routes any alert with `severity = critical`)
- **Chat ID:** 518832696 (Telegram user ID, unchanged)
- **Grouping:** `group_by: ['alertname', 'cluster', 'service']` — incidents with same alertname+cluster+service grouped into single message

**Testing notes:**
- First synthetic alert delivery initially failed with Telegram error "chat not found" (400) — resolved once the user started/interacted with the new bot in Telegram (Telegram requires user-initiated chat creation before bot can send messages). Alertmanager's built-in retry logic then automatically succeeded once this precondition was met.
- Subsequent tests succeeded immediately without retry

**Decision Rationale (Documented in decisions.md):**
- **Why a separate, dedicated bot?** Fresh bot guaranteed distinct by construction; avoids any risk of collateral breakage if Jeanne Alter's bot were reused (e.g., if that bot's token expires later, infra alerts would also break). Explicit design separation also makes intent clearer.
- **Why `bot_token_file` instead of inline swap?** Credential had already failed silently once with zero monitoring; extra effort on migration was justified given the recurring pattern this week of dead credentials going unnoticed for weeks (DDNS: 79 days, backup scheduler: 34 days, this token: unknown minimum). File-based storage enables future credential health checks (validate file exists, periodically test token validity, alert on expiry approaching).
- **Why not re-use the original token?** Token's exact expiry date unknown; new token eliminates any ambiguity and is trivially low-cost with BotFather.

### Storage Status (Verified September 23, 2026)

| Mount            | Device     | Size  | Used | Filesystem | UUID                             | Status |
|------------------|------------|-------|------|------------|----------------------------------|--------|
| /mnt/hdd-backup-1 | /dev/sdb1 | 5.5T  | 18%  | ext4       | 5593849e-8ee9-4d6c-b1bc-3e35650e05fb | ✅ Mounted |
| /mnt/hdd-backup-2 | /dev/sdc1 | 6.9T  | 12%  | ext4       | 5cd7f4a4-7510-42eb-9f35-be49f6a10686 | ✅ Mounted |

**hdd-backup-1 Storage Role (Clarified July 14):**
- **Primary live storage** for Nextcloud data directory (bind mount at `/mnt/ncdata`)
- **Not a backup copy** — misconception cleared
- Nextcloud data written to this drive daily
- Kinmoon NAS does NOT receive a nightly rsync copy (previous documentation was inaccurate) — the actual mechanism is Proxmox's native `backup-daily` vzdump job writing directly to kinmoon-smb CIFS share

**SATA Link Incident (July 6, Resolved):**
- **One-time event:** `ata2: SATA link down` observed 2026-07-06 19:06-19:07, recovered same session, filesystem remounted 20:35:38
- **Status:** Not an ongoing/recurring hardware fault as of July 14 dmesg review
- **Physical fix status:** SATA cable/port swap remains deferred pending physical access and budget availability — not a current technical blocker, infrastructure is functional

---

## Kinmoon NAS (UGREEN DXP2800) — Storage Pool 1 Status (Updated September 23, 2026)

### Storage Architecture

- **Model:** UGREEN DXP2800 (2-bay NAS, UGOS 4.3.0 operating system)
- **IP Address:** 192.168.10.100
- **Hostname:** KinMoon (per SMB "How to use" reference in UGOS Control Panel)
- **SSH Access:** Username `Muzakkir` (capital M), key-based authentication via `~/.ssh/id_ed25519_homelab` from Minimoon

### Storage Pool 1 (RAID 1) — Status: Normal/Healthy

**Current state (as of 2026-09-23):**
- **Array UUID:** `5bb187d0:b14f67a3:9f4d8ab9:16d079f8` (new, pool recreated from scratch on July 31)
- **Hard Drive 1 (Bay 1):** 3TB Seagate IronWolf (NAS-rated, CMR, SN: W3FXXXZY)
- **Hard Drive 2 (Bay 2):** 3TB Seagate ST3000DM008 (SN: Z505511Z)
- **RAID Status:** Normal/clean, both drives `active sync`, zero spares
- **Volume 1:** ext4, 2.6TB capacity, Normal status, fully operational
- **Shared folder:** `proxmox-backups` with Read/Write for user Muzakkir, Recycle Bin enabled (Admin only)
- **GRUB fix:** `libata.force=3.0Gbps` applied to both `/boot/EFI/debian/grub.cfg` (live) and `/boot/EFI/debian/grub.am` (template). Zero `WRITE FPDMA QUEUED` errors since applied. Fix persists across reboots.
- **Storage capacity status (September 23, 2026):** **73.76% used (1.9TB / 2.6TB, 752GB available)** — capacity crisis substantially resolved this session (was 96% / 118GB free pre-cleanup)

### Capacity Crisis Resolution (September 22, 2026)

**Root cause identified:** UGOS `#recycle` folder (Recycle Bin) accumulated 774GB of stale backup chunks pruned/overwritten by Proxmox's own `prune-backups` retention policy (keep-daily=7, keep-weekly=4). The recycle bin was double-counting space already logically freed on-disk as `backup-daily` job ran nightly.

**This is the same pattern first diagnosed July 23, 2026**, which recurred after the July 31 pool rebuild recreated the recycle bin with no retention limit set.

**Resolution (September 22, 2026):**
- Manually purged the recycle bin backlog via `sudo rm -rf "/volume1/proxmox-backups/#recycle"/*` on Kinmoon
- Capacity immediately dropped from **96% (2.5TB / 2.6TB, 118GB free)** to **73.76% (1.9TB / 2.6TB, 752GB available)**
- UGOS Volume 1 now at healthy capacity with ~750GB buffer before backup job stalls

**Pending action (CRITICAL to prevent recurrence):**
- Set auto-purge retention policy on UGOS shared folder `proxmox-backups` → Recycle Bin settings (7-14 days expiration)
- This was NOT done this session — without it, the `#recycle` folder will silently refill over the next 34-day backup cycle exactly as it did between July 31 and Sept 22
- **Estimated time to implement:** 5 minutes in UGOS Control Panel

### Hard Drive Inventory (September 23, 2026)

**Hard Drive 1 (Bay 1):**
- **Model:** 3TB Seagate IronWolf (NAS-rated, CMR)
- **Replacement:** Installed July 24, 2026 (was original SMART-failed drive)
- **SMART Status:** Healthy (2.7TB capacity, "Normal" UGOS status)
- **Role:** Active member of RAID 1 mirror

**Hard Drive 2 (Bay 2):**
- **Model:** 3TB Seagate ST3000DM008 (Desktop/mixed-use class, not NAS-optimized)
- **SMART Status:** Healthy (Reallocated Sector Count 99/10 threshold, well below failure point)
- **Temperature:** 46°C (normal)
- **Issue Fixed (July 24-31, 2026):** Deterministic `WRITE FPDMA QUEUED` error at boot due to SATA link timing issue at full 6.0Gbps
  - **Root Cause:** Some SATA drives have narrow signal tolerance at full 6.0Gbps, causing intermittent command timeouts misidentified as drive failures
  - **Fix Applied:** Kernel boot parameter `libata.force=3.0Gbps` in GRUB forces negotiation to 3.0Gbps, eliminating the error
  - **Fix Locations:** `/boot/EFI/debian/grub.cfg` (live config) and `/boot/EFI/debian/grub.am` (template, regenerates grub.cfg on OS updates)
  - **Status:** Zero errors post-fix; confirmed stable across full data restore operation and nightly backups since July 31
- **Historical note:** Drive 2's same `WRITE FPDMA QUEUED` signature was intermittently detected since March 2026 (at different times from Drive 1), suggesting original Hard Drive 1 "failure" may have been accelerated by this same SATA link instability rather than pure media wear
- **Role:** Active member of RAID 1 mirror

### UGOS Firmware Bug (Documented)

**Daemon:** `storage_serv` (UGOS Storage Manager service)
**Issue:** Mishandles `RebuildFinished` event during active RAID resync, throwing `strconv.Atoi: parsing "-": invalid syntax` error and causing kernel-level `md: recover interrupted`
**Symptoms:** Rebuild process halts at random progress points despite no hardware/I/O error
**Reproduced:** 2+ times during July 30-31 recovery attempts
**Status:** Known issue (documented in UGREEN DACH community forum, 30+ page thread with multiple user reports)
**Mitigation:** Stopped `storage_serv` during rebuild attempts (worked for that variable, but fourth rebuild failed even with service stopped, suggesting multiple independent failure modes)
**Upstream reporting:** Worth reporting to UGREEN with full logs and reproduction case

### Emergency Backup Copy (Safety Net)

**Location:** `/mnt/hdd-backup-2/kinmoon-emergency-backup/` on Kuromoon
**Size:** 1.2TB (matches restored data)
**Status:** Retained post-restore as safety net
**Action:** Delete once Kinmoon's rebuilt array is trusted long-term (not urgent, pending decision next session)

### Backup Job Status

**`backup-daily` vzdump job:**
- **Status:** Operational, confirmed healthy as of September 22, 2026
- **Schedule:** 02:00 daily
- **Destination:** kinmoon-smb CIFS share (mounted at `/mnt/pve/kinmoon-smb`)
- **Retention:** keep-daily=7, keep-weekly=4 (zstd compression)
- **VMID list:** `/etc/pve/jobs.cfg` confirmed to include: `201,202,203,204,205,206,207,208,211,213,214,220,221,222,223,302,303,304,305,400,306,307`
  - **CT 306 (Enshrouded) and CT 307 (Palworld) now included** — closes the backup-coverage gap documented since July 23, 2026. Exact date these two were added is unknown/undocumented.
  - **All 22 LXC containers + 1 KVM VM have backup coverage as of this session**
- **CRITICAL DISCOVERY:** `backup-daily` scheduler silently ceased job-start attempts for 34 days (Aug 18 - Sep 21, 2026), confirmed via `journalctl -u pvescheduler` showing zero entries in that window. This is NOT a job-failure condition (which would appear as failed task records) but a complete scheduler silence — every VMID had NO backup coverage for over a month.
  - **Root cause unconfirmed;** leading theory ties to the same host instability documented in Sept 21 freeze (pvescheduler service only confirmed alive as of Sept 21 20:38, the documented hard-reboot timestamp)
  - **Back half explanation:** Sept 3 journald gap covers Sep 3-21 (18 days)
  - **Front half unexplained:** Aug 18-Sep 3 (~16 days) remains undocumented
- **Tonight's recovery:** `backup-daily` run on Sep 22 02:00-03:45 completed successfully for all 22 VMIDs + 1 VM
  - **VM 223 (Langfuse) was the long pole:** 58 minutes total runtime, CPU-bound tar process (likely high file count — verified via `ps aux` showing active `R`-state process with climbing CPU time, not a hang)
- **First scheduled run post-discovery:** Sep 23, 02:00 (pending verification)

### SSH Access & Credentials (September 23, 2026)

**Username:** `Muzakkir` (capital M, case-sensitive)
**SSH key:** `~/.ssh/id_ed25519_homelab` (ed25519, from Minimoon)
**SSH config alias on Minimoon:** `kinmoon` (resolves to kinmoon.0x0.dev via Tailscale Magic DNS)
**Prerequisites:** UGOS "Personal Folder" feature must be enabled with an assigned volume location (Volume 1) before OpenSSH will authenticate — this sets up the home directory path needed by OpenSSH. Without a home directory assigned, SSH login fails silently even with correct password and Admin role.
**Dependent credentials:**
- Proxmox `/mnt/pve/kinmoon-smb` CIFS storage auth: Updated via `pvesm set kinmoon-smb --username Muzakkir --password`
- Vaultwarden entry: (if one exists, recommend updating to match current password)

---

## Kuromoon Host Incident — September 21, 2026 (Incident Resolved; Watchdog Deployed)

### Timeline & Symptoms

**2026-09-21 ~19:00 (afternoon, exact time unclear):**
- System became unresponsive at application layer: pveproxy, sshd, and CT 203 (Grafana) all accepted TCP connections but never completed protocol/application responses
- ICMP ping responses remained normal (host was answering ICMP but not TCP/application-level requests)
- Proxmox web UI (192.168.10.5:8006) completely inaccessible
- Initial troubleshooting: assumed Kuromoon hypervisor host itself had become unresponsive

**2026-09-21 20:37:**
- Hard power-cycle of Kuromoon performed to restore service
- Service recovered; host booted normally
- CT 220 (nextcloud) failed to autostart post-reboot with error "Failed to run lxc.hook.pre-start for container '220'" / "TASK ERROR: startup for container '220' failed"
- Manually started CT 220; container came up successfully once host storage settled

### Root Cause Analysis (Inconclusive)

**Forensic investigation performed:**
- **ZFS pool health:** `zpool status` confirmed rpool (NVMe mirror) fully healthy — ONLINE, zero errors, last scrub clean (August 9, 2026)
- **Disk space:** Root disk usage negligible (1% used, 676GB free)
- **Hardware fault:** No hardware fault identified on Kuromoon itself
- **Monitoring data:** Prometheus/Grafana (both hosted on Kuromoon) were down during the freeze, so pre-incident resource metrics unavailable
- **Kernel logs:** dmesg buffer empty post-reboot (cleared by reboot); pstore (`/sys/fs/pstore`) also empty
- **journald history:** Discovered journald silently halted on 2026-09-03 23:53 (18 days prior) with unknown cause — system continued operating normally throughout the interim, but logging gap means no incident logs are available for the actual freeze window. This gap also coincides with the back half of the 34-day `backup-daily` scheduler silence (Aug 18 - Sep 21).
- **Conclusion:** Exact trigger cannot be conclusively proven; no surviving diagnostic evidence

### Leading Hypothesis (Unproven)

**I/O stall related to kinmoon-smb CIFS mount at 96% capacity (2.5TB / 2.6TB), combined with previously-documented Kinmoon Hard Drive 1 failing SMART status (July 23, 2026):**
- kinmoon-smb is the destination for Proxmox's `backup-daily` vzdump job (runs 02:00 daily)
- CIFS shares running near-full can cause severe I/O stalls when write operations block on "out of space" errors
- If Kuromoon's host processes attempted I/O to the kinmoon-smb mount during/after backup, and the CIFS server became unresponsive or the mount went into "bad" state, the system could experience a host-wide I/O hang affecting all processes waiting on filesystem operations
- The timing (incident ~19:00 local time) doesn't align with the 02:00 backup job, but lingering effects or a delayed I/O queue flush could have triggered hours later
- This is a **plausible but unproven** theory

**Alternative hypothesis:** The 34-day `backup-daily` scheduler silence and Sept 3 journald gap suggest the host may have been cycling in/out of a bad state for weeks, with the Sept 21 incident being the point where the condition became severe enough to cause a full application-layer freeze. If Kinmoon's NAS was periodically becoming unresponsive (due to the CIFS share being at 96% capacity + Hard Drive 2's intermittent SATA errors pre-GRUB-fix), the Kuromoon host could have been experiencing transient I/O stalls long before Sept 21.

### CT 220 (Nextcloud) Autostart Recurring Pattern

**Confirmed pattern across 4+ reboots:**
- 2026-05-16: Failed autostart
- 2026-07-06: Failed autostart
- 2026-08-11: Failed autostart
- 2026-09-21: Failed autostart

**Failure mode:** "Failed to run lxc.hook.pre-start for container '220'" / "TASK ERROR: startup for container '220' failed"

**Root cause hypothesis:** Race condition where the `/mnt/hdd-backup-1` bind-mount source (backing CT 220's `/mnt/ncdata`) isn't ready yet when Proxmox's bulk autostart sequence reaches CT 220.

**Why it succeeds when manually started:** By the time manual intervention happens, host storage has fully settled and the bind-mount is available.

**Mitigation pending:** Set startup delay on CT 220 via Options → "Start at boot" (deferred to future session).

### journald Silent Halt (September 3, 2026)

**Observed:** journald stopped writing new entries on 2026-09-03 23:53 (boot id `6df864f9f83d4dcfa9e455cef9134894`), despite the system remaining up and in normal use for another 18 days until tonight's reboot.

**Cause:** Unknown — no error in journald logs preceding the halt, no apparent disk space or permission issue.

**Impact:** Loss of 18 days of system logs, making forensic analysis of tonight's incident impossible. The back half of the 34-day `backup-daily` scheduler silence (Sep 3-21) is completely unlogged.

**Recommendation:** Monitor for recurrence; investigate if it happens again (consider checking journald socket, disk space at the time, selinux denials, etc.).

### Watchdog Deployment (Phase 37 Component 1, September 22, 2026 — COMPLETE)

**Deployment:** Pi-hole (`/usr/local/bin/kuromoon-watchdog.sh`)
**Trigger:** Cron every 2 minutes (`*/2 * * * *`)
**Monitoring approach:** HTTPS response check (not ICMP) — tonight's Sept 21 incident proved ICMP ping remains healthy during full application-layer freeze

**Logic:**
- Check Proxmox GUI response on LAN path (192.168.10.5:8006)
- Check Proxmox GUI response on Tailscale path (100.89.254.28:8006)
- If both fail 3 consecutive times (~6 min): send 🔴 failure alert via Telegram Bot API
- When response returns: send ✅ recovery alert via Telegram Bot API
- Track state in `/var/tmp/kuromoon-watchdog-state` and `/var/tmp/kuromoon-watchdog-failcount`

**Alert delivery:** Direct Telegram Bot API call (not ntfy) — ntfy runs on Kuromoon itself, so it would be unavailable during the very failure being reported

**Testing:** Fully tested end-to-end Sept 22 — stopped `pveproxy` on Kuromoon, confirmed 🔴 alert fired after 3 checks; restarted `pveproxy`, confirmed ✅ recovery alert fired correctly

---

## Cloudflare Tunnel & Public Hostname Routing (Updated September 23, 2026)

### Tunnel Configuration

**Tunnel name:** `homelab-tunnel`
**Connector location:** CT 220 (Nextcloud container), systemd service `cloudflared`
**Installed:** 2026-03-07
**Design:** Single tunnel/connector hosts all public routes (not one connector per service)

### Published Application Routes & Access Policies

**Total Access applications (September 23, 2026):** 17 applications

| Hostname | Application | Backend | Service Type | Access Policy | Status |
|---|---|---|---|---|---|
| panel.najhin-gaming.com | panel | http://192.168.30.217:80 | HTTP | Bypass — Operator role (Minimoon automation) | ✅ |
| langfuse.najhin-gaming.com | langfuse | http://192.168.30.223:3000 | HTTP | Allow — Email OTP | ✅ |
| finance.najhin-gaming.com | finance | http://192.168.30.224:8080 | HTTP | Allow — Email OTP | ✅ |
| cloud.najhin-gaming.com | NextCloud | http://192.168.30.220:80 | HTTP | Path-scoped: `/remote.php`, `/ocs`, `/status.php`, `/public.php`