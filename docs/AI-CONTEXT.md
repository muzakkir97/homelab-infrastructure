# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** March 13, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Phase 7A complete (Backup Strategy). Hybrid backup system operational with NFS for small containers and local storage for large containers. All 10 containers backed up automatically.

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

### Naming Conventions

- **Function-based names:** `monitoring-prometheus`, `gaming-panel`, `network-ddns`
- **Moon theme for hardware:** Kuromoon (Proxmox), Kinmoon (NAS), Minimoon (Gaming PC)

---

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                           | IP Address     | Role                                 |
|------------------|----------|---------------------------------|----------------|--------------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM         | 192.168.10.5   | Hypervisor                           |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100        | 192.168.10.1   | Router, Firewall                     |
| Managed Switch   | —        | TP-Link TL-SG108E               | 192.168.1.20   | Layer 2, VLANs                       |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple | 192.168.10.15  | Backups (NFS)                        |
| DNS Server       | —        | Raspberry Pi 4                  | 192.168.30.10  | Pi-hole                              |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D                 | 192.168.20.101 | Personal + Tailscale (100.106.109.4) |

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

| CTID | Name                    | IP             | Autostart | Boot Order | Backup Target | Status    |
|------|-------------------------|----------------|-----------|------------|---------------|-----------|
| 201  | nginx-proxy-manager     | 192.168.30.201 | ✅         | 1          | kinmoon-nfs   | ✅ Running |
| 202  | monitoring-prometheus   | 192.168.30.202 | ✅         | 3          | kinmoon-nfs   | ✅ Running |
| 203  | monitoring-grafana      | 192.168.30.203 | ✅         | 6          | kinmoon-nfs   | ✅ Running |
| 204  | monitoring-loki         | 192.168.30.204 | ✅         | 4          | kinmoon-nfs   | ✅ Running |
| 205  | monitoring-alertmanager | 192.168.30.205 | ✅         | 5          | kinmoon-nfs   | ✅ Running |
| 206  | monitoring-uptime       | 192.168.30.206 | ✅         | 7          | kinmoon-nfs   | ✅ Running |
| 207  | network-ddns            | 192.168.30.207 | ✅         | 2          | kinmoon-nfs   | ✅ Running |
| 220  | nextcloud               | 192.168.30.220 | ✅         | 8          | data-storage  | ✅ Running |
| 300  | gaming-panel            | 192.168.30.210 | ✅         | 9          | kinmoon-nfs   | ✅ Running |
| 302  | gaming-wings-1          | 192.168.30.212 | ✅         | 10         | data-storage  | ✅ Running |

---

## 💾 Backup Strategy (Phase 7A)

### Backup Jobs

| Job              | Schedule | Storage                  | Containers                             | Retention                    |
|------------------|----------|--------------------------|----------------------------------------|------------------------------|
| Small Containers | 02:00    | kinmoon-nfs (NAS)        | 201, 202, 203, 204, 205, 206, 207, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | data-storage (Local HDD) | 220, 302                               | 7 daily, 4 weekly, 2 monthly |

### Storage

| Name         | Type       | Protocol | Capacity | Purpose                 |
|--------------|------------|----------|----------|-------------------------|
| kinmoon-nfs  | NAS        | NFS v3   | 3.6 TB   | Small container backups |
| data-storage | Local HDD  | Direct   | 7.2 TB   | Large container backups |
| local-lvm    | Local NVMe | LVM-Thin | 137 GB   | Container root disks    |
| vmpool-fast  | Local NVMe | ZFS      | 899 GB   | Fast container storage  |

### Why Hybrid?

- **UGREEN NAS limitation:** SMB/CIFS caused kernel crashes; NFS works but fails on sustained large writes (>10GB)
- **Solution:** Small containers → NFS, Large containers (Nextcloud 17GB, Wings 12GB) → Local storage

---

## 📋 Project Phase History

| Phase  | Title                                                     | Status         | Completed        |
|--------|-----------------------------------------------------------|----------------|------------------|
| 1      | Proxmox VE Installation                                   | ✅ Complete     | Jan 2026         |
| 2      | pfSense Firewall & VLAN Setup                             | ✅ Complete     | Jan 2026         |
| 3      | Core Services (Pi-hole, NPM, Tailscale, DDNS)             | ✅ Complete     | Jan 2026         |
| 4      | External Access & SSL                                     | ✅ Complete     | Feb 2026         |
| 5      | Monitoring Stack                                          | ✅ Complete     | Feb 2026         |
| 6A-6D  | Gaming Platform (Pterodactyl, Terraria)                   | ✅ Complete     | Feb 2026         |
| 9      | NAS Deployment (Kinmoon)                                  | ✅ Complete     | Mar 3, 2026      |
| 6F     | Infrastructure Audit, VLAN Migration & Firewall Hardening | ✅ Complete     | Mar 9, 2026      |
| 7      | Nextcloud Deployment                                      | ✅ Complete     | Mar 8, 2026      |
| **7A** | **Backup Strategy**                                       | ✅ **Complete** | **Mar 13, 2026** |
| 6E     | Homepage Dashboard                                        | 📋 Planned     | —                |
| 7B     | n8n Workflow Automation                                   | 📋 Planned     | —                |

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

### Backup Strategy (Phase 7A)

| Issue                        | Resolution                                            |
|------------------------------|-------------------------------------------------------|
| SMB/CIFS kernel crashes      | Switch to NFS protocol                                |
| NFS permission denied        | Change squash to "Map all users to admin"             |
| Large file I/O errors on NAS | Hybrid approach — large containers to local storage   |
| ZFS snapshot busy            | Kill stuck processes or reboot, then destroy snapshot |
| Container locked (backup)    | `pct unlock <CTID>`                                   |

---

## 🔒 Security Architecture

| Layer           | Implementation                                   |
|-----------------|--------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                    |
| Segmentation    | 5 VLANs with enforced firewall rules             |
| DNS             | Pi-hole ad/tracker blocking                      |
| VPN             | Tailscale (subnet router on pfSense + Gaming PC) |
| External Auth   | Cloudflare Access (email OTP) for Grafana        |
| External Access | Cloudflare Tunnel for Nextcloud                  |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)    |
| Backup          | Automated daily backups with 7/4/2 retention     |

---

## ❓ Pending Tasks

| Task                                                | Priority |
|-----------------------------------------------------|----------|
| WiFi Access Point for VLAN 20 (phone Pi-hole)       | Medium   |
| Switch management IP (192.168.1.20 → 192.168.10.20) | Medium   |
| Remove legacy LAN interface                         | Medium   |
| Homepage Dashboard (Phase 6E)                       | Medium   |
| Off-site backup (Backblaze B2)                      | Low      |
| Nextcloud 2FA (TOTP)                                | Low      |

---

## 📝 Session Log (Recent)

### March 16, 2026

- Completed Phase 7A fully (pfSense + Pi-hole backups done)
- Downloaded pfSense config XML to Gaming PC + Nextcloud
- Downloaded Pi-hole Teleporter backup to Gaming PC + Nextcloud
- Enhanced Pi-hole blocklists: 81K → 488K domains (6x increase)
- Added 3 new blocklists: Easyprivacy, AdguardDNS, Prigent-Malware
- Investigated WiFi options for phone Pi-hole access
- Added WiFi Access Point to roadmap (TP-Link EAP225 recommended)

### March 13, 2026

- Completed Phase 7A (Backup Strategy)
- Discovered SMB/CIFS kernel instability during large backups
- Switched NAS backup protocol from SMB to NFS
- Fixed NFS permission issues (squash settings)
- Discovered NAS limitations with sustained large writes
- Implemented hybrid backup: small containers → NFS, large → local
- Created two backup jobs with proper retention (7/4/2)
- Tested and verified all containers backup successfully
- Removed disabled SMB storage

### March 9, 2026

- Completed firewall hardening (Phase 6F final part)
- Created/updated firewall aliases
- Implemented proper inter-VLAN rules
- Installed Tailscale on Gaming PC for admin access

### March 8, 2026

- Deployed Nextcloud (CT 220) with LAMP stack
- Configured Cloudflare Tunnel for external access
- Fixed Uptime Kuma monitors
- Configured container autostart with boot order

---

*Last updated: March 16, 2026 — Update this file at the end of each session before pushing to GitHub*

### April 2, 2026

- Phase 7C: Built Telegram agent bot (Gilgamesh)
- Connected Claude Sonnet 4 API to n8n
- Created workflow: Telegram → Claude → Parse → Nextcloud
- Implemented append logic to prevent file overwrites
- April 2, 2026: Completed Phase 7C-2 - GitHub push integration in Telegram Agent workflow
- Full pipeline working: Telegram to Claude to Nextcloud to GitHub
- April 2, 2026: Completed Phase 7C-2 - GitHub push integration in Telegram Agent workflow
- Full pipeline working: Telegram to Claude to Nextcloud to GitHub

**April 4 2026 - Gilgamesh Upgrade Planning**
- Discussed MCP benefits and security risks
- Designed tiered permission model with Telegram 2FA
- Compared MCP vs OpenClaw vs Claude Code vs Clawdbot
- OpenClaw rejected due to security risk
- Claude Code has no memory so not a replacement
- Final decision: keep Claude Pro plus upgrade Gilgamesh
- Upgrade includes smart routing and SQLite memory and cost tracking
- Adding homelab commands: status, restart, metrics
- Phase 7D added to roadmap
- Bug noted: Gilgamesh breaks on special characters


### 2026-04-04
Session April 5 2026 Phase 7D Implementation
- Implemented conversation memory using n8n Data Tables
- Memory stores last 20 messages (role, content, timestamp)
- Fixed HTTP Request JSON formatting for Claude API
- Fixed If node routing (checks for /update command)
- Fixed Send a text message node to return Claude response
- Fixed Parse File to extract update content directly
- Smart routing temporarily disabled (Sonnet only for now)
- Save Cost node disabled pending column name fix
- Gilgamesh now remembers context across messages
- Memory system verified working with name recall test



### 2026-04-04
Session April 5 2026 Phase 7D Complete
- Upgraded to Claude Sonnet 4.6 (latest model)
- Added web search capability (web_search tool)
- Conversation memory working (n8n Data Tables, 20 messages)
- Fixed workflow routing (If node, Send text message)
- Fixed Parse File for direct /update extraction
- Gilgamesh can now search the web for real-time information
- Memory, web search, and /update all verified working
