# Phase 6F: Infrastructure Audit & Correction

> **Status:** 🔧 In Progress (75%) | **Started:** March 2, 2026

---

## Overview

Phase 6F was introduced after a comprehensive infrastructure audit revealed significant misalignments between the original VLAN design and actual implementation, plus critical security issues with exposed admin interfaces.

### Objectives

| Objective | Status |
|-----------|--------|
| Security audit of all components | ✅ Complete |
| Remove exposed admin interfaces | ✅ Complete |
| Correct VLAN naming and structure | ⏳ Pending |
| Migrate containers to correct VLANs | ⏳ Pending |
| Harden firewall rules | ⏳ Pending |

---

## Audit Summary

**Audit Date:** March 2, 2026

| Category | Issues Found | Critical | High | Medium | Low |
|----------|:---:|:---:|:---:|:---:|:---:|
| Switch VLAN Config | 7 | 1 | 3 | 2 | 1 |
| pfSense Interfaces | 4 | 0 | 2 | 1 | 1 |
| pfSense Firewall Rules | 8 | 1 | 4 | 2 | 1 |
| Proxmox Config | 3 | 0 | 1 | 1 | 1 |
| Nginx Proxy Manager | 6 | 3 | 1 | 1 | 1 |
| Cloudflare DNS | 4 | 1 | 1 | 2 | 0 |
| **TOTAL** | **50** | **6** | **18** | **16** | **10** |

---

## Completed: Security Fixes ✅

### 1. Cloudflare DNS Cleanup

**Removed DNS records:**
- `proxmox.najhin-gaming.com` — Hypervisor admin exposed
- `pihole.najhin-gaming.com` — DNS admin exposed
- `npm.najhin-gaming.com` — Reverse proxy admin exposed
- `*.najhin-gaming.com` (wildcard) — Caught unintended subdomains

**Remaining records:**
- `grafana.najhin-gaming.com` — Protected by Cloudflare Access (email OTP)
- `panel.najhin-gaming.com` — Pterodactyl (DNS only)
- `mc.najhin-gaming.com` — Minecraft (DNS only)
- `terraria.najhin-gaming.com` — Terraria (DNS only)

### 2. NPM Proxy Host Cleanup

**Verified state:** Only Grafana proxy host exists (as intended)

### 3. Cloudflare Access Cleanup

**Deleted:** Orphaned "Proxmox" Access application (no longer needed after DNS record removal)

---

## Pending: VLAN Migration ⏳

### Current vs Target VLAN Structure

| VLAN ID | Current Name | Target Name | Current Purpose | Target Purpose |
|---------|--------------|-------------|-----------------|----------------|
| 10 | VLAN10_MAIN | VLAN10_MGMT | Misconfigured | Management |
| 20 | VLAN20_SERVICES | VLAN20_MAIN | Services (wrong) | Client devices |
| 30 | VLAN30_DMZ | VLAN30_SERVICES | DMZ (wrong) | All services |
| 40 | VLAN40_MALWARE | VLAN40_DMZ | Malware Lab (wrong) | DMZ |
| 50 | Gaming | VLAN50_MALWARE | Gaming | Malware Lab |
| — | LAN (legacy) | — | Proxmox, NAS | To be eliminated |

### Migration Plan (Requires ~2hr Downtime)

**Phase 2-A: pfSense VLAN Reconfiguration**
1. Rename all VLAN interfaces to correct names
2. Update DHCP scopes
3. Create temporary allow-all rules

**Phase 2-B: Switch Reconfiguration**
1. Update VLAN names on TP-Link switch
2. Reconfigure port memberships
3. Update PVIDs

**Phase 2-C: Proxmox Network Changes**
1. Create vmbr0.10 subinterface for management
2. Move Proxmox to 192.168.10.5

**Phase 2-D: Container Migration**
1. Stop all containers
2. Update network configuration (VLAN tag → 30)
3. Assign new static IPs
4. Start containers in boot order

**Phase 2-E: Service Updates**
1. Update Pi-hole IP and pfSense DNS
2. Update NPM backend IPs
3. Update Prometheus targets
4. Update Uptime Kuma monitors

**Phase 2-F: Verification**
1. Test all services
2. Test external access
3. Test inter-VLAN routing
4. Test alerts

### Target Container IPs

| CTID | Name | Current IP | Target IP |
|------|------|------------|-----------|
| 201 | nginx-proxy-manager | 192.168.30.10 | 192.168.30.201 |
| 202 | monitoring-prometheus | 192.168.20.11 | 192.168.30.202 |
| 203 | monitoring-grafana | 192.168.20.12 | 192.168.30.203 |
| 204 | monitoring-loki | 192.168.20.x | 192.168.30.204 |
| 205 | monitoring-alertmanager | 192.168.20.14 | 192.168.30.205 |
| 206 | monitoring-uptime | 192.168.20.x | 192.168.30.206 |
| 207 | network-ddns | 192.168.20.x | 192.168.30.207 |
| 300 | gaming-panel | 192.168.50.10 | 192.168.30.210 |
| 302 | gaming-wings-1 | 192.168.50.12 | 192.168.30.212 |

---

## Pending: Firewall Hardening ⏳

### Current State (Problems)
- LAN has "allow any" rule — no segmentation
- Most VLANs have no inter-VLAN block rules
- Temporary rules left from testing

### Target State
- Default deny between VLANs
- Explicit allow rules for required traffic only
- VLAN 50 (Malware Lab) completely air-gapped
- Proper logging enabled

---

## Pending: Operational Improvements ⏳

| Task | Priority |
|------|----------|
| Configure container autostart with boot order | High |
| Update all Uptime Kuma monitors | Medium |
| Change domain from `.local` to `.home.arpa` | Low |
| Create power outage recovery checklist | Low |

---

## Backups Created

Before starting Phase 6F, the following backups were created:

```
/root/vlan-migration-backup/
├── pfSense-config-backup.xml
├── ct-201-nginx-proxy-manager.conf
├── ct-202-monitoring-prometheus.conf
├── ct-203-monitoring-grafana.conf
├── ct-204-monitoring-loki.conf
├── ct-205-monitoring-alertmanager.conf
├── ct-206-monitoring-uptime.conf
├── ct-207-network-ddns.conf
├── ct-300-gaming-panel.conf
└── ct-302-gaming-wings-1.conf
```

---

## Lessons Learned

1. **VLAN naming drift** from original design creates compounding confusion
2. **Public exposure of admin interfaces** is a critical security anti-pattern
3. **Allow-all firewall rules** defeat the purpose of VLAN segmentation
4. **Regular audits** catch issues before they become problems
5. **Document the target state** clearly so drift is immediately visible

---

## Next Steps

1. Schedule 2-hour maintenance window for VLAN migration
2. Execute migration phases 2-A through 2-F
3. Verify all services after migration
4. Implement proper firewall rules
5. Update documentation to reflect final state

---

*Documentation created: March 2026*
