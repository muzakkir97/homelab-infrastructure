# Current State (Verified)

> **Last Verified:** March 7, 2026  
> **Status:** VLAN migration complete, minor tasks pending

---

## Network Configuration

### VLAN Structure

| VLAN ID | Name | Subnet | Gateway | DHCP Range | Status |
|---------|------|--------|---------|------------|--------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | 192.168.10.1 | .100-.200 | ✅ Active |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | 192.168.20.1 | .100-.200 | ✅ Active |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | .100-.200 | ✅ Active |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | 192.168.40.1 | .100-.200 | ✅ Active |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | 192.168.50.1 | None | ✅ Active |
| — | LAN (legacy) | 192.168.1.0/24 | 192.168.1.1 | None | ⚠️ To be removed |

### DNS Architecture

**Flow:** Clients → pfSense (VLAN Gateway) → Pi-hole (192.168.30.10) → Cloudflare (1.1.1.1)

- pfSense DNS Resolver enabled with Forwarding Mode
- DHCP assigns VLAN gateway IP as DNS server
- pfSense forwards all queries to Pi-hole

### pfSense Interfaces

| Interface | IP Address | Description |
|-----------|------------|-------------|
| WAN | DHCP from ISP | ISP connection |
| LAN | 192.168.1.1/24 | Legacy (to be removed) |
| VLAN10_MGMT | 192.168.10.1/24 | Management |
| VLAN20_MAIN | 192.168.20.1/24 | Client devices |
| VLAN30_SERVICES | 192.168.30.1/24 | All services |
| VLAN40_DMZ | 192.168.40.1/24 | Public-facing |
| VLAN50_MALWARE | 192.168.50.1/24 | Isolated sandbox |

### Switch Configuration (TP-Link TL-SG108E)

| Port | Device | VLAN Mode | PVID |
|------|--------|-----------|------|
| 1 | pfSense | Trunk | 1 |
| 2 | Proxmox | Trunk | 1 |
| 3 | Work Laptop | Access | 20 |
| 4 | Gaming PC | Access | 20 |
| 5 | Pi-hole | Access | 30 |
| 6 | NAS | Access | 10 |

---

## Device Inventory

### Infrastructure Devices

| Device | IP Address | VLAN | Status |
|--------|------------|------|--------|
| Proxmox Server (Ryzen 5600X, 32GB) | 192.168.10.5 | 10 | ✅ Online |
| pfSense (AC8F N100) | 192.168.10.1 | 10 | ✅ Online |
| TP-Link TL-SG108E Switch | 192.168.1.20 | LAN | ✅ Online |
| UGREEN NAS (Kinmoon) | 192.168.10.100 (DHCP) | 10 | ⚠️ Needs static IP |
| Raspberry Pi 4 (Pi-hole) | 192.168.30.10 | 30 | ✅ Online |

---

## Container Inventory

| CTID | Name | IP Address | VLAN | Status |
|------|------|------------|------|--------|
| 201 | nginx-proxy-manager | 192.168.30.201 | 30 | ✅ Running |
| 202 | monitoring-prometheus | 192.168.30.202 | 30 | ✅ Running |
| 203 | monitoring-grafana | 192.168.30.203 | 30 | ✅ Running |
| 204 | monitoring-loki | 192.168.30.204 | 30 | ✅ Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | 30 | ✅ Running |
| 206 | monitoring-uptime | 192.168.30.206 | 30 | ✅ Running |
| 207 | network-ddns | 192.168.30.207 | 30 | ✅ Running |
| 300 | gaming-panel | 192.168.30.210 | 30 | ✅ Running |
| 302 | gaming-wings-1 | 192.168.30.212 | 30 | ✅ Running |

---

## External Services

| Service | Details | Status |
|---------|---------|--------|
| Cloudflare DNS | grafana (proxied), panel/mc/terraria (DNS only) | ✅ Active |
| Cloudflare Access | Grafana (email OTP) | ✅ Active |
| Tailscale | pfSense subnet router (100.110.165.45) | ✅ Active |

---

## Pending Changes

| Item | Current | Target | Priority |
|------|---------|--------|----------|
| NAS static IP | 192.168.10.100 (DHCP) | 192.168.10.15 | High |
| Proxmox SMB mount | 192.168.1.15 | 192.168.10.15 | High |
| Container autostart | Disabled | Enabled | Medium |
| Firewall rules | Allow-all | Proper segmentation | Medium |
| Switch management IP | 192.168.1.20 | 192.168.10.20 | Low |
| Legacy LAN | Active | Remove | Low |

---

*Last updated: March 7, 2026*
