# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** March 9, 2026
> **Purpose:** Upload this file to any AI to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

Enterprise-grade homelab for career transition from Customer Service Engineer (F-Secure, cybersecurity) to Cloud Engineering / DevOps. All documentation on GitHub and LinkedIn.

**Current Status:** Phase 7 complete. 10 containers running with autostart. Pterodactyl access issue needs investigation.

---

## 👤 About Me

| Field | Details |
|-------|---------|
| **Name** | Muzakkir Kholil |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location** | Petaling Jaya, Selangor, Malaysia |
| **Target Role** | Cloud Engineering / DevOps |
| **Domain** | najhin-gaming.com (Cloudflare) |
| **GitHub** | github.com/muzakkir97 |

---

## 🎯 My Preferences

- **Explain like you're teaching** — I want to understand the "why"
- **No multiple-choice widgets during deployment** — Just give instructions
- **Be direct** — Tell me if my idea is bad
- **GitHub docs must be sanitized** — No real IPs in public docs
- **Function-based naming:** `monitoring-prometheus`, `gaming-panel`
- **Moon theme for hardware:** Kuromoon (Proxmox), Kinmoon (NAS), Minimoon (Gaming PC)

---

## 📝 Documentation Maintenance Rules

### Always Update (Every Session)

| File | Purpose | What to Update |
|------|---------|----------------|
| `docs/AI-CONTEXT.md` | AI session continuity | Session log, current status, pending tasks |
| `docs/current-state.md` | Live infrastructure state | IPs, container status, any changes made |
| `docs/changelog.md` | Version history | Add entry at top with date and changes |

### Update When Relevant

| File | When to Update |
|------|----------------|
| `docs/roadmap.md` | Phase completed or plan changes |
| `docs/service-catalog.md` | New service added, IPs changed, ports changed |
| `docs/troubleshoot.md` | New issue encountered and resolved |
| `docs/architecture.md` | New ADR, topology change, major design decision |
| `docs/homelab-audit-report.md` | Audit item completed or new audit performed |
| `README.md` | Phase milestone, new feature worth highlighting |

### Create New (Per Phase)

| File | Location |
|------|----------|
| Phase deployment guide | `docs/phase-X/phase-X-[name].md` |
| LinkedIn post | `docs/phase-X/linkedin-post-phase-X.md` |
| Chat context | `docs/phase-X/chat-context-phase-X.md` |

### End of Session Checklist

```
□ AI-CONTEXT.md — Session log updated
□ current-state.md — Infrastructure accurate  
□ changelog.md — Entry added
□ [If new phase] — Phase folder + docs created
□ [If issue fixed] — troubleshoot.md updated
□ [If audit item done] — audit-report.md updated
```

---

## 🖥️ Hardware

| Device | Hostname | Specs | IP Address | Role |
|--------|----------|-------|------------|------|
| Proxmox Server | Kuromoon | Ryzen 5 5600X, 32GB RAM | 192.168.10.5 | Hypervisor |
| pfSense Firewall | — | AC8F Mini PC, Intel N100 | 192.168.10.1 | Router, Firewall |
| Managed Switch | — | TP-Link TL-SG108E | 192.168.1.20 | Layer 2, VLANs |
| NAS | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | 192.168.10.15 | Backups (SMB) |
| DNS Server | — | Raspberry Pi 4 | 192.168.30.10 | Pi-hole |

---

## 🌐 Network Architecture

### VLAN Structure (Fully Operational)

| VLAN ID | Name | Subnet | Gateway | Purpose |
|---------|------|--------|---------|---------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | 192.168.10.1 | Management (Proxmox, pfSense, NAS) |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | 192.168.20.1 | Client devices |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | 192.168.40.1 | Future public-facing |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | 192.168.50.1 | Isolated lab (air-gapped) |

### DNS Flow
Clients → pfSense (VLAN Gateway) → Pi-hole (192.168.30.10) → Cloudflare (1.1.1.1)

---

## 📦 Container Inventory (All VLAN 30)

| CTID | Name | IP | Boot Order | Status |
|------|------|----|-----------|--------|
| 201 | nginx-proxy-manager | 192.168.30.201 | 1 | ✅ Running |
| 207 | network-ddns | 192.168.30.207 | 2 | ✅ Running |
| 202 | monitoring-prometheus | 192.168.30.202 | 3 | ✅ Running |
| 204 | monitoring-loki | 192.168.30.204 | 4 | ✅ Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | 5 | ✅ Running |
| 203 | monitoring-grafana | 192.168.30.203 | 6 | ✅ Running |
| 206 | monitoring-uptime | 192.168.30.206 | 7 | ✅ Running |
| 220 | nextcloud | 192.168.30.220 | 8 | ✅ Running |
| 300 | gaming-panel | 192.168.30.210 | 9 | ✅ Running |
| 302 | gaming-wings-1 | 192.168.30.212 | 10 | ✅ Running |

---

## 🔧 Services

### Core Infrastructure
- **pfSense** — Firewall, DHCP, NAT, inter-VLAN routing
- **Pi-hole** — DNS filtering (Raspberry Pi 4)
- **Nginx Proxy Manager** — Reverse proxy, Let's Encrypt SSL
- **Tailscale** — VPN (subnet router on pfSense, IP: 100.110.165.45)
- **Cloudflare DDNS** — Dynamic DNS updates

### Monitoring Stack
- **Prometheus** — Metrics collection (port 9090)
- **Grafana** — Dashboards (port 3000, external via CF Access)
- **Loki** — Log aggregation (port 3100)
- **Alertmanager** — Alerts to Telegram (critical) + Discord (warnings)
- **Uptime Kuma** — Availability monitoring (port 3001)

### Productivity
- **Nextcloud** — Cloud storage (port 80, external via Cloudflare Tunnel)
  - URL: https://cloud.najhin-gaming.com
  - Tunnel: homelab-tunnel

### Gaming Platform
- **Pterodactyl Panel** (CT 300) — Game server management UI
  - IP: 192.168.30.210, External: panel.najhin-gaming.com
- **Pterodactyl Wings** (CT 302) — Game server daemon
  - IP: 192.168.30.212
- **Terraria** — tModLoader + Calamity (port 7777)
- **Minecraft** — Paper server (port 25570)

---

## 🔒 External Access

| Service | URL | Method |
|---------|-----|--------|
| Grafana | grafana.najhin-gaming.com | Cloudflare Access (OTP) |
| Nextcloud | cloud.najhin-gaming.com | Cloudflare Tunnel |
| Pterodactyl | panel.najhin-gaming.com | NPM + Port Forward |
| Terraria | terraria.najhin-gaming.com:7777 | Port Forward (DNS only) |
| Minecraft | mc.najhin-gaming.com:25570 | Port Forward (DNS only) |

---

## 📋 Phase Status

| Phase | Title | Status |
|-------|-------|--------|
| 1-5 | Foundation, Network, Services, Monitoring | ✅ Complete |
| 6A-6D | Gaming Platform | ✅ Complete |
| 6F | VLAN Migration & Audit | ✅ Complete |
| 7 | Nextcloud Deployment | ✅ Complete |
| 7A | Backup Strategy | 📋 Next |
| 7B | Workflow Automation (n8n) | 📋 Planned |

---

## 🔧 Audit Status (62% Complete)

**Remaining High-Priority:**
- Firewall hardening (replace allow-all rules)
- Switch management IP (192.168.1.20 → 192.168.10.20)
- Remove legacy LAN (192.168.1.0/24)

**Medium Priority:**
- Update services (Pi-hole, NPM, Uptime Kuma)
- Change domain from `.local` to `.home.arpa`
- SSL for Wings node

See `docs/homelab-audit-report.md` for full tracking.

---

## 🔑 Key Access Info

### SSH Access
```bash
ssh root@192.168.10.5  # Proxmox
ssh pi@192.168.30.10   # Pi-hole
```

### Container Access
```bash
pct enter [CTID]  # Enter container
pct list          # List all containers
pct start [CTID]  # Start container
```

---

## 📝 Session Log

### March 9, 2026
- Reviewed audit completion status (62% done)
- Updated audit report with completion tracking
- Created documentation maintenance rules
- Pterodactyl access issue identified (needs troubleshooting)

### March 8, 2026
- Deployed Nextcloud (CT 220) with Cloudflare Tunnel
- Fixed Uptime Kuma monitors
- Configured container autostart (all 10 containers)
- Fixed Pi-hole node_exporter
- Updated all GitHub documentation

### March 7, 2026
- Phase 6F VLAN migration completed
- All 9 containers migrated to VLAN 30

---

*Last updated: March 9, 2026*
