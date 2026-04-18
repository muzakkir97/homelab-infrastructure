# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** April 7, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Phase 7D complete (Gilgamesh AI Agent). Telegram-based AI assistant with conversation memory, smart routing, cost tracking, and web search. n8n secured with Cloudflare Access.

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

| Device           | Hostname | Specs                                    | IP Address     | Role                                 |
|------------------|----------|------------------------------------------|----------------|--------------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB | 192.168.10.5   | Hypervisor                           |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                 | 192.168.10.1   | Router, Firewall                     |
| Managed Switch   | —        | TP-Link TL-SG108E                        | 192.168.1.20   | Layer 2, VLANs                       |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple          | 192.168.10.15  | Backups (SMB target: kinmoon-smb)    |
| DNS Server       | —        | Raspberry Pi 4                           | 192.168.30.10  | Pi-hole (~489K domains blocked)      |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB         | 192.168.20.101 | **Gaming only — never homelab**      |
| WiFi AP          | —        | TP-Link EAP610                           | TBD            | Purchased, pending setup             |

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

| CTID | Name                    | IP             | Subdomain                   | Autostart | Status    |
|------|-------------------------|----------------|-----------------------------|-----------|-----------|
| 201  | nginx-proxy-manager     | 192.168.30.201 | —                           | ✅        | ✅ Running |
| 202  | monitoring-prometheus   | 192.168.30.202 | —                           | ✅        | ✅ Running |
| 203  | monitoring-grafana      | 192.168.30.203 | grafana.najhin-gaming.com   | ✅        | ✅ Running |
| 204  | monitoring-loki         | 192.168.30.204 | —                           | ✅        | ✅ Running |
| 205  | monitoring-alertmanager | 192.168.30.205 | —                           | ✅        | ✅ Running |
| 206  | monitoring-uptime       | 192.168.30.206 | —                           | ✅        | ✅ Running |
| 207  | network-ddns            | 192.168.30.207 | —                           | ✅        | ✅ Running |
| 208  | dashboard-homepage      | 192.168.30.208 | home.najhin-gaming.com      | ✅        | ✅ Running |
| 211  | automation-n8n          | 192.168.30.211 | n8n.najhin-gaming.com       | ✅        | ✅ Running |
| 220  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com     | ✅        | ✅ Running |
| 300  | gaming-panel            | 192.168.30.210 | —                           | ✅        | ✅ Running |
| 302  | gaming-wings-1          | 192.168.30.212 | terraria/mc.najhin-gaming.com | ✅      | ✅ Running |

**Total: 12 LXC containers, all on VLAN 30, all autostart enabled**

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
- **Context sync:** /update command pushes session summaries to AI-CONTEXT.md via GitHub

### Technical Details
| Component | Value |
|-----------|-------|
| Telegram Bot | @JhinGilgamesh_bot |
| Chat ID | 510832696 |
| Haiku Model | claude-haiku-4-5-20251001 |
| Sonnet Model | claude-sonnet-4-20250514 |
| n8n Container | CT 211, 192.168.30.211 |

### Known Constraint
Telegram messages must be **plain text only** — code blocks, backticks, and complex formatting break JSON parsing in the Claude API request body.

---

## 💾 Backup Strategy (Phase 7A)

### Backup Jobs

| Job              | Schedule | Storage                  | Containers                             | Retention                    |
|------------------|----------|--------------------------|----------------------------------------|------------------------------|
| Small Containers | 02:00    | kinmoon-smb (NAS)        | 201, 202, 203, 204, 205, 206, 207, 300 | 7 daily, 4 weekly, 2 monthly |
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

| Layer           | Implementation                                        |
|-----------------|-------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                         |
| Segmentation    | 5 VLANs with enforced firewall rules                  |
| DNS             | Pi-hole ad/tracker blocking (~489K domains)           |
| VPN             | Tailscale (subnet router on pfSense, primary access)  |
| External Auth   | Cloudflare Access (email OTP) for Grafana and n8n     |
| External Access | Cloudflare Tunnel for Nextcloud                       |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)         |
| Backup          | Automated daily backups with 7/4/2 retention          |

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
| **7D**  | **Gilgamesh Enhancements (Memory, Routing, Web Search)**  | ✅ **Complete**| **Apr 6, 2026**  |
| 7D-Sec  | Cloudflare Access for n8n                                 | ✅ Complete    | Apr 7, 2026      |
| 9       | NAS Deployment (Kinmoon)                                  | ✅ Complete    | Mar 3, 2026      |
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

---

## ❓ Pending Tasks

| Task                                                | Priority |
|-----------------------------------------------------|----------|
| Obsidian setup with Dataview subscription tracker   | High     |
| Switch management IP (192.168.1.20 → 192.168.10.20) | Medium   |
| WiFi Access Point setup (EAP610 purchased)          | Medium   |
| Section-aware merge logic for AI-CONTEXT.md         | Low      |
| Off-site backup (Backblaze B2)                      | Low      |
| Nextcloud 2FA (TOTP)                                | Low      |
| Phase 11: Ollama + ROCm local LLM inference         | Deferred |
| Fedora dual-boot on Minimoon (kernel 6.12+ for RDNA 4) | Deferred |
| Old P400S build repurpose (Z270E + i7-7700K)        | Deferred |

---

## 📝 Session Log (Recent)

### April 18, 2026
Date: April 18, 2026 Phase: Phase 13 - HashiCorp Vault Topics Discussed: Deployed HashiCorp Vault CT 213 on VLAN 30. Resolved LXC issues including template filename, nesting, DNS fix, mlock error, locale warnings. Configured NPM reverse proxy and Cloudflare Access email OTP. Initialized Vault with 5 unseal keys threshold 3. Created homelab policy and non-root token. Enabled KV secrets engine at kv/. Migrated secrets from plaintext Notepad to Vault. Discussed Bitwarden for personal passwords. What Was Built: CT 213 Vault v2.0.0 with file storage backend and disable_mlock true. KV engine at kv/ with secrets kv/gilgamesh (claude-api-key, telegram-bot-token), kv/cloudflare (global-api-key), kv/proxmox (api-token). Homelab policy covering kv/* and secret/*. vault.najhin-gaming.com accessible behind Cloudflare Access. Technical Details: vault.hcl uses disable_mlock true, storage file at /opt/vault/data, listener on 0.0.0.0:8200 tls_disable true. KV engine path is kv/ not secret/. Use vault login to authenticate in shell to avoid non-printable token issues. Key Lessons Learned: New LXC needs echo nameserver 192.168.30.10 > /etc/resolv.conf before apt. Install lsb-release before HashiCorp repo setup. Hardcode amd64 and bookworm in repo entry. Add disable_mlock true for LXC. Use vault login not export for token auth. Changes to AI-CONTEXT.md: Add CT 213 vault 192.168.30.213 vault.najhin-gaming.com to container inventory. Update Phase 13 to complete April 18 2026. Update secrets management to reflect Vault storage. Pending Tasks: Clear Notepad secrets. Setup Bitwarden or Vaultwarden. Clean up duplicate Cloudflare API tokens. Update Cloudflare Access app icons for all 6 apps. Future: integrate Vault into n8n Gilgamesh workflow.


### April 7, 2026
Date: April 7, 2026
Phase: Phase 7E - Gilgamesh Menu System
Topics Discussed

Expanded n8n workflow ideas master list (Homelab, Finance, HR, WhatsApp, AI-powered, etc.)
Malaysia SME market potential for n8n automation services
Built inline keyboard button menu system for Gilgamesh
Proxmox API integration for container status

What Was Built

Telegram inline keyboard menu system with nested navigation
Main Menu with 5 categories: Homelab, Gaming, Gilgamesh, Tools, Help
Homelab submenu with Status, Metrics, Temps, Storage, Back buttons
Callback Router to handle button presses
Proxmox API integration with new token (root@pam!gilgamesh)
Status action that queries all LXC containers and displays status/RAM
Edit message functionality (menus update in place instead of sending new messages)

Technical Details

Telegram Trigger configured for both Message and Callback Query
Main Switch routes: Callback, Menu, Update, Fallback
Callback Router routes: Homelab, Gaming, Gilgamesh, Tools, Help, Back, Status, Fallback
Proxmox API URL: https://192.168.10.5:8006/api2/json/nodes/muzakkir/lxc
Proxmox node name is "muzakkir" (not kuromoon)
Answer Callback node currently deactivated (was breaking data flow)
Expression for multi-line text: {{ "Line1\n\nLine2" }}
Expression for conditional chat ID: {{ $json.callback_query ? $json.callback_query.message.chat.id : $json.message.chat.id }}

Key Lessons Learned

Telegram allows only ONE webhook per bot - must handle both messages and callbacks in same trigger
Answer Callback node transforms data and loses original callback_query - bypass it or restructure flow
Use "Edit a text message" instead of "Send" for submenu navigation (cleaner UX)
n8n Switch node needs "Convert types where required" enabled when checking optional fields
Proxmox API requires: Header Auth with "Authorization: PVEAPIToken=user@realm!token=secret"
Enable "Ignore SSL Issues" for self-signed Proxmox certificate

Changes to AI-CONTEXT.md
Add to Current State section:

Phase 7E: Gilgamesh Menu System (in progress)
Proxmox API token created: root@pam!gilgamesh
Proxmox node name: muzakkir (not kuromoon)

Add to Gilgamesh section:

Inline keyboard menu system implemented
Main menu categories: Homelab, Gaming, Gilgamesh, Tools, Help
Homelab submenu: Status (working), Metrics, Temps, Storage (pending)
Status action queries Proxmox API and displays all container states

Pending Tasks

Build remaining Homelab actions: Metrics, Temps, Storage
Build Gaming submenu and actions
Build Gilgamesh submenu: Memory info, API costs, Clear memory
Build Tools submenu: DNS stats, Uptime, Alerts
Build Help action
Re-enable Answer Callback (optional - removes button loading spinner)
Consider renaming Proxmox node from "muzakkir" to "kuromoon" (low priority)

Workflow Ideas Documented

Full master list of n8n workflows compiled
Malaysia SME market analysis with pricing (RM 1,500 - 15,000 range)
High potential: WhatsApp lead auto-reply, Invoice reminders, HR onboarding, AI customer support


### April 6, 2026
Fixed Gilgamesh update workflow
- Added Cloudflare Access bypass for webhook paths
- Fixed session log date format to match existing entries
- New entries now insert at top of Session Log section


### 2026-04-06
Test session log ordering April 7
- Testing new merge logic
- Entry should appear at top of Session Log section


### 2026-04-06
Test session log ordering
- Testing new merge logic
- New entries should appear at top of Session Log


### 2026-04-06
Test session log ordering
- Testing new merge logic
- New entries should appear at top of Session Log


### April 7, 2026
- Added Cloudflare Access protection to n8n (email OTP)
- n8n now secured same as Grafana
- Updated AI-CONTEXT.md with consolidated state

### April 6, 2026
- Completed Phase 7D (Gilgamesh Enhancements)
- Fixed Save Cost node — recreated gilgamesh_costs table
- Fixed Save Assistant Message — Extract Response node for web search
- Implemented smart routing: Haiku 4.5 for simple, Sonnet 4 for complex
- Added Obsidian phase to roadmap

### April 5, 2026
- Implemented conversation memory using n8n Data Tables
- Memory stores last 20 messages (role, content, timestamp)
- Added web search capability to Gilgamesh
- Fixed HTTP Request JSON formatting, If node routing, Send text message

### April 4, 2026
- Planned Gilgamesh upgrade (MCP evaluation, security model)
- Decided: Keep Claude Pro + upgrade Gilgamesh with API
- Phase 7D added to roadmap

### April 2, 2026
- Completed Phase 7C: Gilgamesh Telegram bot
- Created workflow: Telegram → Claude → Nextcloud → GitHub
- /update command working for context sync

### March 16, 2026
- Completed Phase 7A fully (pfSense + Pi-hole backups)
- Enhanced Pi-hole blocklists: 81K → 489K domains
- Purchased TP-Link EAP610 WiFi AP

---

*Last updated: April 7, 2026 — Update this file at the end of each session before pushing to GitHub*
