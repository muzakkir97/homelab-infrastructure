# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** March 9, 2026
> **Last Updated:** March 13, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure
@@ -11,7 +11,7 @@

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Phase 6F complete (including firewall hardening). All VLANs properly segmented with enforced firewall rules. 10 containers running with autostart. Admin access via Tailscale only.
**Current Status:** Phase 7A complete (Backup Strategy). Hybrid backup system operational with NFS for small containers and local storage for large containers. All 10 containers backed up automatically.

---

@@ -58,7 +58,7 @@ I'm building an **enterprise-grade homelab** for career transition from Customer
| Proxmox Server | Kuromoon | Ryzen 5 5600X, 32GB RAM | 192.168.10.5 | Hypervisor |
| pfSense Firewall | — | AC8F Mini PC, Intel N100 | 192.168.10.1 | Router, Firewall |
| Managed Switch | — | TP-Link TL-SG108E | 192.168.1.20 | Layer 2, VLANs |
| NAS | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | 192.168.10.15 | Backups (SMB) |
| NAS | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | 192.168.10.15 | Backups (NFS) |
| DNS Server | — | Raspberry Pi 4 | 192.168.30.10 | Pi-hole |
| Gaming PC | Minimoon | Ryzen 7 7800X3D | 192.168.20.101 | Personal + Tailscale (100.106.109.4) |

@@ -89,33 +89,46 @@ Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
| 40 | VLAN40_DMZ | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped) |

### Inter-VLAN Traffic Rules (ENFORCED)
---

## 📦 Container Inventory (Proxmox LXC)

| From → To | Status | Reason |
|-----------|--------|--------|
| VLAN 20 → VLAN 10 | ❌ Blocked | Clients can't reach management |
| VLAN 20 → VLAN 30 | ✅ Limited | Specific service ports only |
| VLAN 30 → VLAN 10 | ✅ Limited | Prometheus metrics only |
| VLAN 40 → Internal | ❌ Blocked | DMZ isolated from all internal |
| VLAN 50 → Anywhere | ❌ Blocked | Air-gapped malware lab |
| Tailscale → All | ✅ Allowed | Admin VPN bypass |
| CTID | Name | IP | Autostart | Boot Order | Backup Target | Status |
|------|------|----|-----------|------------|---------------|--------|
| 201 | nginx-proxy-manager | 192.168.30.201 | ✅ | 1 | kinmoon-nfs | ✅ Running |
| 202 | monitoring-prometheus | 192.168.30.202 | ✅ | 3 | kinmoon-nfs | ✅ Running |
| 203 | monitoring-grafana | 192.168.30.203 | ✅ | 6 | kinmoon-nfs | ✅ Running |
| 204 | monitoring-loki | 192.168.30.204 | ✅ | 4 | kinmoon-nfs | ✅ Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | ✅ | 5 | kinmoon-nfs | ✅ Running |
| 206 | monitoring-uptime | 192.168.30.206 | ✅ | 7 | kinmoon-nfs | ✅ Running |
| 207 | network-ddns | 192.168.30.207 | ✅ | 2 | kinmoon-nfs | ✅ Running |
| 220 | nextcloud | 192.168.30.220 | ✅ | 8 | data-storage | ✅ Running |
| 300 | gaming-panel | 192.168.30.210 | ✅ | 9 | kinmoon-nfs | ✅ Running |
| 302 | gaming-wings-1 | 192.168.30.212 | ✅ | 10 | data-storage | ✅ Running |

---

## 📦 Container Inventory (Proxmox LXC)
## 💾 Backup Strategy (Phase 7A)

### Backup Jobs

| Job | Schedule | Storage | Containers | Retention |
|-----|----------|---------|------------|-----------|
| Small Containers | 02:00 | kinmoon-nfs (NAS) | 201, 202, 203, 204, 205, 206, 207, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30 | data-storage (Local HDD) | 220, 302 | 7 daily, 4 weekly, 2 monthly |

### Storage

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
| Name | Type | Protocol | Capacity | Purpose |
|------|------|----------|----------|---------|
| kinmoon-nfs | NAS | NFS v3 | 3.6 TB | Small container backups |
| data-storage | Local HDD | Direct | 7.2 TB | Large container backups |
| local-lvm | Local NVMe | LVM-Thin | 137 GB | Container root disks |
| vmpool-fast | Local NVMe | ZFS | 899 GB | Fast container storage |

### Why Hybrid?
- **UGREEN NAS limitation:** SMB/CIFS caused kernel crashes; NFS works but fails on sustained large writes (>10GB)
- **Solution:** Small containers → NFS, Large containers (Nextcloud 17GB, Wings 12GB) → Local storage

---

@@ -132,7 +145,7 @@ Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
| 9 | NAS Deployment (Kinmoon) | ✅ Complete | Mar 3, 2026 |
| 6F | Infrastructure Audit, VLAN Migration & Firewall Hardening | ✅ Complete | Mar 9, 2026 |
| 7 | Nextcloud Deployment | ✅ Complete | Mar 8, 2026 |
| 7A | Backup Strategy | 📋 Planned (Priority) | — |
| **7A** | **Backup Strategy** | ✅ **Complete** | **Mar 13, 2026** |
| 6E | Homepage Dashboard | 📋 Planned | — |
| 7B | n8n Workflow Automation | 📋 Planned | — |

@@ -154,7 +167,15 @@ Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
| Internet loss after rule changes | Source must be "subnets" not "address" |
| Tailscale ping failing | Normal — use `tailscale ping` instead of ICMP |
| Blocked traffic still works | Disconnect Tailscale to test local firewall |
| Old IPs in aliases | Audit aliases after any IP migration |

### Backup Strategy (Phase 7A)
| Issue | Resolution |
|-------|------------|
| SMB/CIFS kernel crashes | Switch to NFS protocol |
| NFS permission denied | Change squash to "Map all users to admin" |
| Large file I/O errors on NAS | Hybrid approach — large containers to local storage |
| ZFS snapshot busy | Kill stuck processes or reboot, then destroy snapshot |
| Container locked (backup) | `pct unlock <CTID>` |

---

@@ -169,42 +190,57 @@ Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
| External Auth | Cloudflare Access (email OTP) for Grafana |
| External Access | Cloudflare Tunnel for Nextcloud |
| Admin Access | Tailscale only (VLAN 20 blocked from VLAN 10) |
| Backup | Automated daily backups with 7/4/2 retention |

---

## ❓ Pending Tasks

| Task | Priority |
|------|----------|
| Backup strategy (Phase 7A) | High |
| WiFi Access Point for VLAN 20 (phone Pi-hole) | Medium |
| Switch management IP (192.168.1.20 → 192.168.10.20) | Medium |
| Remove legacy LAN interface | Medium |
| Homepage Dashboard (Phase 6E) | Medium |
| Off-site backup (Backblaze B2) | Low |
| Nextcloud 2FA (TOTP) | Low |
| Nextcloud data → NAS storage | Low |

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
- Audited all pfSense firewall rules
- Created/updated firewall aliases (RFC1918, DNS_SERVERS, PROXMOX, etc.)
- Implemented proper inter-VLAN rules for all 5 VLANs
- Created/updated firewall aliases
- Implemented proper inter-VLAN rules
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
*Last updated: March 16, 2026 — Update this file at the end of each session before pushing to GitHub*
