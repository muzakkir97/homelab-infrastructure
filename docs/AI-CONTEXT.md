# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** March 6, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Phase 6F (Infrastructure Audit & Correction) — 75% complete. Security fixes done, VLAN migration pending.

---

## 👤 About Me

| Field | Details |
|-------|---------|
| **Name** | Muzakkir Kholil |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location** | Kuala Lumpur, Malaysia |
| **Target Role** | Cloud Engineering / DevOps |
| **Domain** | najhin-gaming.com (Cloudflare) |
| **GitHub** | github.com/muzakkir97 |

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

| Device | Hostname | Specs | IP Address | Role |
|--------|----------|-------|------------|------|
| Proxmox Server | Kuromoon | Ryzen 5 5600X, 32GB RAM | 192.168.10.5 | Hypervisor |
| pfSense Firewall | — | AC8F Mini PC, Intel N100 | 192.168.10.1 | Router, Firewall |
| Managed Switch | — | TP-Link TL-SG108E | 192.168.1.20 | Layer 2, VLANs |
| NAS | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | 192.168.1.15 | Backups (SMB) |
| DNS Server | — | Raspberry Pi 4 | 192.168.20.10 | Pi-hole |
| Gaming PC | Minimoon | Ryzen 7 7800X3D | — | Personal (not homelab) |

### Known Hardware Issues
- **pfSense NIC:** Only 2 of 4 Intel i226-V ports work (igc2, igc3) due to FreeBSD driver issues
- **NAS drives:** WD Green 1TB SSD and Kingston 120GB NVMe not detected (investigation pending)

---

## 🌐 Network Architecture

### Topology: Router-on-a-Stick
```
Internet → ISP Router (192.168.100.1) → pfSense (WAN: 192.168.100.169)
                                              ↓
                                    802.1Q Trunk (igc2)
                                              ↓
                                    TP-Link TL-SG108E
                                              ↓
                    ┌─────────┬─────────┬─────────┬─────────┐
                  VLAN10   VLAN20   VLAN30   VLAN40   VLAN50
                  Mgmt     Main    Services   DMZ    Malware
```

### VLAN Design (TARGET — after migration)

| VLAN ID | Name | Subnet | Gateway | Purpose |
|---------|------|--------|---------|---------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, Switch, NAS) |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | 192.168.20.1 | Client devices |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (no DHCP, air-gapped) |

### Current State (BEFORE migration)
- Services are scattered across wrong VLANs
- Legacy LAN (192.168.1.0/24) still exists with Proxmox and NAS
- VLAN migration requires ~2 hours downtime with physical access

---

## 📦 Container Inventory (Proxmox LXC)

| CTID | Name | Current IP | Target IP | Status |
|------|------|------------|-----------|--------|
| 201 | nginx-proxy-manager | 192.168.30.10 | 192.168.30.201 | ✅ Running |
| 202 | monitoring-prometheus | 192.168.20.11 | 192.168.30.202 | ⏳ Migration pending |
| 203 | monitoring-grafana | 192.168.20.12 | 192.168.30.203 | ⏳ Migration pending |
| 204 | monitoring-loki | 192.168.20.x | 192.168.30.204 | ⏳ Migration pending |
| 205 | monitoring-alertmanager | 192.168.20.14 | 192.168.30.205 | ⏳ Migration pending |
| 206 | monitoring-uptime | 192.168.20.x | 192.168.30.206 | ✅ Running |
| 207 | network-ddns | 192.168.20.x | 192.168.30.207 | ✅ Running |
| 300 | gaming-panel | 192.168.50.10 | 192.168.30.210 | ✅ Running |
| 302 | gaming-wings-1 | 192.168.50.12 | 192.168.30.212 | ✅ Running |

### Storage
| ID | Type | Location | Purpose |
|----|------|----------|---------|
| local | Directory | /var/lib/vz | ISOs, templates |
| local-lvm | LVM-Thin | pve/data | VM/CT disks |
| kinmoon-smb | SMB/CIFS | 192.168.1.15 | Backups |

---

## 🔧 Services Deployed

### Core Infrastructure
- **pfSense** — Firewall, DHCP, NAT, inter-VLAN routing
- **Pi-hole** — DNS filtering, ad blocking (Raspberry Pi 4)
- **Nginx Proxy Manager** — Reverse proxy, Let's Encrypt SSL
- **Tailscale** — VPN (subnet router on pfSense)
- **Cloudflare DDNS** — Dynamic DNS updates

### Monitoring Stack
- **Prometheus** — Metrics collection
- **Grafana** — Dashboards (external via grafana.najhin-gaming.com + Cloudflare Access)
- **Loki** — Log aggregation
- **Alertmanager** — Alert routing (Telegram for critical, Discord for warnings)
- **Uptime Kuma** — Service availability monitoring

### Gaming Platform
- **Pterodactyl Panel** — Game server management UI
- **Pterodactyl Wings** — Game server daemon (Docker)
- **Terraria** — tModLoader + Calamity mod (port 7777)
- **Minecraft** — Paper server (port 25570, currently offline)

---

## 📋 Project Phase History

| Phase | Title | Status | Completed |
|-------|-------|--------|-----------|
| 1 | Proxmox VE Installation | ✅ Complete | Jan 2026 |
| 2 | pfSense Firewall & VLAN Setup | ✅ Complete | Jan 2026 |
| 3 | Core Services (Pi-hole, NPM, Tailscale, DDNS) | ✅ Complete | Jan 2026 |
| 4 | External Access & SSL | ✅ Complete | Feb 2026 |
| 5 | Monitoring Stack | ✅ Complete | Feb 2026 |
| 6A-6D | Gaming Platform (Pterodactyl, Terraria) | ✅ Complete | Feb 2026 |
| 9 | NAS Deployment (Kinmoon) | ✅ Complete | Mar 3, 2026 |
| **6F** | **Infrastructure Audit & Correction** | **🔧 75%** | **In Progress** |
| 6E | Homepage Dashboard | 📋 Planned | — |
| 7A | Backup Strategy | 📋 Planned (Priority) | — |
| 7B | n8n Workflow Automation | 📋 Planned | — |
| 7C | AI Agent (OpenClaw) | 📋 Planned | — |
| 8 | Gaming Expansion | 📋 Planned | — |

---

## 🔧 Phase 6F: Current Work

### Completed ✅
1. **Security fixes:**
   - Removed Cloudflare DNS records: proxmox, pihole, npm, wildcard (*)
   - Cleaned NPM proxy hosts (only Grafana exposed now)
   - Deleted orphaned Cloudflare Access application
   
2. **VLAN migration planning:**
   - Documented all current IPs
   - Backed up pfSense config
   - Backed up all LXC configs to `/root/vlan-migration-backup/`
   - Created comprehensive migration guide

### Pending ⏳
3. **VLAN migration execution** (requires ~2hr downtime window):
   - Rename VLANs in pfSense to match design
   - Update switch VLAN configuration
   - Create Proxmox vmbr0.10 subinterface
   - Migrate all 9 containers to VLAN 30
   - Update Pi-hole IP, pfSense DNS
   - Update firewall rules

4. **Post-migration:**
   - Configure container autostart
   - Add inter-VLAN firewall rules (currently allow-all)
   - Update Uptime Kuma monitors
   - Update Prometheus targets

---

## 🐛 Key Lessons Learned

### Network & VLANs
| Issue | Resolution |
|-------|------------|
| Proxmox LXC VLAN not working | Add `bridge-vids 2-4094` to /etc/network/interfaces |
| Intel i226-V NICs failing | FreeBSD driver issue; only igc2/igc3 work on AC8F |
| Double NAT breaking port forward | Port forward on BOTH ISP router AND pfSense |
| Inter-VLAN DNS not resolving | Pi-hole listening mode must be "ALL", not "LOCAL" |

### Services
| Issue | Resolution |
|-------|------------|
| Let's Encrypt failing with Cloudflare | Use DNS-01 challenge, not HTTP-01 |
| NFS permission denied for LXC backup | Use SMB instead (NFS can't handle user namespace UIDs) |
| Minimal Debian missing rsyslog | Install explicitly: `apt install rsyslog` |
| Docker image arch mismatch | Use multi-arch images, verify platform |

### Pterodactyl/Gaming
| Issue | Resolution |
|-------|------------|
| tModLoader wrong runtime | Modern versions need .NET, not Mono |
| Large mod upload failing | Increase nginx `client_max_body_size` to 500M |
| Game server unreachable externally | Cloudflare must be "DNS only" (grey cloud), not proxied |

---

## 🔒 Security Architecture

| Layer | Implementation |
|-------|----------------|
| Perimeter | ISP Router → pfSense firewall |
| Segmentation | 5 VLANs (VLAN 50 air-gapped) |
| DNS | Pi-hole ad/tracker blocking |
| VPN | Tailscale (subnet router on pfSense) |
| External Auth | Cloudflare Access (email OTP) for Grafana |
| Encryption | TLS everywhere via NPM + Let's Encrypt |
| Monitoring | Prometheus + Alertmanager |

### Access Control
| Resource | Internal | Tailscale | Cloudflare Access | Public |
|----------|----------|-----------|-------------------|--------|
| Proxmox | ❌ | ✅ | ❌ | ❌ |
| pfSense | ❌ | ✅ | ❌ | ❌ |
| Pi-hole | ❌ | ✅ | ❌ | ❌ |
| Grafana | ✅ | ✅ | ✅ (OTP) | ❌ |
| Pterodactyl | ✅ | ✅ | ❌ | ✅ |
| Game Servers | ✅ | ✅ | ❌ | ✅ |

---

## 📂 Documentation Structure

### GitHub Repository
```
homelab-infrastructure/
├── README.md                    # Project overview
├── docs/
│   ├── AI-CONTEXT.md           # THIS FILE
│   ├── current-state.md        # Live infrastructure (source of truth)
│   ├── architecture.md         # Target design, ADRs
│   ├── roadmap.md              # Phase status, timeline
│   ├── service-catalog.md      # All services with ports/configs
│   ├── changelog.md            # Version history
│   ├── troubleshoot.md         # Common issues
│   ├── phase-1/                # Historical docs
│   ├── phase-2/
│   ├── phase-3/
│   ├── phase-4/
│   ├── phase-5/
│   ├── phase-6/                # TO BE CREATED
│   └── phase-9/                # TO BE CREATED
```

### Key Files
- **current-state.md** — Always-accurate live state (update after changes)
- **AI-CONTEXT.md** — This file (update at end of each session)
- **roadmap.md** — Phase progress tracking

---

## ❓ Open Questions / Decisions Pending

1. **n8n deployment timing** — Deploy during Phase 6F or wait for Phase 7B?
2. **Homepage dashboard** — Deploy as Phase 6E after 6F, or defer further?
3. **Missing NAS drives** — Investigate WD Green SSD and Kingston NVMe detection

---

## 🔄 How to Use This Document

### For Any AI (Claude, ChatGPT, Copilot, etc.)
1. Upload this file at the start of your session
2. Reference `current-state.md` from GitHub for live infrastructure details
3. Check `roadmap.md` for current phase status

### For Muzakkir
1. **After each session:** Update this file with changes made
2. **Push to GitHub:** Keep it as single source of truth
3. **Before work laptop/other AI sessions:** Pull latest and upload

---

## 📝 Session Log (Recent)

### March 6, 2026
- GitHub & LinkedIn documentation audit completed
- Created AI-CONTEXT.md for multi-AI workflow
- Fixed GitHub repo file naming (removed special characters, added .md extensions)
- Added Phase 6A-6D, Phase 6F, Phase 9 documentation
- Added historical correction banners to phase-1 through phase-4 docs
- Total: 17 files changed, 4 banners added
- Repo now fully aligned with current infrastructure state

### March 5, 2026 (Previous Session)
- Phase 6F security fixes completed
- VLAN migration planned but not executed
- Updated GitHub docs (README, current-state, roadmap, etc.)

### March 3, 2026
- Phase 9 (NAS) completed
- UGREEN DXP2800 "Kinmoon" deployed
- SMB backup storage configured

### March 2, 2026
- Comprehensive infrastructure audit (50 issues found)
- Created prioritized fix plan for Phase 6F

---

*Last updated: March 6, 2026 — Update this file at the end of each session before pushing to GitHub*
