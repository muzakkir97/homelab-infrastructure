# 🔍 Homelab Infrastructure Audit Report

> **Audit Date:** March 2, 2026 | **Auditor:** Claude AI + Muzakkir | **Status:** Phase 6F Complete

---

## Audit Completion Status

> **Last Review:** March 9, 2026

| Category | Total | Done | Pending | Completion |
|----------|:-----:|:----:|:-------:|:----------:|
| Priority 1 (Critical Security) | 6 | 6 | 0 | ✅ 100% |
| Priority 2 (VLAN Realignment) | 11 | 9 | 2 | 🟡 82% |
| Priority 3 (Firewall Hardening) | 7 | 2 | 5 | 🔴 29% |
| Priority 4 (Operational) | 10 | 4 | 6 | 🟡 40% |
| **TOTAL** | **34** | **21** | **13** | **62%** |

### Remaining Items

**High Priority:**
- [ ] Firewall hardening — Replace allow-all rules with proper segmentation
- [ ] Switch management IP — Move from 192.168.1.20 to 192.168.10.20
- [ ] Remove legacy LAN — 192.168.1.0/24 still active

**Medium Priority:**
- [ ] Update services (Pi-hole, NPM, Uptime Kuma)
- [ ] Change domain from `.local` to `.home.arpa`
- [ ] Configure SSL for Wings node
- [ ] Create power outage recovery checklist

**Low Priority:**
- [ ] Exclude VLAN 50 from DNS Resolver

---

## Original Audit Summary (March 2, 2026)

| Category | Items Checked | Issues Found | Critical | High | Medium | Low |
|----------|:---:|:---:|:---:|:---:|:---:|:---:|
| Switch VLAN Config | ✅ | 7 | 1 | 3 | 2 | 1 |
| pfSense Interfaces | ✅ | 4 | 0 | 2 | 1 | 1 |
| pfSense DHCP | ✅ | 3 | 0 | 1 | 1 | 1 |
| pfSense DNS | ✅ | 3 | 0 | 1 | 1 | 1 |
| pfSense Firewall Rules | ✅ | 8 | 1 | 4 | 2 | 1 |
| Proxmox Config | ✅ | 3 | 0 | 1 | 1 | 1 |
| Pi-hole | ✅ | 1 | 0 | 0 | 0 | 1 |
| Nginx Proxy Manager | ✅ | 6 | 3 | 1 | 1 | 1 |
| Cloudflare DNS | ✅ | 4 | 1 | 1 | 2 | 0 |
| Uptime Kuma | ✅ | 6 | 0 | 2 | 3 | 1 |
| Gaming (Pterodactyl) | ✅ | 5 | 0 | 2 | 2 | 1 |
| **TOTAL** | **10/10** | **50** | **6** | **18** | **16** | **10** |

---

## 1. Switch VLAN Configuration (TP-Link TL-SG108E)

### State at Audit (March 2)

| Port | Device | Current PVID |
|------|--------|:---:|
| 1 | AC8F Mini PC (pfSense) | 1 |
| 2 | Proxmox Server (Kuromoon) | 1 |
| 3 | Acasis Dock (Work Laptop) | 1 |
| 4 | Gaming PC (Minimoon) | 1 |
| 5 | Raspberry Pi 4 (Pi-hole) | 20 |
| 6–8 | Empty | 1 |

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| S1 | VLAN names don't match plan | **High** | ✅ Fixed | Names corrected in pfSense (switch has 8-char limit) |
| S2 | VLAN 50 labeled "Gaming" | **High** | ✅ Fixed | Renamed to Malware Lab, gaming moved to VLAN 30 |
| S3 | Ports 3-4 on Default VLAN 1 only | **High** | ✅ Fixed | Port 3 & 4 now on VLAN 20 |
| S4 | VLANs only on ports 1-2 | **Medium** | ✅ Fixed | Ports configured per device |
| S5 | Pi-hole PVID misaligned | **Medium** | ✅ Fixed | Pi-hole on VLAN 30 (port 5, PVID 30) |
| S6 | pfSense PVID is 1 | **Low** | ✅ OK | Trunk port, PVID doesn't matter |
| S7 | Default VLAN 1 has all ports | **Critical** | ⚠️ Partial | Reduced but not eliminated |

### Current State (March 9)

| Port | Device | VLAN Mode | PVID |
|------|--------|-----------|------|
| 1 | pfSense | Trunk | 1 |
| 2 | Proxmox | Trunk | 1 |
| 3 | Work Laptop | Access | 20 |
| 4 | Gaming PC | Access | 20 |
| 5 | Pi-hole | Access | 30 |
| 6 | NAS | Access | 10 |

---

## 2. pfSense Interface Assignments

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| P1 | VLAN names mismatched | **High** | ✅ Fixed | Renamed to VLAN10_MGMT, VLAN20_MAIN, VLAN30_SERVICES, etc. |
| P2 | Untagged LAN exists | **High** | ⚠️ Pending | Still active at 192.168.1.0/24 |
| P3 | VLAN 50 is "Gaming" | **Medium** | ✅ Fixed | Now VLAN50_MALWARE |
| P4 | 7 networks total | **Low** | ⚠️ Pending | Legacy LAN still exists |

### Current State (March 9)

| Interface | VLAN | IP Address | Status |
|-----------|:----:|------------|:------:|
| WAN | — | DHCP | ✅ |
| LAN (legacy) | — | 192.168.1.1/24 | ⚠️ To remove |
| VLAN10_MGMT | 10 | 192.168.10.1/24 | ✅ |
| VLAN20_MAIN | 20 | 192.168.20.1/24 | ✅ |
| VLAN30_SERVICES | 30 | 192.168.30.1/24 | ✅ |
| VLAN40_DMZ | 40 | 192.168.40.1/24 | ✅ |
| VLAN50_MALWARE | 50 | 192.168.50.1/24 | ✅ |

---

## 3. pfSense DHCP Settings

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| D1 | VLAN 50 has DHCP enabled | **High** | ✅ Fixed | DHCP disabled on Malware Lab |
| D2 | LAN DHCP serves unplanned network | **Medium** | ⚠️ Pending | Will be removed with legacy LAN |
| D3 | Gaming VLAN DHCP | **Low** | ✅ Fixed | Gaming VLAN eliminated |

---

## 4. pfSense DNS Configuration

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| N1 | Pi-hole on wrong VLAN | **High** | ✅ Fixed | Pi-hole now at 192.168.30.10 |
| N2 | Domain uses `.local` | **Medium** | ⚠️ Pending | Still using homelab.local |
| N3 | DNS on ALL interfaces | **Low** | ⚠️ Pending | VLAN 50 should be excluded |

---

## 5. pfSense Firewall Rules

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| F1 | LAN "allow any" | **Critical** | ⚠️ Pending | Still allow-all |
| F2 | VLAN10 no inter-VLAN block | **High** | ⚠️ Pending | Still allow-all |
| F3 | VLAN20 no inter-VLAN block | **High** | ⚠️ Pending | Still allow-all |
| F4 | VLAN30 allows outbound to any | **High** | ⚠️ Pending | Still allow-all |
| F5 | Proxmox on untagged LAN | **High** | ✅ Fixed | Now at 192.168.10.5 |
| F6 | Gaming on VLAN 50 | **Medium** | ✅ Fixed | Gaming on VLAN 30 |
| F7 | Temp Minecraft rule | **Low** | ⚠️ Unknown | Need to verify |
| F8 | Rule IPs outdated | **Medium** | ✅ Fixed | NAT rules updated |

---

## 6. Proxmox Configuration

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| X1 | Proxmox on untagged LAN | **High** | ✅ Fixed | Now at 192.168.10.5 via vmbr0.10 |
| X2 | No container autostart | **Medium** | ✅ Fixed | All 10 containers with autostart + boot order |
| X3 | Unknown nic1 interface | **Low** | ✅ OK | Inactive, not needed |

### Current Container State (March 9)

| CTID | Name | IP | Boot Order | Status |
|:---:|------|---------|:---:|:------:|
| 201 | nginx-proxy-manager | 192.168.30.201 | 1 | ✅ Running |
| 202 | monitoring-prometheus | 192.168.30.202 | 3 | ✅ Running |
| 203 | monitoring-grafana | 192.168.30.203 | 6 | ✅ Running |
| 204 | monitoring-loki | 192.168.30.204 | 4 | ✅ Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | 5 | ✅ Running |
| 206 | monitoring-uptime | 192.168.30.206 | 7 | ✅ Running |
| 207 | network-ddns | 192.168.30.207 | 2 | ✅ Running |
| 220 | nextcloud | 192.168.30.220 | 8 | ✅ Running |
| 300 | gaming-panel | 192.168.30.210 | 9 | ✅ Running |
| 302 | gaming-wings-1 | 192.168.30.212 | 10 | ✅ Running |

---

## 7. Pi-hole Configuration

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| H1 | Updates available | **Low** | ⚠️ Pending | Core, FTL, Web updates available |

---

## 8. Nginx Proxy Manager

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| R1 | Proxmox exposed publicly | **Critical** | ✅ Fixed | DNS record & proxy host removed |
| R2 | Pi-hole exposed publicly | **Critical** | ✅ Fixed | DNS record & proxy host removed |
| R3 | NPM exposed publicly | **Critical** | ✅ Fixed | DNS record & proxy host removed |
| R4 | Grafana needs CF Access | **High** | ✅ Fixed | Cloudflare Access with email OTP |
| R5 | Backend IPs outdated | **Medium** | ✅ Fixed | All proxy hosts updated |
| R6 | NPM update available | **Low** | ⚠️ Pending | v2.13.6 → v2.14.0 |

---

## 9. Cloudflare DNS Records

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| C1 | Admin subdomains exist | **Critical** | ✅ Fixed | proxmox, pihole, npm records deleted |
| C2 | DNS-only exposes real IP | **High** | ✅ OK | Necessary for game servers |
| C3 | Wildcard catches all | **Medium** | ✅ Fixed | Wildcard record deleted |
| C4 | No status subdomain | **Medium** | ⚠️ Pending | Not configured |

### Current Records (March 9)

| Type | Name | Proxy Status | Purpose |
|------|------|:---:|---------|
| A | grafana | Proxied ☁️ | Grafana (CF Access) |
| A | cloud | Proxied ☁️ | Nextcloud (CF Tunnel) |
| A | panel | DNS Only | Pterodactyl Panel |
| A | mc | DNS Only | Minecraft |
| A | terraria | DNS Only | Terraria |

---

## 10. Uptime Kuma

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| U1 | Alertmanager down | **High** | ✅ Fixed | Container autostart configured |
| U2 | Prometheus down | **High** | ✅ Fixed | Container autostart configured |
| U3 | Only 5 monitors | **Medium** | ✅ Fixed | Now 6 monitors |
| U4 | Proxmox SSL errors | **Medium** | ✅ Fixed | "Ignore TLS" enabled |
| U5 | No external status page | **Medium** | ⚠️ Pending | Not configured |
| U6 | Update available | **Low** | ⚠️ Pending | Update available |

### Current Monitors (March 9)

| Monitor | Target | Status |
|---------|--------|:------:|
| Alertmanager | http://192.168.30.205:9093 | ✅ UP |
| Grafana | http://192.168.30.203:3000 | ✅ UP |
| NextCloud | http://192.168.30.220/status.php | ✅ UP |
| Pi-hole | http://192.168.30.10 | ✅ UP |
| Prometheus | http://192.168.30.202:9090 | ✅ UP |
| Proxmox | https://192.168.10.5:8006 | ✅ UP |

---

## 11. Gaming Services (Pterodactyl)

### Issues & Resolution

| # | Issue | Severity | Status | Resolution |
|---|-------|----------|:------:|------------|
| G1 | Wings heartbeat failed | **High** | ✅ Fixed | Container autostart, DNS fixed |
| G2 | Game servers offline | **High** | ✅ Fixed | Dependent on Wings |
| G3 | No SSL on Wings | **Medium** | ⚠️ Pending | Still no SSL |
| G4 | Gaming on VLAN 50 | **Medium** | ✅ Fixed | Moved to VLAN 30 |
| G5 | Wings 10GB RAM | **Low** | ⚠️ Monitor | Large allocation |

### Current State (March 9)

| Service | IP | VLAN | Status |
|---------|-----|:----:|:------:|
| Pterodactyl Panel | 192.168.30.210 | 30 | ✅ Running |
| Pterodactyl Wings | 192.168.30.212 | 30 | ✅ Running |

---

## Additional Tasks

| # | Task | Category | Status |
|---|------|----------|:------:|
| A1 | Configure container autostart | Operations | ✅ Done |
| A2 | Update Pi-hole | Maintenance | ⚠️ Pending |
| A3 | Update NPM | Maintenance | ⚠️ Pending |
| A4 | Update Uptime Kuma | Maintenance | ⚠️ Pending |
| A5 | Verify Tailscale VPN | Verification | ✅ Done |
| A6 | Change domain to .home.arpa | Configuration | ⚠️ Pending |
| A7 | Add missing Uptime Kuma monitors | Monitoring | ✅ Done |
| A8 | Create recovery checklist | Documentation | ⚠️ Pending |

---

## Next Audit Checklist

For future audits, check:

### Network
- [ ] VLAN assignments match design
- [ ] Switch port configurations correct
- [ ] No devices on legacy/default VLANs
- [ ] Firewall rules follow least-privilege

### Services
- [ ] All containers running with autostart
- [ ] All services accessible internally
- [ ] External access working (Grafana, Nextcloud, Pterodactyl)
- [ ] Monitoring alerts functional (Telegram, Discord)

### Security
- [ ] No admin interfaces exposed publicly
- [ ] Cloudflare Access protecting external services
- [ ] Tailscale VPN working for remote admin
- [ ] DNS records minimal (no wildcards, no admin subdomains)

### Maintenance
- [ ] All services on latest versions
- [ ] Backups running and tested
- [ ] Uptime Kuma monitors for all critical services

---

*Audit created: March 2, 2026 | Last updated: March 9, 2026*
