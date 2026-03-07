# Phase 6F: Infrastructure Audit & VLAN Migration

> **Status:** ✅ Complete (Core Migration)  
> **Date:** March 6-7, 2026  
> **Duration:** ~17 hours (9 PM Mar 6 → 2:30 PM Mar 7)  
> **Downtime:** ~4 hours (during active migration)

---

## Executive Summary

Phase 6F transformed the homelab from a partially-segmented network with legacy flat LAN to a properly architected VLAN infrastructure. All services were migrated to their correct VLANs, DNS architecture was rebuilt using pfSense forwarding to Pi-hole, and remote access via Tailscale was secured.

### Key Achievements

- ✅ Migrated all 9 LXC containers to VLAN 30 (Services)
- ✅ Fresh Pi-hole installation on VLAN 30 (192.168.30.10)
- ✅ Configured pfSense DNS Resolver with Pi-hole forwarding
- ✅ Moved NAS to VLAN 10 (Management)
- ✅ Fixed Tailscale remote access with proper firewall rules
- ✅ Updated all service configurations (Prometheus, Grafana, NPM, Uptime Kuma, Pterodactyl)

---

## 1. Pre-Migration State

### Network Issues Identified in Audit

| Issue | Severity | Description |
|-------|----------|-------------|
| VLAN naming mismatch | High | Switch and pfSense VLAN names didn't match original design |
| Devices on wrong VLANs | High | Containers scattered across VLANs 20, 30, 50 instead of all on VLAN 30 |
| Legacy LAN active | Medium | 192.168.1.0/24 still hosting Proxmox and NAS |
| No firewall segmentation | Medium | Allow-all rules between VLANs |
| Pi-hole on wrong VLAN | Medium | Pi-hole at 192.168.20.10 instead of VLAN 30 |

### Original Device Placement

| Device | Old IP | Old VLAN |
|--------|--------|----------|
| Proxmox | 192.168.1.5 | Legacy LAN |
| NAS | 192.168.1.15 | Legacy LAN |
| Pi-hole | 192.168.20.10 | VLAN 20 |
| Most containers | 192.168.20.x | VLAN 20 |
| Gaming containers | 192.168.50.x | VLAN 50 |

---

## 2. Target Architecture

### VLAN Design (Final)

| VLAN ID | Name | Subnet | Purpose |
|---------|------|--------|---------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | Infrastructure: Proxmox, pfSense, Switch, NAS |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | Client devices: Gaming PC, Work Laptop |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | All services: Pi-hole, containers |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | Future public-facing services |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | Isolated security lab |

### DNS Architecture

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Clients   │────▶│   pfSense   │────▶│   Pi-hole   │────▶│  Cloudflare │
│  (VLANs)    │     │  DNS Proxy  │     │ 192.168.30.10│    │   1.1.1.1   │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
```

**Why this design:** Direct cross-VLAN DNS queries to Pi-hole were timing out (root cause unknown). Using pfSense as DNS proxy provides reliable DNS for all VLANs while maintaining Pi-hole ad blocking.

---

## 3. Migration Execution

### 3.1 Switch Configuration

**Device:** TP-Link TL-SG108E

| Port | Device | VLAN Mode | PVID | Notes |
|------|--------|-----------|------|-------|
| 1 | pfSense | Trunk | 1 | Tagged: 10, 20, 30, 40, 50 |
| 2 | Proxmox | Trunk | 1 | Tagged: 10, 20, 30, 40, 50 |
| 3 | Work Laptop | Access | 20 | Untagged on VLAN 20 |
| 4 | Gaming PC | Access | 20 | Untagged on VLAN 20 |
| 5 | Pi-hole | Access | 30 | Untagged on VLAN 30 |
| 6 | NAS | Access | 10 | Untagged on VLAN 10 |

**Critical Learning:** Proxmox required a **trunk port** because it uses a VLAN-aware bridge (vmbr0) with VLAN subinterface (vmbr0.10). Access ports strip VLAN tags, breaking connectivity.

### 3.2 Proxmox Network Configuration

**File:** `/etc/network/interfaces`

```bash
auto lo
iface lo inet loopback

iface nic0 inet manual

auto vmbr0
iface vmbr0 inet manual
    bridge-ports nic0
    bridge-stp off
    bridge-fd 0
    bridge-vlan-aware yes
    bridge-vids 2-4094

auto vmbr0.10
iface vmbr0.10 inet static
    address 192.168.10.5/24
    gateway 192.168.10.1
```

### 3.3 Container Migration

All containers migrated to VLAN 30 using:

```bash
pct set <CTID> -net0 name=eth0,bridge=vmbr0,tag=30,ip=192.168.30.<IP>/24,gw=192.168.30.1
pct set <CTID> -nameserver 192.168.30.1
pct stop <CTID> && pct start <CTID>
```

**Final Container IPs:**

| CTID | Name | IP Address |
|------|------|------------|
| 201 | nginx-proxy-manager | 192.168.30.201 |
| 202 | monitoring-prometheus | 192.168.30.202 |
| 203 | monitoring-grafana | 192.168.30.203 |
| 204 | monitoring-loki | 192.168.30.204 |
| 205 | monitoring-alertmanager | 192.168.30.205 |
| 206 | monitoring-uptime | 192.168.30.206 |
| 207 | network-ddns | 192.168.30.207 |
| 300 | gaming-panel | 192.168.30.210 |
| 302 | gaming-wings-1 | 192.168.30.212 |

### 3.4 Pi-hole Fresh Installation

Original Pi-hole became unreachable after VLAN changes. Fresh install performed:

1. Flashed Raspberry Pi OS Lite (64-bit) via Raspberry Pi Imager
2. Discovered new OS uses **NetworkManager** (not dhcpcd)
3. Configured static IP via nmcli:

```bash
sudo nmcli con mod "netplan-eth0" ipv4.addresses 192.168.30.10/24
sudo nmcli con mod "netplan-eth0" ipv4.gateway 192.168.30.1
sudo nmcli con mod "netplan-eth0" ipv4.dns "1.1.1.1 8.8.8.8"
sudo nmcli con mod "netplan-eth0" ipv4.method manual
sudo nmcli con down "netplan-eth0" && sudo nmcli con up "netplan-eth0"
```

4. Installed Pi-hole: `curl -sSL https://install.pi-hole.net | bash`

### 3.5 pfSense DNS Configuration

1. **System → General Setup:**
   - DNS Server 1: 192.168.30.10 (Pi-hole)
   - DNS Server 2: 1.1.1.1 (Cloudflare fallback)

2. **Services → DNS Resolver:**
   - Enable DNS Resolver: ✅
   - Enable Forwarding Mode: ✅
   - DNSSEC: ✅

3. **Services → DHCP Server → Each VLAN:**
   - DNS Server: [VLAN Gateway IP] (e.g., 192.168.10.1 for VLAN 10)

### 3.6 Tailscale Firewall Rules

Added rules to pfSense → Firewall → Rules → Tailscale:

| Action | Protocol | Source | Destination | Description |
|--------|----------|--------|-------------|-------------|
| Pass | Any | Any | This Firewall | Allow Tailscale to pfSense |
| Pass | Any | Any | Any | Allow Tailscale full network access |

---

## 4. Service Configuration Updates

### 4.1 Prometheus (`/etc/prometheus/prometheus.yml`)

Updated all scrape targets to new IPs:

```yaml
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'alertmanager'
    static_configs:
      - targets: ['192.168.30.205:9093']

  - job_name: 'proxmox'
    static_configs:
      - targets: ['192.168.10.5:9100']

  - job_name: 'grafana'
    static_configs:
      - targets: ['192.168.30.203:3000']

  # Additional targets...
```

### 4.2 Grafana Data Sources

- Prometheus: `http://192.168.30.202:9090`
- Loki: `http://192.168.30.204:3100`

### 4.3 Pterodactyl Panel (`.env`)

```
APP_URL=http://192.168.30.210
```

### 4.4 Pterodactyl Wings (`/etc/pterodactyl/config.yml`)

```yaml
remote: 'http://192.168.30.210'
```

---

## 5. Troubleshooting Log

### Issue 1: Proxmox Lost Connectivity After Switch Changes

**Symptom:** Proxmox couldn't ping gateway after switch VLAN changes.

**Root Cause:** Port 2 was set as access (untagged VLAN 10), but Proxmox sends tagged traffic via vmbr0.10.

**Fix:** Changed port 2 to trunk (tagged on all VLANs).

### Issue 2: Cross-VLAN DNS Queries Timing Out

**Symptom:** `nslookup google.com 192.168.30.10` times out from VLANs 10/20, but ping works.

**Root Cause:** Unknown (firewall allows all, port appears open).

**Workaround:** pfSense DNS Resolver with Forwarding Mode. Clients query VLAN gateway, pfSense forwards to Pi-hole.

### Issue 3: Pi-hole Static IP Not Applied

**Symptom:** Edited `/etc/dhcpcd.conf` but IP didn't change after reboot.

**Root Cause:** New Raspberry Pi OS uses NetworkManager, not dhcpcd.

**Fix:** Used `nmcli` instead of editing dhcpcd.conf.

### Issue 4: Pterodactyl Login Timeout

**Symptom:** Panel login showed "timeout of 20000ms exceeded."

**Root Cause:** Container DNS still pointed to old Pi-hole IP (192.168.20.10).

**Fix:** Updated nameserver and restarted container.

### Issue 5: Windows nslookup Using IPv6

**Symptom:** `nslookup` times out, shows `fe80::1` as DNS server.

**Root Cause:** Windows prefers IPv6 DNS. pfSense advertises fe80::1 via Router Advertisement.

**Fix:** Disabled IPv6 on Ethernet adapter.

---

## 6. Current State Summary

### Network

| VLAN | Gateway | DHCP | DNS | Status |
|------|---------|------|-----|--------|
| 10 (Management) | 192.168.10.1 | .100-.200 | 192.168.10.1 | ✅ Active |
| 20 (Main) | 192.168.20.1 | .100-.200 | 192.168.20.1 | ✅ Active |
| 30 (Services) | 192.168.30.1 | .100-.200 | 192.168.30.1 | ✅ Active |
| 40 (DMZ) | 192.168.40.1 | .100-.200 | 1.1.1.1 | ✅ Active |
| 50 (Malware) | 192.168.50.1 | None | None | ✅ Active |

### Devices

| Device | IP | VLAN | Status |
|--------|-----|------|--------|
| Proxmox | 192.168.10.5 | 10 | ✅ Online |
| pfSense | 192.168.10.1 | 10 | ✅ Online |
| NAS | 192.168.10.100 (DHCP) | 10 | ⚠️ Needs static IP |
| Pi-hole | 192.168.30.10 | 30 | ✅ Online |
| Gaming PC | DHCP | 20 | ✅ Online |

### Services

All containers running on VLAN 30 with correct IPs. Monitoring stack operational. Pterodactyl Panel and Wings connected.

---

## 7. Remaining Tasks

| Task | Priority | Status |
|------|----------|--------|
| Set NAS static IP (192.168.10.15) | High | Pending |
| Update Proxmox SMB mount | High | Pending (after NAS) |
| Enable container autostart | Medium | Pending |
| Firewall hardening | Medium | Pending |
| Switch management IP migration | Low | Pending |
| Remove legacy LAN interface | Low | Pending |
| Investigate cross-VLAN issue | Low | Pending |

---

## 8. Key Learnings

1. **Trunk vs Access Ports:** Any device using VLAN-aware bridges or VLAN subinterfaces (like Proxmox) must be on a trunk port.

2. **NetworkManager vs dhcpcd:** Modern Raspberry Pi OS uses NetworkManager. Always verify with `systemctl status NetworkManager` before editing config files.

3. **DNS Architecture:** Using pfSense as DNS proxy is more reliable than direct cross-VLAN DNS queries to Pi-hole.

4. **Container DNS Updates:** `pct set -nameserver` updates Proxmox config but NOT the running container. Must restart container for resolv.conf to update.

5. **Tailscale Firewall:** Tailscale interface requires explicit firewall rules in pfSense. No rules = all traffic blocked.

---

## Appendix: Commands Reference

### Container Network Update
```bash
pct set <CTID> -net0 name=eth0,bridge=vmbr0,tag=30,ip=192.168.30.<IP>/24,gw=192.168.30.1
pct set <CTID> -nameserver 192.168.30.1
pct stop <CTID> && pct start <CTID>
```

### Pi-hole Static IP (NetworkManager)
```bash
sudo nmcli con mod "netplan-eth0" ipv4.addresses 192.168.30.10/24
sudo nmcli con mod "netplan-eth0" ipv4.gateway 192.168.30.1
sudo nmcli con mod "netplan-eth0" ipv4.dns "1.1.1.1 8.8.8.8"
sudo nmcli con mod "netplan-eth0" ipv4.method manual
sudo nmcli con down "netplan-eth0" && sudo nmcli con up "netplan-eth0"
```

### Enable Container Autostart
```bash
pct set <CTID> --onboot 1 --startup order=<N>
```

---

*Documentation created: March 7, 2026*
