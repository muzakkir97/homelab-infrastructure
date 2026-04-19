# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** April 19, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Phase 58 complete (Windrose Server Deployment). Game server deployed on CT 302 via Docker. 14 LXC containers running.

---

## 👤 About Me

| Field            | Details                                              |
|------------------|------------------------------------------------------|
| **Name**         | Muzakkir Kholil                                      |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location**     | Petaling Jaya, Selangor, Malaysia                    |
| **Target Role**  | Cloud Engineering / DevOps                           |
| **Domain**       | najhin-gaming.com (Cloudflare)                       |
| **GitHub**       | github.com/muzakkir97                                |

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

---

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                                     | IP Address     | Role                                 |
|------------------|----------|-------------------------------------------|----------------|--------------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB  | 192.168.10.5   | Hypervisor                           |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                  | 192.168.10.1   | Router, Firewall                     |
| Managed Switch   | —        | TP-Link TL-SG108E                         | 192.168.1.20   | Layer 2, VLANs                       |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple           | 192.168.10.15  | Backups (SMB target: kinmoon-smb)    |
| DNS Server       | —        | Raspberry Pi 4                            | 192.168.30.10  | Pi-hole (~489K domains blocked)      |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB          | 192.168.20.101 | **Gaming only — never homelab**      |
| WiFi AP          | —        | TP-Link EAP610                            | TBD            | Purchased, pending setup             |

### GPU Notes (Kuromoon)
- RX 6700 XT in IOMMU Group 18, audio in Group 19
- Earmarked for Ollama/ROCm AI inference (Phase 11)
- Idle baseline: CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan

---

## 🌐 Network Architecture

### Topology: Router-on-a-Stick

```
Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
                                              ↓
                                    802.1Q Trunk (igc2)
                                              ↓
                                    TP-Link TL-SG108E
                                              ↓
                    ┌─────────┬─────────┬─────────┬─────────┐
                  VLAN10   VLAN20   VLAN30   VLAN40   VLAN50
                  Mgmt     Main    Services   DMZ    Malware
```

### VLAN Design (OPERATIONAL)

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                |
|---------|-----------------|-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)     |

---

## 📦 Container Inventory (Proxmox LXC)

| CTID | Name                    | IP             | Subdomain                     | Autostart | Status     |
|------|-------------------------|----------------|-------------------------------|-----------|------------|
| 201  | nginx-proxy-manager     | 192.168.30.201 | —                             | ✅        | ✅ Running |
| 202  | monitoring-prometheus   | 192.168.30.202 | —                             | ✅        | ✅ Running |
| 203  | monitoring-grafana      | 192.168.30.203 | grafana.najhin-gaming.com     | ✅        | ✅ Running |
| 204  | monitoring-loki         | 192.168.30.204 | —                             | ✅        | ✅ Running |
| 205  | monitoring-alertmanager | 192.168.30.205 | —                             | ✅        | ✅ Running |
| 206  | monitoring-uptime       | 192.168.30.206 | —                             | ✅        | ✅ Running |
| 207  | network-ddns            | 192.168.30.207 | —                             | ✅        | ✅ Running |
| 208  | dashboard-homepage      | 192.168.30.208 | home.najhin-gaming.com        | ✅        | ✅ Running |
| 211  | automation-n8n          | 192.168.30.211 | n8n.najhin-gaming.com         | ✅        | ✅ Running |
| 213  | vault                   | 192.168.30.213 | vault.najhin-gaming.com       | ✅        | ✅ Running |
| 214  | password-vaultwarden    | 192.168.30.214 | passwords.najhin-gaming.com   | ✅        | ✅ Running |
| 220  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com       | ✅        | ✅ Running |
| 300  | gaming-panel            | 192.168.30.210 | —                             | ✅        | ✅ Running |
| 302  | gaming-wings-1          | 192.168.30.212 | terraria/mc.najhin-gaming.com | ✅        | ✅ Running |

**Total: 14 LXC containers, all on VLAN 30, all autostart enabled**

### Gaming Servers (Docker on CT 302)
- **Terraria** — terraria.najhin-gaming.com
- **Minecraft** — mc.najhin-gaming.com  
- **Windrose** — Deployed at /opt/windrose, 4 max players, Medium difficulty

---

## 🤖 Gilgamesh AI Agent (Phase 7C/7D)

### Architecture
```
Telegram (@JhinGilgamesh_bot) → n8n Workflow → Claude API → Response
                                     ↓
                              Memory (n8n Data Tables)
                                     ↓
                              /update → Nextcloud → GitHub
```

### Features
- **Conversation memory:** Last 20 messages stored in n8n Data Tables
- **Smart routing:** Simple inputs → Haiku 4.5, complex queries → Sonnet 4
- **Web search:** Real-time information via Claude's web_search tool
- **Cost tracking:** Token usage logged to gilgamesh_costs table
- **Inline keyboard menu:** Main menu with Homelab, Gaming, Gilgamesh, Tools, Help categories
- **Context sync:** /update command pushes session summaries to AI-CONTEXT.md via GitHub

### Technical Details

| Component      | Value                       |
|----------------|-----------------------------|
| Telegram Bot   | @JhinGilgamesh_bot          |
| Chat ID        | 510832696                   |
| Haiku Model    | claude-haiku-4-5-20251001   |
| Sonnet Model   | claude-sonnet-4-20250514    |
| n8n Container  | CT 211, 192.168.30.211      |
| Proxmox API    | root@pam!gilgamesh token    |
| Proxmox node   | muzakkir (not kuromoon)     |

### Inline Keyboard Menu Status

| Menu Item        | Status       |
|------------------|--------------|
| Main Menu        | ✅ Working   |
| Homelab → Status | ✅ Working   |
| Homelab → Metrics| 📋 Pending   |
| Homelab → Temps  | 📋 Pending   |
| Homelab → Storage| 📋 Pending   |
| Gaming submenu   | 📋 Pending   |
| Gilgamesh submenu| 📋 Pending   |
| Tools submenu    | 📋 Pending   |
| Help             | 📋 Pending   |

### Known Constraints
- Telegram messages must be **plain text only** — code blocks, backticks, and complex formatting break JSON parsing in the Claude API request body
- /update appends to Session Log only — no section-aware merging yet (planned redesign: file-based approach via Nextcloud)

---

## 💾 Backup Strategy (Phase 7A)

### Backup Jobs

| Job              | Schedule | Storage                  | Containers                              | Retention                    |
|------------------|----------|--------------------------|----------------------------------------|------------------------------|
| Small Containers | 02:00    | kinmoon-smb (NAS)        | 201, 202, 203, 204, 205, 206, 207, 214, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | data-storage (Local HDD) | 220, 302                               | 7 daily, 4 weekly, 2 monthly |

### Storage

| Name         | Type       | Protocol | Capacity | Purpose                 |
|--------------|------------|----------|----------|-------------------------|
| kinmoon-smb  | NAS        | SMB/CIFS | 3.6 TB   | Small container backups |
| data-storage | Local HDD  | Direct   | 7.2 TB   | Large container backups |
| local-lvm    | Local NVMe | LVM-Thin | 137 GB   | Container root disks    |
| vmpool-fast  | Local NVMe | ZFS      | 899 GB   | Fast container storage  |

---

## 🔒 Security Architecture

| Layer           | Implementation                                                                    |
|-----------------|-----------------------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                                                     |
| Segmentation    | 5 VLANs with enforced firewall rules                                              |
| DNS             | Pi-hole ad/tracker blocking (~489K domains)                                       |
| VPN             | Tailscale (subnet router on pfSense, primary access)                              |
| External Auth   | Cloudflare Access (email OTP) for Grafana, n8n, Vault, Vaultwarden (7 apps total) |
| External Access | Cloudflare Tunnel for Nextcloud                                                   |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)                                    |
| Backup          | Automated daily backups with 7/4/2 retention                                     |
| Secrets         | HashiCorp Vault (CT 213, vault.najhin-gaming.com). KV engine at kv/. Secrets: kv/gilgamesh, kv/cloudflare, kv/proxmox, kv/alertmanager, kv/github, kv/nextcloud, kv/n8n, kv/pihole (8 paths total) |
| Passwords       | Vaultwarden (CT 214, passwords.najhin-gaming.com). Personal password manager with Bitwarden clients. |

---

## 📋 Project Phase History

| Phase   | Title                                                     | Status         | Completed        |
|---------|-----------------------------------------------------------|----------------|------------------|
| 1       | Proxmox VE Installation                                   | ✅ Complete    | Jan 2026         |
| 2       | pfSense Firewall & VLAN Setup                             | ✅ Complete    | Jan 2026         |
| 3       | Core Services (Pi-hole, NPM, Tailscale, DDNS)             | ✅ Complete    | Jan 2026         |
| 4       | External Access & SSL                                     | ✅ Complete    | Feb 2026         |
| 5       | Monitoring Stack                                          | ✅ Complete    | Feb 2026         |
| 6A-6D   | Gaming Platform (Pterodactyl, Terraria, Minecraft)        | ✅ Complete    | Feb 2026         |
| 6E      | Homepage Dashboard                                        | ✅ Complete    | Mar 2026         |
| 6F      | Infrastructure Audit, VLAN Migration & Firewall Hardening | ✅ Complete    | Mar 9, 2026      |
| 7       | Nextcloud Deployment                                      | ✅ Complete    | Mar 8, 2026      |
| 7A      | Backup Strategy                                           | ✅ Complete    | Mar 13, 2026     |
| 7B      | n8n Workflow Automation                                   | ✅ Complete    | Apr 2, 2026      |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration               | ✅ Complete    | Apr 2, 2026      |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Web Search)      | ✅ Complete    | Apr 6, 2026      |
| 7D-Sec  | Cloudflare Access for n8n                                 | ✅ Complete    | Apr 7, 2026      |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                            | 🔄 In Progress | —                |
| 9       | NAS Deployment (Kinmoon)                                  | ✅ Complete    | Mar 3, 2026      |
| 13      | HashiCorp Vault — Secrets Manager                         | ✅ Complete    | Apr 18, 2026     |
| 23      | Vaultwarden + Secrets Audit & Cleanup                     | ✅ Complete    | Apr 18, 2026     |
| 58      | Windrose Server Deployment                                | ✅ Complete    | Apr 19, 2026     |
| 11      | Ollama + ROCm on Kuromoon RX 6700 XT                      | 📋 Planned     | —                |

---

## 🐛 Key Lessons Learned

### Network & VLANs

| Issue                            | Resolution                                          |
|----------------------------------|-----------------------------------------------------|
| Proxmox LXC VLAN not working     | Add `bridge-vids 2-4094` to /etc/network/interfaces |
| Intel i226-V NICs failing        | FreeBSD driver issue; only igc2/igc3 work on AC8F   |
| Double NAT breaking port forward | Port forward on BOTH ISP router AND pfSense         |
| Inter-VLAN DNS not resolving     | Use pfSense DNS Resolver forwarding to Pi-hole      |

### Firewall Hardening

| Issue                            | Resolution                                    |
|----------------------------------|-----------------------------------------------|
| Internet loss after rule changes | Source must be "subnets" not "address"        |
| Tailscale ping failing           | Normal — use `tailscale ping` instead of ICMP |
| Blocked traffic still works      | Disconnect Tailscale to test local firewall   |

### Backup Strategy

| Issue                        | Resolution                                            |
|------------------------------|-------------------------------------------------------|
| SMB/CIFS kernel crashes      | Switch to NFS protocol (later switched back to SMB)   |
| NFS permission denied        | Change squash to "Map all users to admin"             |
| Large file I/O errors on NAS | Hybrid approach — large containers to local storage   |
| ZFS snapshot busy            | Kill stuck processes or reboot, then destroy snapshot |
| Container locked (backup)    | `pct unlock <CTID>`                                   |

### n8n & Gilgamesh

| Issue                                | Resolution                                                      |
|--------------------------------------|-----------------------------------------------------------------|
| n8n Data Tables schema caching bug   | Delete and recreate the table entirely                          |
| Claude API 404 model not found       | Use exact model ID: `claude-haiku-4-5-20251001`                 |
| Web search partial response capture  | Add Extract Response Code node, filter by `type === "text"`     |
| Expression scoping issues            | Use Code node with `this.helpers.httpRequest`, reference `$json`|
| NFS fails for LXC backups            | UID namespace mapping (100000-165536); use SMB instead          |
| pfSense rule not working             | Place new rules BEFORE RFC1918 block rules                      |
| n8n GitHub credential 401            | User field (`muzakkir97`) must be populated                     |
| Telegram single webhook constraint   | All update types (message + callback) must use one trigger      |
| Answer Callback node loses data      | Bypass it; handle callback acknowledgment separately            |

### HashiCorp Vault (Phase 13)

| Issue                                | Resolution                                                      |
|--------------------------------------|-----------------------------------------------------------------|
| apt fails in new LXC                 | Run `echo "nameserver 192.168.30.10" > /etc/resolv.conf` first |
| HashiCorp repo setup fails           | Install `lsb-release` before adding repo                       |
| Wrong repo architecture/codename    | Hardcode `amd64` and `bookworm` in repo entry                   |
| Vault fails to start in LXC          | Add `disable_mlock = true` to vault.hcl                        |
| Token auth issues in shell           | Use `vault login <token>` not `export VAULT_TOKEN=`            |

---

## 🤖 Bots & Integrations

| Bot | Platform | Source | Status | Purpose |
|-----|----------|--------|--------|---------|
| @JhinGilgamesh_bot | Telegram | n8n CT 211 | ✅ Active | Personal AI agent — chat, homelab control, /update |
| Homelab Alerts | Telegram | Alertmanager CT 205 | ✅ Active | Critical alerts (host down, high CPU/memory/disk) |
| Homelab Alerts | Discord webhook | Alertmanager CT 205 | ✅ Active | Warning-level alerts to #alerts channel |

**Planned:** Migrate Alertmanager alerts to route through n8n first (central hub). Game server notifications to Discord via n8n.

---

## ❓ Pending Tasks

### Immediate
| Task | Priority |
|------|----------|
| Share Windrose invite code with friends | High |
| Monitor RAM usage with Windrose running | High |
| Set up Bitwarden app on phone with passwords.najhin-gaming.com server | High |
| Update hardcoded PAT in Prepare GitHub Push to use n8n credential instead | Medium |
| Update Cloudflare Access app icons (all 7 apps) | Medium |

### Gilgamesh
| Task | Priority |
|------|----------|
| Complete menu submenus (Metrics, Temps, Storage, Gaming, Gilgamesh, Tools, Help) | Medium |
| /update redesign — file attachment via Telegram, push to GitHub + Nextcloud | Medium |
| Homepage embedded Gilgamesh chat UI (web frontend, shared memory with Telegram) | Medium |
| Integrate Vault secrets into n8n Gilgamesh workflow | Medium |

### Infrastructure
| Task | Priority |
|------|----------|
| Migrate Alertmanager alerts through n8n (central notification hub) | Medium |
| Switch management IP (192.168.1.20 → 192.168.10.20) | Medium |
| Remove legacy LAN 192.168.1.0/24 (after switch migration) | Medium |
| WiFi Access Point setup (EAP610 purchased) | Medium |
| Homepage security (Cloudflare Access) | Low |
| Off-site backup (Backblaze B2) | Low |
| Nextcloud 2FA (TOTP) | Low |

### Deferred
| Task | Priority |
|------|----------|
| Phase 11: Ollama + ROCm local LLM inference (RX 6700 XT) | Deferred |
| Fedora dual-boot on Minimoon (kernel 6.12+ for RDNA 4) | Deferred |
| Old P400S build repurpose (Z270E + i7-7700K) | Deferred |
| Claude Project auto-sync — revisit if Anthropic releases Project Files API | Deferred |

---

## 📝 Session Log (Recent)

### April 19, 2026
Date: April 19, 2026
Phase: Phase 58 — Windrose Server Deployment
Topics Discussed

Deployed Windrose dedicated server via Docker on CT 302
Used indifferentbroccoli/windrose-server-docker image
Server running at /opt/windrose on gaming-wings-1
Invite code set, server confirmed working — connected successfully

Decisions Made

Game servers deployed as Docker Compose on CT 302 alongside Pterodactyl
Windrose server set to 4 max players, Medium difficulty

Changes to AI-CONTEXT.md

Add Windrose server to gaming section
Note Docker deployment path /opt/windrose on CT 302

Errors & Resolutions

None this session

Action Items

Share invite code with friends
Monitor RAM usage with Windrose running

### April 18, 2026 (Part 2)
Date: April 18, 2026
Phase: Phase 23 — Vaultwarden + Secrets Audit & Cleanup
Topics Discussed

Deployed Vaultwarden (CT 214, 192.168.30.214, passwords.najhin-gaming.com)
Full secrets audit across all 14 containers
Migrated all API keys to HashiCorp Vault kv/ (8 paths total)
Stored all service logins and API keys in Vaultwarden (~23 entries)
Cleaned up duplicate Cloudflare API tokens (9 to 3)
Cleaned up duplicate n8n GitHub credentials (3 to 1)
Generated new GitHub PAT (fine-grained, n8n-gilgamesh, Contents read/write)
Fixed /update workflow — Nextcloud 404 and GitHub 404 (removed /docs from paths)
Deleted old docs/AI-CONTEXT.md from GitHub
Deleted plaintext notepad secrets file

Decisions Made

Vaultwarden for personal password reference, Vault for machine secrets — API keys stored in both
Cloudflare Access app icons skipped (UI doesn't support custom icons easily)
GitHub PAT set with fine-grained permissions (Contents read/write, homelab-infrastructure repo only)
Google Sign-In services (Anthropic, Tailscale) stored with "Google Sign-In" as password field

### April 18, 2026
- Deployed HashiCorp Vault as Phase 13 (CT 213, 192.168.30.213)
- Vault v2.0.0 installed via HashiCorp apt repo on Debian 12
- Configured file storage backend, `disable_mlock = true` (required for LXC)
- Initialized with 5 unseal keys, threshold 3
- KV secrets engine enabled at path `kv/`
- Secrets migrated from plaintext Notepad: kv/gilgamesh, kv/cloudflare, kv/proxmox
- Created homelab policy (kv/* + secret/*) and non-root homelab token
- NPM reverse proxy configured for vault.najhin-gaming.com
- Cloudflare Access email OTP protection added (6th protected app)

### April 7, 2026
- Built Gilgamesh inline keyboard menu system (Phase 7D-Menu)
- Main Menu with 5 categories: Homelab, Gaming, Gilgamesh, Tools, Help
- Homelab → Status action working (queries Proxmox API, displays all container states)
- Proxmox API token created: root@pam!gilgamesh
- Proxmox node name confirmed as "muzakkir" (not kuromoon)
- Added Cloudflare Access protection to n8n (email OTP)

### April 6, 2026
- Completed Phase 7D (Gilgamesh Enhancements)
- Fixed Save Cost node — recreated gilgamesh_costs table
- Fixed Save Assistant Message — Extract Response node for web search multi-block responses
- Implemented smart routing: Haiku 4.5 for simple queries, Sonnet 4 for complex
- Fixed /update workflow: Cloudflare Access bypass for webhook paths, session log insert at top

---

*Last updated: April 19, 2026 — Update this file at the end of each session before pushing to GitHub*