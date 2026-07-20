# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** July 17, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Architecture redesign complete. 7-layer model finalized. Midas CFO Agent, MERLIN Reminders, Daily Note Creator, Morning Briefing, Health Tracking all active. Obsidian Phases 22.1, 22.2, and 22.8B complete. Phase 24.7 (ntfy), 24.1 (Firefly III), and 24.8 (Langfuse) complete. Nextcloud Deck integration complete with Da Vinci project management. Hardware upgraded to 128GB DDR4 with 3-tier storage architecture. 21 LXC containers + 1 KVM VM deployed. Da Vinci Stage 2 (RAG) complete with Qdrant + nomic embeddings. Phase 7E (Extended Memory) complete with conversation archival. Pelican panel migration complete with Minecraft/Terraria split. Da Vinci Documentation Pipeline rebuilt May 19, 2026 with 3 separate Haiku API calls and immediate cost logging. Concurrency protection and inbox watcher schedule finalized May 18-19, 2026. Infrastructure troubleshooting complete May 20, 2026: CT 207 Promtail crash loop resolved (53,649 restarts), CT 304 tModLoader CPU leak fixed with cpulimit + daily cron. Phase 16.4 (Documentation Pipeline Expansion — 8 files) complete May 21, 2026: expanded from 3-file to 8-file sequential Haiku chain. decisions.md promoted to Phase 2 priority. Pipeline tested and verified May 21, 2026: 3 separate API calls, immediate cost logging, per-file system prompts, all 8 files successfully pushed to GitHub. Phase 24.8 (Langfuse) wired to Da Vinci Update Pipeline May 21, 2026: single trace (da-vinci-update) with 8 child generations logged per run. Traces confirmed in ClickHouse and accessible via API; UI trace list has known v3 self-hosted bug where traces don't appear in list view (1-hour aggregation delay). VM 400 disk expanded from 56GB to 86GB (LVM thin resize, 34GB free). Model testing complete: qwen3:14b confirmed as primary (honest about limitations), gemma3 + phi4-mini removed (confident hallucination). AI-CONTEXT max_tokens bumped to 25000 (was hitting ceiling). Langfuse CT 223 LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES set to true. Phase 24.9 (Personal Knowledge System) complete May 22, 2026: Langfuse UI trace list working (1-hour analytics delay resolved), Gilgamesh wired to Langfuse, Da Vinci Personal Knowledge gateway deployed, muzakkir-profile.md created and indexed in Qdrant (1,736 chunks), Gilgamesh successfully recalling personal facts via RAG, Knowledge Indexer updated to include 04-personal/ and expanded folder set (90 files indexed). Documentation audit (May 22, 2026) — corrections only: ROADMAP.md had 5 archived phases still showing as active; current-state.md had hallucinated hardware (EPYC 5645/256GB/RTX 4070 vs actual Ryzen 5 5600X/128GB/RX 6700 XT); agents.md MERLIN/Midas status incorrect; Knowledge Indexer folder list inconsistent across files. All corrections applied. Phase 24.10 (Triggered Qdrant Re-indexing) complete May 25, 2026: Post-write webhook added to Da Vinci Personal Knowledge gateway. Partial reindex (/davinci-reindex-personal, ~1s) triggers after muzakkir-profile.md writes. Full daily reindex (3am, ~21s) still operational. Phase 52 (DOCP Memory Optimization) complete May 2026. Web Search (Gilgamesh) deployed May 25, 2026: Firecrawl API integrated. Keyword-based intent detection, search results injected into context, search queries force-routed to Haiku (local models cannot follow injected context). Firecrawl credits per search: 2. All changes verified May 25, 2026. **Gaming platform expanded July 5, 2026: Enshrouded server (CT 306) deployed in Pelican Panel. Network architecture migrated from double-NAT (ISP router + pfSense) to true bridge mode on Huawei HG8145B7N, eliminating all double-NAT routing conflicts permanently. pfSense WAN reconfigured from DHCP to PPPoE. Public IP changed to 202.184.101.136 (was 202.184.35.79). TP-Link EAP610 AX1800 access point (SSID A21-22A) deployed and configured — root cause of AP failure identified and fixed: TL-SG108E switch ports 7-8 were on legacy VLAN 1, not VLAN20_MAIN. All 21 containers + 1 VM running. DNS records require update. Enshrouded external UDP connectivity not yet retested post-bridging.** **Agents ecosystem renamed from "Kuromoon" to "Chaldea" (July 8, 2026) — Kuromoon now refers to physical hardware only, Chaldea is the agents system layer.** **Gilgamesh renamed to Jeanne Alter ("The Corrupted Ruler"), pending full propagation across bot, n8n, Telegram, docs (July 8, 2026).** **Career timeline updated (July 8, 2026): September 2026 job-transition deadline dropped. Cloud/DevOps roles pursued at slower pace. Chaldea project reframed as indefinite, long-term. hdd-backup-2 Prometheus alert removed (copy-paste bug fixed, then intentionally disabled per user decision). Duplicate muzakkir-profile.md.md file exists in Nextcloud. Jeanne Alter assistant messages not saving to Data Table (silent conversation memory breakage).** **Documentation audit completed (July 9, 2026): Cross-session gap analysis identified seven previously-undocumented items from past sessions: Interest-Capture Loop concept, Four Blind Spots analysis (time vs priorities, docs drift, bus factor, skill-market fit), two domains tracked (najhin-gaming.com for gaming, muzakkir.tech for portfolio), two previously-named agents (Cu Chulainn renamed from Guardian May 16, Scathach prioritized May 16, Nightingale health concept May 17), and Phase 27 Domain Migration & Infrastructure Audit. Cu Chulainn and Scathach rename propagation pending. muzakkir.tech Cloudflare zone completion unconfirmed. agents.md structural incompleteness addressed (July 9, 2026): full sections drafted for MERLIN, Midas, EMIYA, Cu Chulainn, Scathach with Agent Type classifications and Funnel Agent design principle. MERLIN Cloudflare SSL expiry check no longer hardcoded — migrated to Uptime Kuma source July 9, resolving the July 14 urgency. Midas cost tracking planned migration from duplicate Data Table to Langfuse Metrics API (low urgency). Cu Chulainn and Scathach sections added with concept-stage governance. Jeanne Alter assistant message table save bug remains critical, unresolved.** **Jeanne Alter Email Management Pipeline design complete (July 9, 2026): Read + Notify tier, 4 personal accounts (muzakkir.kholil06@gmail.com, muzakkirkholil97@icloud.com, hyperjhin00@gmail.com, business.najhin@gmail.com), work and NSFW accounts excluded. Architecture: per-account triggers → shared Email Classifier → branches to notification and/or Da Vinci Personal Knowledge gateway for permanent categories (bills/payments/subscriptions). Staging store (~7-day retention) for transient email. 3x/day schedule. Credentials (Gmail OAuth2, iCloud IMAP app-specific password) going to n8n credential store — first concrete step of ecosystem-wide credential store migration. Build effort: ~6-8h across 6 rollout steps. Design approved, implementation not yet started.** **Monitoring fix complete (July 14, 2026): node_exporter `/mnt` exclusion bug root-caused and fixed. Debian package's default `--collector.filesystem.mount-points-exclude` regex included `mnt`, making all `/mnt` mountpoints (hdd-backup-1, hdd-backup-2, ssd-storage, kinmoon-smb) invisible to Prometheus since ~May 16. Fixed by editing `/etc/default/prometheus-node-exporter` ARGS to remove `mnt` from exclude list and restarting service. MountpointMissing_hddbackup1 alert (stuck active for 8 days) auto-resolved. Confirmed hdd-backup-1 is primary live storage for Nextcloud (not a backup copy); Kinmoon NAS receives nightly rsync at 03:00. Physical SATA cable/port swap remains deferred (no budget). All 21 containers + 1 VM remain running and healthy.** **Palworld server added July 16, 2026 (CT 307): Deployed in Pelican Panel to test multiplayer gaming infrastructure. External connectivity initially broken due to PublicIP misconfiguration (was pointing to internal LAN IP 192.168.30.219). Root-caused and fixed: Pelican egg "Public IP" variable had User Editable + User Viewable permissions unchecked (blocking manual updates via Startup tab); enabled permissions and set PublicIP to current WAN IP (202.184.109.124). PalWorldSettings.ini found to require unbroken single-line format for OptionSettings block — Pelican's web file editor introduces line breaks, corrupting the file and causing Palworld to silently ignore ALL OptionSettings (not just edited field). Established safe editing method: use `pct exec` + sed from Proxmox host, never the Pelican web editor. Pelican's file Download function (404 error) not yet debugged. In-game stutter with 3 concurrent players traced to nightly vzdump backup job (sequential all-container) running concurrently with gameplay (~02:11 AM), causing CPU/disk I/O contention. Backup/restart consolidation and DDNS automation elevated in priority. WAN IP instability observed: 3 changes in one week (202.184.101.136 → 202.184.103.49 → 202.184.109.124); confirmed as TIME PPPoE session renegotiation behavior. All 22 containers + 1 VM running.** **Total capacity: 22 LXC containers + 1 KVM VM (CT 307 Palworld addition July 16-17).**

---

## 👤 About Me

| Field            | Details                                              |
|------------------|------------------------------------------------------|
| **Name**         | Muzakkir Kholil                                      |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location**     | Petaling Jaya, Selangor, Malaysia                    |
| **Target Role**  | Cloud Engineering / DevOps                           |
| **Domains**      | najhin-gaming.com (Cloudflare, gaming servers permanent), muzakkir.tech (Cloudflare, portfolio/homelab — zone setup directed July 1, status pending verification) |
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
- **Conversation history** → 15 messages (gilgamesh_conversations)
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
                                         pfSense WAN (pppoe0, 202.184.101.136 — see WAN IP Instability)
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

### WAN IP Instability (Documented July 16-17, 2026)

**Observed instability:** Three public IP address changes occurred in approximately one week:
1. 202.184.35.79 → 202.184.101.136 (July 5, 2026, bridge mode migration)
2. 202.184.101.136 → 202.184.103.49 (July 16, 2026)
3. 202.184.103.49 → 202.184.109.124 (July 16, 2026, second change same day)

**Root cause:** TIME Fiber PPPoE session renegotiation behavior — IP reassignment occurs on PPPoE reconnect, not strictly predictable intervals. ISP-level DHCP behavior (IPs appear to be DHCP-assigned rather than static), not a bridge-mode artifact.

**Impact on gaming servers:** Palworld (CT 307) PublicIP field and Cloudflare DNS records require manual updates after each WAN IP change. Impact low for established games (friend-invite/Steam relay mechanisms work regardless), but external port-forwarding-based connectivity (raw IP join) breaks until manual update.

**Mitigation strategy:** DDNS automation elevated to high priority (previously queued roadmap item). Should implement automatic Cloudflare DNS update on WAN IP change (detect via pfSense/Uptime Kuma, trigger Cloudflare API update, optionally trigger Palworld PublicIP field update via Pelican API if feasible).

**Current status (as of session end July 17, 2026):** WAN IP is 202.184.109.124. Palworld PublicIP field updated to this value. Cloudflare DNS records for game subdomains (Terraria, Minecraft, Enshrouded, Palworld) still require manual update verification.

### ISP Router Configuration (Huawei HG8145B7N)

- **Mode:** True bridge mode (all routing, NAT, DHCP, Wi-Fi disabled on this device)
- **Function:** Pure pass-through for single PPPoE session only
- **Wi-Fi/LAN ports on ISP router:** Now provide NO internet to directly-connected devices — this is expected, permanent behavior of bridge mode, not a fault
- **Reason for bridge mode:** To eliminate double-NAT layer and allow pfSense direct internet access for proper port forwarding and external service connectivity

### pfSense Configuration (Post-Bridge Mode)

- **WAN interface:** Changed from DHCP to **PPPoE (pppoe0)**
- **PPPoE credentials:** Username `muzakkir655@timebb`, password stored in Vault
- **Public IP:** `202.184.109.124` (current as of July 17, 2026; subject to ISP-initiated changes on PPPoE renegotiation — see WAN IP Instability section above)
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
| 220 | LXC  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com       | ✅         | ✅ Running |
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

### Nextcloud Hub (CT 220)

- **Root disk:** 100GB (resized from 20GB to resolve quota issues)
- **Data directory:** /mnt/ncdata (bind mount from /mnt/hdd-backup-1/nextcloud-data)
- **Version:** 33.0.2
- **Storage:** Data on Tier 3 HDD, app on NVMe (1.3GB after migration)
- **WebDAV base URL:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/
- **Enabled apps:** Deck (project management), Tasks, Calendar
- **Backup:** rsync to Kinmoon NAS nightly at 03:00
- **Known issue:** Duplicate `muzakkir-profile.md.md` file exists in 04-personal/ (WebDAV write-race artifact between Obsidian sync and the Da Vinci Personal Knowledge gateway) — pending cleanup

### Firefly III (CT 221)

- **Container:** fireflyiii/core:latest + MariaDB backend
- **IP:** 192.168.30.224 (NOTE: not .221, which is VM 400)
- **Purpose:** Personal finance tracking (MYR currency)
- **Access:** finance.najhin-gaming.com (Cloudflare Access + Email OTP)
- **API:** Token stored in Vaultwarden (name: gilgamesh)

### ntfy (CT 222)

- **Container:** binwiederhier/ntfy:latest on port 2586
- **Purpose:** Universal notification hub for all agents
- **Auth:** Built-in username/password (not Cloudflare Access for phone app compatibility)
- **Access:** ntfy.najhin-gaming.com (Cloudflare Tunnel without Access)
- **Integration:** Uptime Kuma → ntfy alerts configured

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

### network-ddns (CT 207)

- **Container:** LXC Ubuntu 22.04
- **IP:** 192.168.30.207
- **Services:** Promtail (log forwarding), ddclient (DNS updates)
- **Promtail Status:** Fixed May 20, 2026 — crash loop resolved (53,649 restarts), YAML corrupted with shell commands pasted into config file
- **Loki URL:** http://192.168.30.204:3100/loki/api/v1/push (CT 204, VLAN 30)
- **DDNS Scope:** Currently updates Cloudflare DNS only; Palworld PublicIP field updates remain manual (not yet automated)
- **Note:** node_exporter in this container reads host /proc/stat, so CPU alerts are host-level metrics, not CT 207-specific

### minecraft-server (CT 303)

- **Container:** Paper 1.21.4 on port 25570 (192.168.30.215)
- **Panel:** Pelican Wings
- **CPU limit:** Set via Pelican panel
- **Note:** Watch for CPU ramp-up issues; set cpulimit in Proxmox if needed

### terraria-server (CT 304)

- **Container:** tModLoader on port 7777 (192.168.30.216)
- **Panel:** Pelican Wings
- **CPU Management:** cpulimit 1.5 (hard ceiling at Proxmox hypervisor level) set via `pct set 304 --cpulimit 1.5`
- **Daily Cron:** `0 4 * * * pct exec 304 -- bash -c "kill $(pgrep -f tModLoader)" 2>/dev/null` — restart process daily at 4am to clear CPU leak
- **Issue Fixed (May 20, 2026):** tModLoader idles high (70%+) and ramps to 97%+ CPU over hours with heavy mods. cpulimit + daily cron restart resolves the issue.
- **Panel CPU Limit:** Set to 150% (= 1.5 cores, matching Proxmox cpulimit) in Pelican panel

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
  - **External connectivity:** NOT YET RETESTED post-bridge-mode. Double NAT eliminated; DNS records still point to old IP (202.184.35.79) and require update to new IP (202.184.109.124). Requires manual retest with packet capture once DNS updated.
  - **Friend connectivity method:** Steam relay/friend-invite mechanism confirmed working (masks that direct UDP forwarding was previously broken) — now should work directly via fixed port forwarding with bridge mode
- **Note on Pelican stats:** Panel's CPU/Memory/Disk display cosmetically broken for this Proton-wrapped egg (shows 0%/0 bytes/Unavailable regardless of actual server activity) — use server log for ground truth

### palworld-server (CT 307)

- **Container:** Palworld dedicated server in Pelican Panel (Docker, Proton-wrapped)
- **IP:** 192.168.30.219
- **Port:** 8211 UDP (LAN: 192.168.30.219:8211)
- **Panel:** Pelican Wings (panel.najhin-gaming.com)
- **Deployed:** July 16, 2026 (new gaming platform expansion)
- **Configuration:**
  - **PalWorldSettings.ini location:** `/home/container/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini`
  - **World save location:** `/home/container/Pal/Saved/SaveGames/0/6376E22F11CD4588A54E2EB0E7B1CD1F/` (note: no WorldOption.sav present — settings remain in ini file only)
  - **PublicIP field (for external connectivity):** 202.184.109.124 (current as of July 17, 2026; subject to WAN IP changes — see Network Architecture WAN IP Instability section)
  - **Pelican egg variable "Public IP":** Now has User Editable + User Viewable permissions enabled (fixed July 17 — previously unchecked, blocking manual IP updates via Startup tab)
  - **PalCaptureRate:** Default (1.000000) — was tested at 100.000000 but reverted to default after testing

**Critical Known Issues (July 16-17, 2026):**
- **PalWorldSettings.ini requires unbroken single-line format for OptionSettings block:** Pelican's web file editor (Files tab) introduces line breaks into `OptionSettings=(...)`, corrupting the file and causing Palworld to silently ignore ALL OptionSettings (fallback to defaults). Root cause identified: Pelican doesn't preserve multiline field formatting.
  - **Safe editing method established:** Use `pct exec 307 -- sed` commands from the Proxmox host to edit ini files directly, never use the Pelican web file editor.
  - **Example (safe):** `pct exec 307 -- sed -i 's/PalCaptureRate=.*/PalCaptureRate=1.000000/g' /home/container/Pal/Saved/Config/LinuxServer/PalWorldSettings.ini`
  
- **Pelican panel's file Download function (Files tab → Archive → Download) returns 404 "resource not found" error:** Reproducible even when accessing the panel via internal IP (192.168.30.217), ruling out Cloudflare/tunnel as the cause. Root cause not yet identified (suspected Wings/node FQDN mismatch in signed URL generation or archive path issue). Not debugged further this session (time-sensitive save backup was instead completed via `pct exec` + `pct pull` method from Proxmox host).
  - **Reliable backup workaround:** `pct exec <CTID> -- tar -czf /tmp/backup.tar.gz -C <path> <folder>` followed by `pct pull <CTID> /tmp/backup.tar.gz <destination>` — bypasses Wings/panel entirely, works directly at Proxmox host level.

- **WAN IP instability requires manual PublicIP updates:** Three IP changes observed in one week (July 16-17) due to TIME Fiber PPPoE renegotiation. Each IP change requires: (1) update PalWorldSettings.ini PublicIP field, (2) update Cloudflare DNS records, (3) potentially restart Palworld. **DDNS automation elevated to high priority** to automate PublicIP and DNS updates on IP change.

- **In-game stutter with 3 concurrent players:** Traced to nightly vzdump backup job (sequential all-container, currently ~02:11 AM) running concurrently with gameplay, causing CPU/disk I/O contention. NOT a Palworld resource or hardware capacity issue.
  - **Mitigation in planning:** Consolidate Palworld restart schedule, Terraria restart schedule, and vzdump backup job into a single off-peak maintenance block (time window not yet finalized).

**Backup information:**
- SaveGames backup created July 17, 2026 and stored at: `/mnt/hdd-backup-1/palworld-backup-20260716/palworld-save-backup.tar.gz` (22M, verified contents include Level.sav, LevelMeta.sav, Players/ data for world 6376E22F11CD4588A54E2EB0E7B1CD1F)
- Pre-fix (corrupted, 13-line) PalWorldSettings.ini backed up as `PalWorldSettings.ini.bak` inside the container (pending cleanup/deletion once stability confirmed)

**External connectivity status (as of July 17, 2026):**
- **LAN connectivity:** Confirmed working via Pelican panel
- **External connectivity:** NOT YET RETESTED post-bridge-mode and post-ini-fix. Friend connectivity mechanism (Steam relay/invite) presumed working. Direct port forwarding (8211 UDP) should now work with PublicIP correctly set, but not yet verified with packet capture.

### ollama-gpu (VM 400)

- **Type:** KVM VM with PCIe passthrough — NOT an LXC
- **GPU:** RX 6700 XT 12GB (0d:00.0) + audio (0d:00.1) passed through
- **PCIe config:** hostpci0=0000:0d:00.0,pcie=1,rombar=0 hostpci1=0000:0d:00.1,pcie=1
- **OS:** Ubuntu 22.04
- **SSH:** `ssh muzakkir@192.168.30.221` — use `muzakkir` user, not root
- **Disk:** Expanded from 56GB to 86GB (May 21, 2026) via Proxmox resize + LVM extension (device: /dev/vda, not /dev/sda). 34GB free post-expansion.
- **Ollama models:** qwen3:14b (primary, 9.3GB), qwen3.5:latest (secondary), nomic-embed-text (768 dims). Removed (May 21-22): gemma3:4b, gemma3:12b, phi4-mini, llama3.2:latest (all hallucinated factual data in testing).
- **Open WebUI:** Docker container, connects via 172.17.0.1:11434
- **Qdrant:** Port 6333 (REST), 6334 (gRPC), storage at /opt/qdrant/storage

### Gaming Servers

- **Pelican Panel (CT 305):** Next-gen game server panel (192.168.30.217), SQLite + Redis + PHP 8.3, access: panel.najhin-gaming.com
- **Minecraft Server (CT 303):** Paper 1.21.4 on port 25570 (192.168.30.215), Pelican Wings
- **Terraria Server (CT 304):** tModLoader on port 7777 (192.168.30.216), Pelican Wings
- **Enshrouded Server (CT 306):** Dedicated server on port 15636 (192.168.30.218), Pelican Wings
- **Palworld Server (CT 307):** Dedicated server on port 8211 UDP (192.168.30.219), Pelican Wings, deployed July 16, 2026
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

### Storage Status (Verified July 14, 2026)

| Mount            | Device     | Size  | Used | Filesystem | UUID                             |
|------------------|------------|-------|------|------------|----------------------------------|
| /mnt/hdd-backup-1 | /dev/sdb1 | 5.5T  | 18%  | ext4       | 5593849e-8ee9-4d6c-b1bc-3e35650e05fb |
| /mnt/hdd-backup-2 | /dev/sdc1 | 6.9T  | 0%   | ext4       | 5cd7f4a4-7510-42eb-9f35-be49f6a10686 |

**hdd-backup-1 Storage Role (Clarified July 14):**
- **Primary live storage** for Nextcloud data directory (bind mount at `/mnt/ncdata`)
- **Not a backup copy** — misconception cleared
- Nextcloud data written to this drive daily
- Kinmoon NAS receives a **nightly rsync copy at 03:00** — one-way, time-lagged replication, not a live mirror

**SATA Link Incident (July 6, Resolved):**
- **One-time event:** `ata2: SATA link down` observed 2026-07-06 19:06-19:07, recovered same session, filesystem remounted 20:35:38
- **Status:** Not an ongoing/recurring hardware fault as of July 14 dmesg review
- **Physical fix status:** SATA cable/port swap remains deferred pending physical access and budget availability — not a current technical blocker, infrastructure is functional

---

## 📚 Obsidian Knowledge Base (Phase 22)

### Vault Configuration

- **Path:** C:\\Users\\muzak\\Nextcloud\\Obsidian\\second-brain (auto-syncs to CT 220)
- **Platform:** Obsidian on Minimoon (Gaming PC)
- **Sync:** Via Nextcloud — changes sync automatically between local vault and cloud

### Plugins Installed

1. **Dataview** — Database queries and dashboards
2. **Tasks** — Task management and tracking
3. **Templater** — Note templates and automation
4. **Calendar** — Calendar view integration
5. **Excalidraw** — Diagrams and sketches
6. **Kanban** — Kanban boards for project management

### Folder Structure (Phase 22.1 Complete)

```
second-brain/
├── 00-inbox/          # Quick capture, unsorted notes
├── 01-homelab/        # Infrastructure documentation
├── 02-career/         # Career transition materials
├── 03-knowledge/      # Learning notes, references
├── 04-personal/       # Personal management, subscriptions, finance (indexed in Qdrant)
│   ├── subscriptions/ # Individual notes per subscription
│   ├── finance/       # Budget tracking, grocery lists
│   ├── grocery/       # Shopping lists and meal planning
│   ├── health/        # Blood pressure tracking, food logs
│   └── muzakkir-profile.md  # Personal profile managed by Da Vinci Personal Knowledge gateway
├── 05-templates/      # Note templates
├── 06-archive/        # Completed or outdated notes
├── 07-daily/          # Daily notes from Phase 22.2
├── 08-agents/         # Agent configurations and knowledge (was 08-projects)
├── 09-people/         # People notes (was 09-meetings)
├── 10-projects/       # Active projects and goals (was 10-reference)
└── 11-reference/      # Quick reference materials
```

### Key Features (Phase 22.1 + 22.2 + 22.8B + Phase 24.9)

- **Subscription tracker:** 6 subscriptions tracked in 04-personal/subscriptions/
- **Dashboard:** Dataview queries at 04-personal/dashboard showing costs and totals
- **Daily notes:** Auto-created at midnight via n8n (Phase 22.2)
- **Morning briefing:** 7am Telegram summary via MERLIN
- **Health tracking:** Food log, BP log, medication log via Jeanne Alter buttons (Phase 22.8B)
- **Personal profile:** muzakkir-profile.md managed by Da Vinci Personal Knowledge gateway, indexed in Qdrant obsidian_knowledge collection, recalled by Jeanne Alter via RAG (Phase 24.9)
- **/daily command:** Jeanne Alter creates immediate daily notes

---

## 🔮 Chaldea Agents Ecosystem (Renamed from Kuromoon July 8, 2026)

### PRIMARY GOAL: Jeanne Alter to replace claude.ai by end of 2026 (ongoing, no deadline)

**Current Cost:** $30-40/month Claude Pro + API usage
**Target Cost:** $5-10/month (local LLM + occasional API)

### 4-Phase Plan

#### Phase 1: Foundation (✅ COMPLETE)

- **Phase 38** — Ollama + ROCm on Kuromoon RX 6700 XT ✅ COMPLETE
- **Phase 22** — Obsidian Knowledge Base ✅ COMPLETE

#### Phase 2: Intelligence (✅ COMPLETE)

- **Phase 41** — Hybrid Routing (Ollama + Haiku + Sonnet) ✅ COMPLETE
- **Phase 7E** — Extended Memory (20+ message conversations via RAG) ✅ COMPLETE

#### Phase 3: Capabilities (In Progress)

- **Phase 7F** — File Generation (code, configs, docs)
- **Phase 7G** — Vision API (image analysis) — deferred (VRAM contention on single GPU)
- **Phase 7H** — Document Upload (PDF/text processing)
- **Phase 25.1** — Voice Interface (Whisper STT + local TTS) — research stage, approved
- **Email Management Pipeline** — Read + Notify tier, 4 personal accounts, design complete July 9, 2026, implementation pending (6-8h effort)

#### Phase 4: Refinement (Planned)

- **Phase 7I** — Quality Assurance (compare outputs with Claude Pro)
- **Phase 7J** — Migration (full switch to Jeanne Alter)

### Success Criteria

- 20+ message conversation retention (✅ complete)
- Knowledge base recall from Obsidian (✅ complete)
- Vision capabilities for screenshots/diagrams (deferred)
- Artifact generation (code, configs) (in progress)
- Monthly cost under $10 (on track)
- Real-time web search capability (✅ complete, May 25, 2026)
- Email management (design complete, implementation pending)

### Cost Projection Table

| Component  | Current          | Target          | Savings           |
|------------|------            |------           |------         ----|
| Claude Pro | $20/month        | $0/month        | $240/year         |
| API Usage  | $10-20/month     | $5-10/month     | $60-120/year      |
| **Total**  | **$30-40/month** | **$5-10/month** | **$300-360/year** |

### Backup Plan

Keep Claude Pro if quality insufficient, still save $10-15/month with hybrid approach.

### Proposed New Phases (July 8-9, 2026)

#### Phase 25.3 — Self-Evolving Skills Learning Loop

Inspiration: OpenClaw skills system + Hermes Agent closed learning loop
Concept: Agents that get smarter the longer they run by capturing solutions as reusable knowledge

Loop: detect → extract → store → recall → refine
1. Detection — After multi-step resolution or user correction, agent flags "this was hard/new"
2. Skill extraction — Agent generates structured procedure doc (problem, context, solution steps, gotchas)
3. Storage — Da Vinci writes to Obsidian (03-knowledge/skills/) and embeds in Qdrant
4. Recall — On similar future queries, RAG retrieves skill doc and injects into agent context
5. Refinement — If skill gets corrected or improved, updated version overwrites original

Dependencies: Da Vinci Stage 2 (Qdrant) → Phase 7E (extended memory) → Phase 24.4 (knowledge ingestion)
Effort: 10-12h
Architecture: Knowledge documents via RAG, not executable code. Fits Da Vinci-as-sole-writer. Executable skills = future EMIYA extension.

Examples of auto-generated skills:
- n8n Data Table Upsert always inserting → Conditions section gotcha
- WebDAV append pattern → GET-then-PUT code template
- Mixed RAM validation → memtest86+ procedure
- New LXC SSH access → pfSense rule template
- Promtail YAML corruption detection → shell command filtering
- tModLoader CPU leak diagnosis → cpulimit + cron restart pattern
- Palworld PublicIP field automation → manual ini edit + Pelican egg variable + Cloudflare DNS pattern

#### Phase 27 — Domain Migration & Infrastructure Audit (New, July 9, 2026)

**Context:** Muzakkir owns two domains (tracked in About Me as of July 9):
- **najhin-gaming.com** — Permanent for game servers (Minecraft, Terraria, Enshrouded, Palworld, Pelican panel)
- **muzakkir.tech** — Professional portfolio domain (purchased July 1, 2026; Cloudflare zone setup directed to begin July 1, completion status unconfirmed as of July 9)

**Phase 27.1 — Infrastructure Audit (Planned)**
- Inventory current Cloudflare Access policies (9 apps: grafana, n8n, vault, passwords, cloud, finance, ntfy, langfuse, home)
- Audit Tunnel routes, SSL certificates, NPM configs, DNS hygiene, service bindings
- Identify migration blockers and compatibility issues

**Phase 27.2 — Domain Migration (Planned)**
- Migrate 9 homelab subdomains from najhin-gaming.com to muzakkir.tech:
  - grafana.najhin-gaming.com → grafana.muzakkir.tech
  - n8n.najhin-gaming.com → n8n.muzakkir.tech
  - vault.najhin-gaming.com → vault.muzakkir.tech
  - passwords.najhin-gaming.com → passwords.muzakkir.tech
  - cloud.najhin-gaming.com → cloud.muzakkir.tech
  - finance.najhin-gaming.com → finance.muzakkir.tech
  - ntfy.najhin-gaming.com → ntfy.muzakkir.tech
  - langfuse.najhin-gaming.com → langfuse.muzakkir.tech
  - home.najhin-gaming.com → home.muzakkir.tech
- Game server subdomains remain on najhin-gaming.com (Minecraft, Terraria, Enshrouded, Palworld, panel)
- Migration mechanism: Cloudflare Tunnel handles routing; update tunnel routes + DNS records only (no NAT/port-forward changes needed)
- **Status:** Cloudflare zone for muzakkir.tech setup directed July 1, 2026 — completion status unconfirmed (pending verification next infrastructure session)
- Effort: ~2-3 hours (audit + DNS update + tunnel reconfig + verification)

#### Phase — Interest-Capture Loop (Concept, July 7 Session)

**Problem:** Jeanne Alter has no passive awareness of Muzakkir's personal interests outside homelab/career. Example: Path of Exile 2, current league "Return of the Ancients" (started May 29, 2026, v0.5.0) — game actively played, but no mechanism captures this as a lasting interest for future context.

**Current write path:** Da Vinci → muzakkir-profile.md → Qdrant → agent recall (works for explicitly stated facts)

**Missing capture path:** Nothing watches ordinary conversation and judges what constitutes a lasting interest worth writing down. Hard design problem is the judgment call ("what counts as lasting"), not storage.

**Design approaches (research stage):**
1. **Curator pattern:** User manually flags interests via /interest command, Da Vinci gateway assesses and writes (explicit, low noise, high overhead)
2. **Statistical pattern:** Track mention frequency of activities/topics in conversation, auto-flag recurring interests after N mentions (implicit, may noise, requires tuning)
3. **Cue-based:** Watch for contextual markers ("I've been playing," "I'm learning," "I started") in conversation, route to quality gate (medium precision, moderate complexity)

**Status:** Concept only, not scoped or built. Deferred pending clarification of what triggers interest-capture (manual vs. automatic) and where captured interests appear (muzakkir-profile.md? separate interests.md?).

#### Phase — Gap Analysis: Four Blind Spots (Documented July 7, Addressed July 9)

Tracked list of ecosystem gaps distinct from existing roadmap items:

1. **Time vs. stated priorities**
   - **Statement:** Health → Mental → Work → Gaming → n8n business → Homelab
   - **Reality:** Nothing currently measures whether actual time allocation reflects stated order
   - **Implication:** Homelab expansion may be optimizing the wrong priority layer
   - **Solution:** Proposal to track time per week by category; MERLIN could report weekly breakdowns for user review

2. **Docs vs. reality drift**
   - **History:** May 22, 2026 documentation audit caught 5 significant gaps (ROADMAP archival status, hallucinated hardware specs, agents roster inconsistencies)
   - **Problem:** One-off manual audit found artifacts; no continuous mechanism prevents future drift
   - **Implication:** Documentation becomes stale faster than it's updated
   - **Solution:** Proposal for automated checks (Da Vinci consistency scans, doc-to-code linting) run during update pipeline

3. **Bus factor**
   - **Risk:** Entire Chaldea ecosystem depends on Muzakkir alone (single operator)
   - **Implication:** If unavailable (illness, overload, emergency), system cannot continue learning/improving
   - **Example:** If Muzakkir is unavailable, no one can tune Jeanne Alter's routing, add new n8n workflows, or maintain Obsidian knowledge
   - **Solution:** (Research stage) Graceful degradation playbook (which services stay running offline? which agents require active tuning?), possibly operational runbook for co-maintainer handoff

4. **Skill-market fit**
   - **Current skillset:** n8n (workflow automation), LXC (containers), Qdrant (vector DB), Obsidian (knowledge management), Cloudflare, pfSense, Nextcloud
   - **Target roles:** Cloud Engineering / DevOps in Malaysian market
   - **Gap:** Malaysian market DevOps typically requires Terraform, Kubernetes, CI/CD (GitHub Actions, GitLab CI), Docker (vs LXC), cloud providers (AWS, Azure, GCP)
   - **Implication:** Homelab builds depth in niche tools but breadth gap for market-ready roles
   - **Solution:** Proposed phases to build K8s + CI/CD skills explicitly (Phase ~28, research stage)

**Status:** All four documented July 7, cross-checked July 9. Not roadmap items; awareness layer only.

#### Phase Chaldea Rename Propagation (July 8, Pending)

**Name change (July 8, 2026):** Rename Gilgamesh → Jeanne Alter ("The Corrupted Ruler") across:
- Bot identity (telegram bot persona)
- n8n system prompt node text (all agent workflows)
- Telegram bot username (@JhinGilgamesh_bot → NEW USERNAME)
- All 8 documentation files (AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)

**Effort:** Multi-session, requires careful coordination across systems.

**Scope:** Kuromoon now refers strictly to physical hardware. Chaldea is the agents ecosystem layer. Update all references where needed.

#### Phase — Agent Roster Rename: Guardian → Cu Chulainn (Propagation Pending)

**Rename history:** Guardian was renamed to **Cu Chulainn** ⚡ (Lancer-class servant) on May 16, 2026, intended role: security monitoring agent. **Propagation has never occurred** — all documentation still references "Guardian." 

**Propagation scope (multi-session):**
- agents.md: full section for Cu Chulainn (drafted July 9, 2026 — see Cu Chulainn section below)
- AI-CONTEXT.md: agent roster table (replace Guardian → Cu Chulainn)
- decisions.md: any historical Guardian references
- n8n workflows: system prompts if any reference Guardian

**Build priority:** 3rd (after Scathach, Cu Chulainn — no, Scathach 1st, Cu Chulainn 3rd) — not yet scoped for implementation.

#### Phase — Scathach Agent Design (Named May 16, Updated July 9 with LangGraph Sequencing)

**Introduction:** Lancer-class servant (Scathach, FGO lore: Cù Chulainn's teacher, master of land/sea combat). Intended role: **Career growth and job application workflows** (1st priority after core agents).

**Flagged requirement (June 5 session):** Evaluate LangGraph before starting Scathach build, since career-research workflows are expected to need autonomous multi-step reasoning loops that n8n handles poorly for this use case.

**Updated sequencing (July 9, 2026):** LangGraph evaluation should happen *after* Phase 24.12 (Jeanne Alter's refactor to n8n's native AI Agent node), not standalone. 24.12 is already testing whether n8n's native AI Agent node can handle multi-step autonomous reasoning for Jeanne Alter — if it can, that same node may cover Scathach's career-research reasoning needs too, avoiding the need to introduce a second orchestration framework into the stack.

**Build priority:** 1st (ahead of Cu Chulainn and EMIYA expansions)

**Status:** Named and prioritized May 16, no implementation scope defined yet, **LangGraph evaluation now sequenced behind Phase 24.12 (Jeanne Alter Architecture Refactor).**

**Dependencies:** Requires decision on whether to continue pure n8n architecture or adopt external orchestration framework (LangGraph, CrewAI, etc.); now more directly informed by Phase 24.12's outcome.

#### Phase — Nightingale Agent (Health Pipeline Extraction, Concept)

**Introduction:** Health tracking currently handled within Jeanne Alter (food/BP/medication logging buttons, Phase 22.8B). 

**Proposed extraction:** Dedicated **Nightingale** agent (Nightingale, FGO lore: nurse/healer class servant) to own health pipeline independently.

**When noted:** May 17, 2026 session; explicitly deferred to avoid mid-session scope creep.

**Status:** Concept only, not scoped, not prioritized in build order.

#### Phase: Ecosystem-Wide Credential Store Migration

Move all hardcoded n8n credentials (Da Vinci, MERLIN, Midas, Jeanne Alter) from hardcoded node values to n8n's built-in credential store. Applies to GitHub API tokens, Anthropic API keys, Nextcloud auth, Proxmox API tokens, etc.

**First concrete implementation:** Jeanne Alter Email Management Pipeline will use n8n credential store for Gmail OAuth2 and iCloud IMAP passwords (July 9, 2026).

**Effort:** Multi-session, security + cleanliness improvement.

#### Phase: Jeanne Alter Architecture Refactor

Adopt open-source components for consistent agent design:
- **MCP (Model Context Protocol)** — Tool execution ("hands")
- **Mem0** (self-hosted, Qdrant-backed) — Unified memory layer (replaces Data Table + RAG split)
- **n8n AI Agent node** — Orchestration ("skeleton")
- **n8n credential store** — Security ("immune system")
- **Personality file pattern** — Persona/voice definition

**Scope:** Rebuild Jeanne Alter's core architecture incrementally, not a one-shot refactor. Keep resumé value (DIY around proven external components, not bespoke from zero).

**Effort:** Multi-session.

#### Phase: Jeanne Alter Web Search Quality Improvement

Current: One Firecrawl query → single Haiku call → raw results injected.
Target: Iterative multi-query search with synthesis step (closer to Gemini-style).

**Effort:** One session.

#### Phase: Universal Time/Date Awareness

Inject current date/time into every agent's system prompt (not just Da Vinci's Personal Knowledge gateway). Pattern: `{{date}}` replacement applied ecosystem-wide.

**Effort:** Quick, one sitting.

#### Phase: Multi-Agent Discussion Protocol (Research Stage)

CrewAI-based framework for agents to deliberate complex topics.
- Trigger: complexity-based (quick topics via Telegram, complex/research topics in background)
- Disagreement resolution: Jeanne Alter presents both sides, user arbitrates (not consensus-forced)
- Reporting: summary by default, full transcript on request
- Explicitly human-in-the-loop, not autonomous self-modification

**Status:** Research phase, explicitly deferred — not a priority.

#### Phase: Solomon — Overseer/Growth Agent (Research Stage)

FGO-lore fit: administrator of Chaldea. Weekly cron-triggered review of all agents' activity/logs/decisions; proposes fixes for user approval.

**Scope:** Deliberately human-in-the-loop growth, not autonomous self-evolution.

**Status:** Research phase, explicitly deferred.

---

## 🎴 Chaldea Agent Roster

Theme: Homelab agents named after Fate/Grand Order servants.

### Active Agents

| Servant      | Class  | Role                                                                          | Platform                      | Status                     | Pronouns |
|--------------|--------|-------------------------------------------------------------------------------|-------------------------------|----------------------------|----------|
| Jeanne Alter 👑 | Ruler | Life Interface & Personal AI Assistant (Langfuse wired May 22, 2026; Web search live May 25, 2026; Email management design complete July 9, 2026) | Telegram (@JhinGilgamesh_bot) | ✅ Active (rename pending July 8) | she/her |
| Da Vinci 🎨  | Caster | Sync + Goals + Review (Stage 2 RAG active; documentation pipeline active; Personal Knowledge gateway active; Deck sync pending) | n8n/Nextcloud | ✅ Active | she/her |
| Midas 💰     | Caster | CFO — Cost Tracking & Optimization (Langfuse Metrics API migration pending, pending Uptime Kuma SSL migration) | n8n                           | ✅ Active | he/him  |
| MERLIN 🔮    | Caster | Proactive Nudges & Scheduler (Uptime Kuma SSL source migration complete July 9, 2026 — resolved July 14 urgency) | n8n                           | ✅ Active | he/him  |

### Full 6+ Agent Roster (Design Complete, Partial Deployment)

| Servant             | Class    | Role                                          | Platform      | Build Order | Status     |
|---------------------|----------|-----------------------------------------------|---------------|-------------|------------|
| Jeanne Alter 👑     | Ruler    | Life Interface & Personal AI Assistant (Langfuse wired, RAG for personal profile, web search, email management design complete) | Telegram | —         | ✅ Active (rename pending) |
| Da Vinci 🎨         | Caster   | Sync + Goals + Weekly Review + RAG + Deck Sync | n8n/Nextcloud | —      | ✅ Active (Deck sync design complete, needs backfill) |
| Midas 💰            | Caster   | CFO — Cost Tracking & Optimization            | n8n           | 2nd         | ✅ Active (Langfuse cost-source migration pending) |
| MERLIN 🔮           | Caster   | Proactive Nudges & Health Scheduler           | n8n           | 3rd         | ✅ Active (Uptime Kuma SSL source migration complete July 9, 2026) |
| Scathach ⚡         | Lancer   | Career Growth & Job Application Workflows     | n8n (pending LangGraph eval after Phase 24.12) | 1st | 📋 Concept (LangGraph eval sequenced after Phase 24.12, build scope pending) |
| EMIYA 🏹            | Archer   | CTO — Infrastructure + Agent Spawning         | n8n           | 2nd (research) | ✅ Partial (App Management 24.1, ntfy 24.7, Langfuse 24.8 deployed; Alert Translation 24.2, Container Updates 24.3, Knowledge Ingestion 24.4, Proactive Monitoring 24.5, Performance 24.6 pending) |
| Cu Chulainn ⚡      | Lancer   | Security Monitoring & Threat Detection        | n8n           | 3rd (research) | 📋 Concept (renamed from Guardian May 16, full section drafted July 9, propagation + build pending) |
| Nightingale 🕊      | —        | Health Pipeline Agent (Food/BP/Medication)   | n8n           | Deferred    | 📋 Concept (extraction from Jeanne Alter, not yet scoped) |

**Notes:**

- Roster finalized at 6+ core agents + 2 concept agents (Nightingale, Solomon overseer)
- Jeanne Alter identity pending full propagation (rename from Gilgamesh, July 8, 2026)
- Scathach, Cu Chulainn, and MERLIN sections drafted July 9, 2026 with full governance + design principles
- All agents write Obsidian data through Da Vinci Personal Knowledge gateway (not directly)
- Da Vinci Deck sync design complete but requires manual backfill of existing cards with sync-id tags before automation goes live

### Agent Design Principles (July 9, 2026)

#### Funnel Agents

An agent is a **funnel** if there is existing infrastructure — deployed or deployable — that the agent sits on top of and translates into a daily digest with a recommended action. The agent's value is aggregation and actionability, not re-implementing what the underlying tool already does. A funnel agent should always answer two questions, not one: "what happened" and "what should I do about it."

**Test:** If there's infrastructure that could be deployed (or already is) and the agent's job is to sit on top of it and translate it, that agent is a funnel.

**Classification (July 9, 2026):**
- **Funnel:** MERLIN (sources from Uptime Kuma), Midas (sources from Langfuse), Cu Chulainn (sources from Alertmanager, once built)
- **Not a funnel — Interface:** Jeanne Alter (she is the interface itself; nothing underneath to translate)
- **Not a funnel — Knowledge System:** Da Vinci (creates and maintains state across Obsidian/GitHub/Deck; doesn't aggregate someone else's data)
- **Not a funnel — Generative:** Scathach (career research and reasoning; nothing to funnel from)
- **Hybrid — Funnel-Plus:** EMIYA (reads infra state like a funnel, but also executes changes on approval — propose → approve → execute, a step past reporting)

**Why this matters:** Building a funnel agent that reinvents the underlying check (e.g., MERLIN's original hardcoded SSL date instead of reading Uptime Kuma) duplicates effort that's already solved elsewhere in the stack, creates a second source of truth that can drift from the first, and spends build time on plumbing instead of the actual differentiator — the daily aggregation and recommendation layer.

### Da Vinci — Sync + Goals + Weekly Review + RAG + Personal Knowledge Gateway + Nextcloud Deck

**Role scope:**

- **Documentation:** maintains AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md via /update and /sync-docs
- **Obsidian writes:** session summaries written to vault via Nextcloud WebDAV; personal knowledge writes via Da Vinci Personal Knowledge gateway
- **Nextcloud Deck:** kanban board sync (Homelab board ID 4) — matches cards to documentation items via hidden sync-id tags, auto-updates stack based on status keywords (Backlog/Up Next/Blocked/In Progress/Done)
- **Goal tracking:** monitors progress, identifies blockers, suggests next steps
- **Weekly review:** Sunday analysis of week's progress, agent performance, system health
- **RAG retrieval (Stage 2):** Qdrant + nomic-embed-text for knowledge recall across all agents
- **Personal Knowledge gateway:** receives facts from Jeanne Alter and future agents via webhook, assesses quality, writes to muzakkir-profile.md, logs cost
- **Observability logs:** all agents log activity to Da Vinci
- **Langfuse tracing:** logs trace (da-vinci-update) with 8 child generations per pipeline run

**Stage 2 stack:** Qdrant (VM 400, port 6333/6334) + nomic-embed-text + n8n Knowledge Indexer workflow

**Collections:**
- obsidian_knowledge: 1,736 points (90 indexed files including 04-personal/, 08-agents/, 09-people/, 10-projects/)
- gilgamesh_conversations: archived long-term memories

**Pronouns:** she/her

**Technical notes:**

- Direct node references required: use `$('Extract Response').first().json.updatedDoc` instead of `$json` after Merge node
- max_tokens: AI-CONTEXT 25000, changelog 6000, troubleshoot 4000, ROADMAP 8000, agents 8000, current-state 4000, service-catalog 4000, decisions 3000 (prevents hallucinated content)
- Grounding fix: Per-file system prompts in Claude project instructions with explicit per-file sections (CHANGES TO AI-CONTEXT.MD, CHANGES TO CHANGELOG.MD, etc.)
- Nextcloud Deck integration complete with 6 additional nodes for kanban management
- Da Vinci workflow includes Limit node before Notify Complete to prevent notification spam
- Knowledge Indexer runs at 3am daily (full re-index v1), indexes 90 files now (was ~70 before Phase 24.9). Partial reindex triggered via webhook after Da Vinci writes (Phase 24.10).
- **Concurrency lock:** Check Running node queries n8n API before proceeding
- **Schedule:** Inbox Watcher runs every 15 minutes (was 1 minute)
- **Model:** claude-haiku-4-5-20251001 (8 separate API calls, one per file: AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)
- **Cost logging:** Fires immediately after each of the 8 API calls, before parse/push nodes — logs even on downstream failure
- **Langfuse integration:** Single Langfuse node branched off Push to GitHub (after all 8 files complete). Logs one trace (da-vinci-update) with 8 child generations per run. Uses internal URL: http://192.168.30.223:3000
- **Haiku pricing:** $0.80/1M input tokens, $4.00/1M output tokens
- **Cost per run estimate:** ~$0.25-0.35 for 8 files (up from ~$0.11 for 3 files)
- **Personal Knowledge gateway:** Separate n8n workflow (Da Vinci — Personal Knowledge). Webhook POST /davinci-personal-knowledge. Uses Claude Haiku max_tokens 4000. Writes to muzakkir-profile.md via WebDAV. Filters out one-time events, stores durable personal facts. Quality gate prevents noise.
- **Triggered Qdrant Re-indexing (Phase 24.10):** Webhook POST /davinci-reindex-personal triggers partial reindex (04-personal/ only, ~1s). Fires async after Da Vinci writes. Full reindex still runs daily at 3am (~21s, indexes all 10 folders).
- **Nextcloud Deck Sync (Design Complete, Pending Manual Backfill):** New 9th pipeline step after GitHub push. Mechanism: match cards via hidden `sync-id` tag in description footer. Update existing cards silently (title/description/stack); create new cards for genuinely new items. Stack routing: status-keyword based ("Complete" → Done, "In Progress" → In Progress, etc.). Scope: Homelab board (ID 4) only. **Precondition:** All ~30 existing cards must be manually backfilled with sync-id tags before automation goes live (chosen deliberately to avoid fuzzy automated matching on known conflicts).

### Nextcloud Deck Integration

Da Vinci integrates with Nextcloud Deck for kanban project management:

**Boards:** 3 boards (Homelab ID: 4, Career ID: 5, Personal ID: 6)
**Stacks:** Backlog → Up Next → Blocked → In Progress → Done
**Labels:** 22 on Homelab (category, priority, agent, effort), 9 on Career/Personal
**Behavior:** Auto-create/complete cards, advisory comments for movement
**API:** http://192.168.30.220/index.php/apps/deck/api/v1.0
**Auth:** Basic auth with app password (credential: NextCloud-Deck)
**Sync scope:** Homelab board only, pending manual backfill of existing cards with sync-id tags

### MERLIN — Proactive Nudges & Health Scheduler

**Servant Class:** Caster
**Ascension Stage:** 2/4
**True Name:** Merlin
**Agent Type:** Funnel (sources from Uptime Kuma, Proxmox, Vault)

#### Overview

Proactive infrastructure nudge and health scheduler. Deployed as v1 April 27, 2026. Runs a daily 8am check across several infrastructure signals and delivers a Telegram digest. As a funnel agent, MERLIN's job is to read state that already exists elsewhere and translate it into "what happened + what to do" — not to re-implement monitoring logic that dedicated tools already handle better.

#### Core Responsibilities

- **SSL certificate expiry reporting** (RESOLVED July 9, 2026 — migrated to Uptime Kuma source, no longer hardcoded)
- Backup restore test reminders (tracks lastTestDate, currently self-tracked — no existing tool covers this, appropriately custom)
- Proxmox memory check (85% threshold)
- Vault seal status check
- Health nudges: medication reminders, BP readings, exercise prompts
- Daily 8am schedule via n8n Cron trigger

#### Active Workflows

##### MERLIN — Reminders

**Status:** Active — SSL check source migrated to Uptime Kuma (July 9, 2026), July 14 urgency resolved
**Nodes:** 8
**Trigger:** Schedule (daily 8am)
**Current checks:** 
- SSL expiry (migrated to Uptime Kuma data source, no longer hardcoded)
- Backup restore test (baseline 2026-01-01)
- Proxmox memory usage (85% threshold)
- Vault seal status

#### Design Decision (July 9, 2026)

**MERLIN's SSL check was re-sourced from Uptime Kuma instead of a direct Cloudflare API call.** Uptime Kuma (CT 206, already deployed) natively tracks certificate expiry on any HTTPS monitor with zero extra credentials required. This removed the Cloudflare token dependency entirely and resolved the July 14 urgent issue without waiting on a Vault credential re-store.

#### Known Limitations

- No "hands" — report-only, cannot execute remediation itself
- No memory — stateless checks each run
- Currently reports raw numbers ("X days until expiry") without a recommended action — incomplete per the Funnel Agent design principle; each check should also state what to do (e.g., "renew via Cloudflare dashboard" or "backup test overdue, run restore-verify script")
- Langfuse not yet wired to MERLIN (planned)
- Credentials hardcoded in n8n nodes (pending Phase 24.11 credential store migration)

#### Dependencies

- n8n (CT 211) — orchestration
- Uptime Kuma (CT 206) — SSL/cert data source (fully integrated July 9, 2026, resolves prior Cloudflare API dependency)
- Proxmox API — memory check
- HashiCorp Vault (CT 213) — seal status check
- Telegram — delivery

#### Monitoring

- Daily 8am run — verify Telegram delivery
- **SSL check migrated to Uptime Kuma source (July 9, 2026) — July 14 urgency resolved**

### Midas — CFO (Cost Tracking & Optimization)

**Servant Class:** Caster
**Ascension Stage:** 2/4
**True Name:** Midas
**Agent Type:** Funnel (sources from Langfuse for Haiku/Claude costs; retains own tracking for Ollama)

#### Overview

CFO agent — cost tracking and optimization reporting. Deployed v1 April 27, 2026. As a funnel agent, Midas's differentiators are Telegram delivery and MYR currency conversion, not the underlying cost calculation — Langfuse (CT 223, already self-hosted and wired to Da Vinci + Jeanne Alter) already calculates and tracks per-model LLM costs automatically.

#### Core Responsibilities

- `/midas` command: full API cost report via Telegram
- 9am daily brief: cost summary sent to Telegram
- USD to MYR conversion (hardcoded rate: 4.7)
- Ollama savings calculation (Ollama calls cost $0; Midas estimates the Haiku-equivalent cost avoided)
- $10 monthly API spend limit monitoring
- Planned v2: Firefly III API integration (budget sync, MYR currency)

#### Active Workflows

##### Midas — CFO Report
**Nodes:** 6 | **Trigger:** Webhook (`/midas` command)

##### Midas — Daily Brief
**Nodes:** 4 | **Trigger:** Schedule (9am daily)

#### Design Decision (July 9, 2026)

**Haiku/Claude cost data should be sourced from Langfuse's Metrics API instead of the separate `gilgamesh_costs` Data Table.** Both currently track the same Haiku calls independently, which risks the two numbers drifting apart with no way to tell which is correct. Ollama cost tracking (savings estimate) stays in the existing Data Table since those calls are $0 and outside Langfuse's automatic cost-inference model, which is built for paid providers.

#### Known Limitations

- **Pending migration:** Haiku/Claude cost tracking currently duplicated between Langfuse and gilgamesh_costs Data Table — single source of truth not yet established
- Currently reports totals without a recommendation — incomplete per the Funnel Agent design principle; when spend trends toward the $10 monthly threshold, Midas should suggest a specific action (e.g., "route more queries to Ollama" or name the highest-cost command_type driving the trend)
- Hardcoded USD→MYR exchange rate (4.7) — not dynamically fetched, will drift from actual rate over time
- Reads `gilgamesh_costs` Data Table, named for historical (Gilgamesh) identity — pending rename alongside broader Jeanne Alter propagation
- Langfuse not yet directly wired to Midas (needed for the Metrics API migration above)
- $10 monthly spend limit is monitored, not enforced — no automatic throttling if exceeded

#### Dependencies

- n8n (CT 211) — orchestration
- Langfuse (CT 223) — Haiku/Claude cost source (replaces direct Data Table calculation for paid-model costs, pending implementation)
- `gilgamesh_costs` Data Table — retained for Ollama savings tracking only
- Telegram — delivery

#### Monitoring

- `/midas` command response time and accuracy
- 9am daily brief delivery
- Verify Langfuse Metrics API migration doesn't break existing MYR conversion or Ollama savings math once implemented

### EMIYA — CTO + Infrastructure + Agent Spawning

**Servant Class:** Archer
**Ascension Stage:** Partial — 3 of 8 sub-phases complete (corrects prior Planned/0/4 status conflict flagged July 8, 2026)
**True Name:** EMIYA
**Agent Type:** Funnel-Plus (reads infrastructure state like a funnel, but also executes changes on approval)

#### Overview

CTO / infrastructure agent responsible for app lifecycle management, alerting, and (eventually) agent-spawning. Deployed incrementally across 8 sub-phases (24.1–24.8). This section corrects a status conflict identified in the July 8-9, 2026 documentation audit: agents.md previously had no entry for EMIYA at all, while AI-CONTEXT.md already showed 3 of 8 sub-phases complete.

#### Design Principle

EMIYA proposes → Muzakkir approves → EMIYA executes. No autonomous destructive actions. (This approval-gated pattern was validated against 2026 industry practice during design review — comparable self-hosted assistant projects added similar approval-gate features specifically to address the security risk of broad-permission autonomous agents; EMIYA's design already avoids that risk.)

#### Core Responsibilities (8 sub-phases, 24.1–24.8)

1. **App Management** (24.1) ✅ COMPLETE — Firefly III, ntfy, Langfuse lifecycle
2. **Alert Translation** (24.2) 📋 Planned — Alertmanager alerts → plain English via ntfy
3. **Container Updates** (24.3) 📋 Planned — Docker/apt upgrades, approval-gated
4. **Knowledge Ingestion** (24.4) 📋 Planned — URL → Firecrawl → triple-write pipeline
5. **Proactive Monitoring** (24.5) 📋 Planned — anomaly detection, trend alerts
6. **Performance + Goal Optimization** (24.6) 📋 Planned — resource reallocation, goal tracking
7. **Universal Notifications** (24.7) ✅ COMPLETE — ntfy hub for all agent communications
8. **Langfuse AI Observability** (24.8) ✅ COMPLETE — LLM performance tracking and tracing

#### Active Workflows

No dedicated "EMIYA"-branded n8n workflow currently exists as a single unified entity. The 3 completed sub-phases (24.1, 24.7, 24.8) were implemented as direct infrastructure deployments (Firefly III/ntfy/Langfuse container setup and wiring) rather than as an EMIYA-specific workflow. This is a real structural gap: EMIYA is currently a roadmap category with completed infrastructure underneath it, not yet a running agent that reads and acts on that infrastructure.

#### Known Limitations

- Not yet a unified running agent — completed sub-phases are infrastructure completions, not EMIYA-branded workflows that reason over them
- Remaining 5 of 8 sub-phases (24.2–24.6) are one-line descriptions only, unscoped in detail
- Estimated total effort: 24-32h across all 8 sub-phases; roughly 9-12h already spent on 24.1/24.7/24.8, remainder unestimated per-phase
- Once Alert Translation (24.2) is built, EMIYA becomes a second Alertmanager-sourced funnel-plus alongside Cu Chulainn's planned security-focused translation — worth deciding at that point whether these should be one workflow with two lenses (general ops vs. security) or genuinely separate agents, to avoid two agents independently parsing the same Alertmanager feed

#### Dependencies

- Firefly III (CT 221), ntfy (CT 222), Langfuse (CT 223) — live, wired (24.1/24.7/24.8)
- Alertmanager (CT 205) — planned integration (24.2)
- Docker/apt — planned integration (24.3)
- Firecrawl — planned integration (24.4)

#### Monitoring

- App Management: Firefly III/ntfy/Langfuse containers monitored via existing Prometheus/Uptime Kuma, not via a dedicated EMIYA process yet
- No dedicated EMIYA-level monitoring exists — flagged as a gap since the reasoning/translation layer is unbuilt

### Cu Chulainn — Security Monitoring & Threat Detection

**Servant Class:** Lancer
**Ascension Stage:** 0/4 (Concept — no implementation started)
**True Name:** Cú Chulainn (renamed from Guardian May 16, 2026)
**Agent Type:** Funnel (sources from Alertmanager, once built)

#### Overview

Security monitoring and threat detection agent for the Chaldea ecosystem, translating Alertmanager's alert feed into readable, prioritized security reporting. In FGO canon, Cu Chulainn is Scathach's student, reflected in build order (Scathach built first, Cu Chulainn second). No implementation work has begun as of July 9, 2026. Renamed from "Guardian" on May 16, 2026 (rename propagation to n8n workflows pending once workflows exist).

#### Core Responsibilities (Planned — none implemented)

- Alert translation: converting raw Prometheus/Alertmanager alerts into readable, prioritized summaries, security-focused
- Threat detection across Chaldea/homelab infrastructure
- Security reporting to Muzakkir, delivered via Jeanne Alter/Telegram, including a recommended action per the Funnel Agent design principle (not just "alert fired" but "here's what it likely means and what to check")

#### Active Workflows

None. No n8n workflow exists for this agent yet.

#### Known Limitations

- Build priority is 3rd, behind Scathach (1st) and after current core agents (MERLIN, Midas, EMIYA) — work is not expected to start until Scathach's build is underway or complete
- Overlaps with EMIYA's planned Alert Translation (24.2), which also sources from Alertmanager — worth deciding whether these merge into one workflow with two lenses (general ops vs. security) once both are scoped, to avoid duplicate parsing of the same feed
- No defined trigger mechanism or report format specified yet
- Estimated effort: 6-8h (per ROADMAP.md), unvalidated since no scoping work has occurred
- Rename propagation (from Guardian) pending once first n8n workflow is built

#### Dependencies (Planned, not yet wired)

- Prometheus / Alertmanager (CT 202 / CT 205) — intended alert source
- Jeanne Alter — intended reporting interface to Muzakkir
- n8n — intended orchestration platform

#### Monitoring

Not applicable — no active deployment to monitor.

### Scathach — Career Growth & Job Application Workflows

**Servant Class:** Lancer
**Ascension Stage:** 0/4 (Concept — LangGraph evaluation sequenced behind Phase 24.12, blocking)
**True Name:** Scathach
**Agent Type:** Not a funnel (generative career research/reasoning — nothing existing to translate)

#### Overview

Career growth and job application workflows agent. Named and prioritized on May 16, 2026 as the 1st build priority in the agent roster. In FGO canon, Scathach is Cu Chulainn's teacher and a master of land/sea combat, reflected in the build order. No implementation work has begun as of July 9, 2026.

#### Core Responsibilities (Planned — none implemented)

- Career research: job market analysis, role matching against target positions (Cloud Infrastructure Engineer, DevOps Engineer, Systems Administrator)
- Job application workflow automation
- Possible integration point with ApplyPilot (C:\Users\muzak\.applypilot\) — relationship not yet defined

#### Active Workflows

None. No n8n or LangGraph workflow exists for this agent yet.

#### Updated Sequencing (July 9, 2026)

**LangGraph evaluation should happen *after* Phase 24.12 (Jeanne Alter's refactor to n8n's native AI Agent node), not standalone.** 24.12 is already testing whether n8n's native AI Agent node can handle multi-step autonomous reasoning for Jeanne Alter — if it can, that same node may cover Scathach's career-research reasoning needs too, avoiding the need to introduce a second orchestration framework into the stack. Original prerequisite (decision logged 2026-07-09, decisions.md) still holds — LangGraph must be evaluated before implementation scope is defined — but the evaluation itself is now sequenced behind 24.12 rather than done independently.

#### Known Limitations

- **Blocking prerequisite, sequenced after Phase 24.12:** LangGraph evaluation cannot proceed until 24.12 clarifies whether n8n's native AI Agent node handles multi-step reasoning adequately
- No defined data sources, external tool integrations, or output format
- Relationship to ApplyPilot not yet clarified
- Estimated effort: TBD — cannot be scoped until the (now-sequenced) LangGraph evaluation completes

#### Dependencies (Planned, not yet adopted)

- Phase 24.12 (Jeanne Alter Architecture Refactor) — should complete first, may resolve the LangGraph question
- LangGraph — framework evaluation pending (sequenced after 24.12), not yet adopted for any agent
- Possible ApplyPilot integration (C:\Users\muzak\.applypilot\)
- Reporting interface (likely Jeanne Alter/Telegram) — undetermined

#### Monitoring

Not applicable — no active deployment to monitor.

---

## 🤖 Jeanne Alter AI Agent (Phase 7C/7D/7D-Menu/15/41/Da Vinci Stage 2/Phase 7E/Phase 24.9/Phase 24.10/Web Search/Email Management Design)

> **Note:** Historical identity was "Gilgamesh" (@JhinGilgamesh_bot). Renamed to Jeanne Alter ("The Corrupted Ruler") July 8, 2026. Full propagation across bot, n8n system prompt, Telegram username, and all 8 documentation files pending.

### Jeanne Alter Chat Pipeline — Architecture

**Architecture:**
```
Telegram (@JhinGilgamesh_bot) → n8n Workflow → Get Conversation History
                                                    ↓
                                          Format Messages (RAG + history)
                                                    ↓
                                    If (Needs Web Search?)
                                      ↙              ↖
                               (true)               (false)
                                 ↓                    ↓
                        Web Search (Firecrawl)   Route Model
                                 ↓                    ↓
                         Inject Search Results    Call Ollama or
                                 ↓                 Call Claude API
                               Route Model            ↓
                                 ↓               Merge Response
                        Call Claude API              ↓
                                 ↓             Extract Response
                        Merge Response              ↓
                                 ↓          Save User Message
                        Extract Response      Save Assistant Message
                                 ↓          → gilgamesh_conversations
                        Save User Message         ↓
                        Save Assistant Message  Send Telegram
                        → gilgamesh_conversations ↓
                                 ↓          Count Conversations
                        Send Telegram             ↓
                                 ↓          Archive to Qdrant (async)
                        Count Conversations      ↓
                                 ↓          Langfuse Trace (async)
                        Archive to Qdrant (async) ↓
                                 ↓         Send to Personal Knowledge (async)
                        Langfuse Trace (async)
                                 ↓
                   Send to Personal Knowledge (async)
```

**Key Components:**

| Component | Details |
|-----------|---------|
| **Telegram Trigger** | POST endpoint listening for /messages and /callback_query |
| **Format Messages** | Builds conversation history (last 15 rows from gilgamesh_conversations), performs Qdrant RAG query, sets claude-sonnet-4 as default model |
| **If