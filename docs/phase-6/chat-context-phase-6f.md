# Chat Context — Phase 6F Completion

> **Purpose:** Session continuity document for new AI chat sessions  
> **Last Updated:** March 7, 2026  
> **Current Phase:** 6F Complete → Ready for Phase 7A

---

## Project Summary

Homelab infrastructure project building enterprise-grade self-hosted environment. Primary goals: career transition to cloud engineering/DevOps, replace cloud subscriptions with self-hosted alternatives, create isolated security lab.

**GitHub:** github.com/muzakkir97/homelab-infrastructure  
**Domain:** najhin-gaming.com (Cloudflare)

---

## Current Infrastructure State

### Hardware
- **Proxmox Server (Kuromoon):** Ryzen 5 5600X, 32GB RAM — IP: 192.168.10.5
- **pfSense Router:** AC8F Mini PC (Intel N100) — IP: 192.168.10.1
- **Switch:** TP-Link TL-SG108E — IP: 192.168.1.20 (legacy, needs migration)
- **NAS (Kinmoon):** UGREEN DXP2800 — IP: 192.168.10.100 (DHCP, needs static)
- **Pi-hole:** Raspberry Pi 4 — IP: 192.168.30.10

### VLAN Structure (Fully Operational)
| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | Management |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | Clients |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | All services |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | Future public-facing |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | Isolated lab |

### Containers (All on VLAN 30)
| CTID | Name | IP |
|------|------|-----|
| 201 | nginx-proxy-manager | 192.168.30.201 |
| 202 | monitoring-prometheus | 192.168.30.202 |
| 203 | monitoring-grafana | 192.168.30.203 |
| 204 | monitoring-loki | 192.168.30.204 |
| 205 | monitoring-alertmanager | 192.168.30.205 |
| 206 | monitoring-uptime | 192.168.30.206 |
| 207 | network-ddns | 192.168.30.207 |
| 300 | gaming-panel | 192.168.30.210 |
| 302 | gaming-wings-1 | 192.168.30.212 |

### DNS Architecture
Clients → VLAN Gateway (pfSense) → Pi-hole (192.168.30.10) → Cloudflare (1.1.1.1)

### External Access
- **Tailscale:** pfSense as subnet router (100.110.165.45)
- **Cloudflare Access:** Grafana (email OTP)
- **Public DNS:** panel, mc, terraria (DNS only mode)

---

## What Was Completed in Phase 6F

### March 6-7, 2026 (~17 hours)

1. **Switch VLAN Configuration**
   - Port 2 (Proxmox) changed to trunk mode
   - Ports 3-6 assigned to VLANs 20, 20, 30, 10

2. **Container Migration**
   - All 9 containers migrated to VLAN 30
   - All nameservers updated to 192.168.30.1
   - All containers restarted for resolv.conf update

3. **Pi-hole Fresh Install**
   - Raspberry Pi OS uses NetworkManager (not dhcpcd!)
   - Static IP via nmcli commands
   - Fresh Pi-hole v6.x installation

4. **DNS Architecture**
   - pfSense DNS Resolver with Forwarding Mode
   - Upstream: Pi-hole, fallback: Cloudflare
   - DHCP assigns VLAN gateway as DNS

5. **Service Updates**
   - Prometheus scrape targets
   - Grafana data sources
   - NPM proxy hosts
   - Uptime Kuma monitors
   - Pterodactyl Panel + Wings configs

6. **Tailscale Fixed**
   - Added firewall rules on Tailscale interface
   - Remote access now working

---

## Known Issues

1. **Cross-VLAN DNS/HTTPS:** Direct queries time out (ping works). Root cause unknown. Workaround: pfSense forwarding.

2. **NAS on DHCP:** Currently 192.168.10.100, needs static 192.168.10.15.

3. **Proxmox SMB Mount:** Still points to 192.168.1.15 (old NAS IP).

4. **Uptime Kuma → Proxmox:** Monitor fails (cross-VLAN HTTPS issue).

---

## Pending Tasks (Priority Order)

### High Priority
1. **NAS Static IP** — Set to 192.168.10.15 via UGOS app
2. **Proxmox SMB Mount** — Update IP in /etc/pve/storage.cfg
3. **Test Backup** — `vzdump 201 --storage kinmoon-smb --mode snapshot`

### Medium Priority
4. **Container Autostart** — Enable with boot order
5. **Firewall Hardening** — Replace allow-all rules with specific rules

### Low Priority
6. **Switch Management IP** — Change to 192.168.10.20
7. **Remove Legacy LAN** — Delete 192.168.1.0/24 interface
8. **Investigate Cross-VLAN Issue** — Why DNS/HTTPS fails despite allow-all

---

## Phase Roadmap

| Phase | Title | Status |
|-------|-------|--------|
| 6F | Infrastructure Audit & VLAN Migration | ✅ Complete |
| 7A | Backup Strategy | 🔜 Next |
| 7B | Workflow Automation (n8n) | Planned |
| 7C | AI Agent (OpenClaw/ClawdBot) | Planned |
| 8 | Gaming Expansion (Discord bot, more servers) | Planned |
| 9 | NAS Deployment | ✅ Complete (hardware) |

---

## Key Technical Learnings

1. **Trunk vs Access Ports:** Devices with VLAN-aware bridges need trunk ports.

2. **NetworkManager vs dhcpcd:** Check `systemctl status` before editing configs.

3. **Container DNS Updates:** Must restart container after `pct set -nameserver`.

4. **Tailscale Firewall:** No rules = all traffic blocked.

5. **DNS Architecture:** pfSense as DNS proxy more reliable than cross-VLAN queries.

---

## User Preferences

- Prefers complex approaches for learning value
- Wants explanations of "why" not just "how"
- Maintains dual documentation (sanitized public + detailed private)
- Creates LinkedIn posts for each completed phase
- Prefers direct instructions over multiple-choice during deployment

---

## Files to Reference

- `/docs/current-state.md` — Live infrastructure state
- `/docs/phase-6/phase-6f-vlan-migration.md` — Phase 6F completion doc
- `/docs/changelog.md` — All changes
- `/docs/troubleshoot.md` — Issue resolutions

---

*Use this document to quickly onboard a new AI chat session.*
