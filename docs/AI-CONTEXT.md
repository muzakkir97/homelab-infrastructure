# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** March 9, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Phase 6F complete (including firewall hardening). All VLANs properly segmented with enforced firewall rules. 10 containers running with autostart. Admin access via Tailscale only.

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
| NAS | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | 192.168.10.15 | Backups (SMB) |
| DNS Server | — | Raspberry Pi 4 | 192.168.30.10 | Pi-hole |
| Gaming PC | Minimoon | Ryzen 7 7800X3D | 192.168.20.101 | Personal + Tailscale (100.106.109.4) |

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

| VLAN ID | Name | Subnet | Gateway | Purpose |
|---------|------|--------|---------|---------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | 192.168.20.1 | Client devices |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped) |

### Inter-VLAN Traffic Rules (ENFORCED)

| From → To | Status | Reason |
|-----------|--------|--------|
| VLAN 20 → VLAN 10 | ❌ Blocked | Clients can't reach management |
| VLAN 20 → VLAN 30 | ✅ Limited | Specific service ports only |
| VLAN 30 → VLAN 10 | ✅ Limited | Prometheus metrics only |
| VLAN 40 → Internal | ❌ Blocked | DMZ isolated from all internal |
| VLAN 50 → Anywhere | ❌ Blocked | Air-gapped malware lab |
| Tailscale → All | ✅ Allowed | Admin VPN bypass |

---

## 📦 Container Inventory (Proxmox LXC)

| CTID | Name | IP | Autostart | Boot Order | Status |
|------|------|----|-----------|------------|--------|
| 201 | nginx-proxy-manager | 192.168.30.201 | ✅ | 1 | ✅ Running |
| 202 | monitoring-prometheus | 192.168.30.202 | ✅ | 3 | ✅ Running |
| 203 | monitoring-grafana | 192.168.30.203 | ✅ | 6 | ✅ Running |
| 204 | monitoring-loki | 192.168.30.204 | ✅ | 4 | ✅ Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | ✅ | 5 | ✅ Running |
| 206 | monitoring-uptime | 192.168.30.206 | ✅ | 7 | ✅ Running |
| 207 | network-ddns | 192.168.30.207 | ✅ | 2 | ✅ Running |
| 220 | nextcloud | 192.168.30.220 | ✅ | 8 | ✅ Running |
| 300 | gaming-panel | 192.168.30.210 | ✅ | 9 | ✅ Running |
| 302 | gaming-wings-1 | 192.168.30.212 | ✅ | 10 | ✅ Running |

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
| 6F | Infrastructure Audit, VLAN Migration & Firewall Hardening | ✅ Complete | Mar 9, 2026 |
| 7 | Nextcloud Deployment | ✅ Complete | Mar 8, 2026 |
| 7A | Backup Strategy | 📋 Planned (Priority) | — |
| 6E | Homepage Dashboard | 📋 Planned | — |
| 7B | n8n Workflow Automation | 📋 Planned | — |

---

## 🐛 Key Lessons Learned

### Network & VLANs
| Issue | Resolution |
|-------|------------|
| Proxmox LXC VLAN not working | Add `bridge-vids 2-4094` to /etc/network/interfaces |
| Intel i226-V NICs failing | FreeBSD driver issue; only igc2/igc3 work on AC8F |
| Double NAT breaking port forward | Port forward on BOTH ISP router AND pfSense |
| Inter-VLAN DNS not resolving | Use pfSense DNS Resolver forwarding to Pi-hole |

### Firewall Hardening
| Issue | Resolution |
|-------|------------|
| Internet loss after rule changes | Source must be "subnets" not "address" |
| Tailscale ping failing | Normal — use `tailscale ping` instead of ICMP |
| Blocked traffic still works | Disconnect Tailscale to test local firewall |
| Old IPs in aliases | Audit aliases after any IP migration |

---

## 🔒 Security Architecture

| Layer | Implementation |
|-------|----------------|
| Perimeter | ISP Router → pfSense firewall |
| Segmentation | 5 VLANs with enforced firewall rules |
| DNS | Pi-hole ad/tracker blocking |
| VPN | Tailscale (subnet router on pfSense + Gaming PC) |
| External Auth | Cloudflare Access (email OTP) for Grafana |
| External Access | Cloudflare Tunnel for Nextcloud |
| Admin Access | Tailscale only (VLAN 20 blocked from VLAN 10) |

---

## ❓ Pending Tasks

| Task | Priority |
|------|----------|
| Backup strategy (Phase 7A) | High |
| Switch management IP (192.168.1.20 → 192.168.10.20) | Medium |
| Remove legacy LAN interface | Medium |
| Nextcloud 2FA (TOTP) | Low |
| Nextcloud data → NAS storage | Low |

---

## 📝 Session Log (Recent)

### March 9, 2026
- Completed firewall hardening (Phase 6F final part)
- Audited all pfSense firewall rules
- Created/updated firewall aliases (RFC1918, DNS_SERVERS, PROXMOX, etc.)
- Implemented proper inter-VLAN rules for all 5 VLANs
- Installed Tailscale on Gaming PC for admin access
- Fixed "subnets vs address" issue causing internet loss
- Verified segmentation working (VLAN10 blocked locally, works via Tailscale)

### March 8, 2026
- Deployed Nextcloud (CT 220) with LAMP stack
- Configured Cloudflare Tunnel for external access
- Fixed Uptime Kuma monitors
- Configured container autostart with boot order

### March 7, 2026
- Phase 6F VLAN migration completed
- All 9 containers migrated to VLAN 30

---

*Last updated: March 9, 2026 — Update this file at the end of each session before pushing to GitHub*
