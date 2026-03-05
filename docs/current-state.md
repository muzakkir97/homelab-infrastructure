# Current State (Verified)

> **Last Verified:** March 5, 2026  
> **Status:** VLAN restructure complete, service migration pending

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

### pfSense Interfaces

| Interface | IP Address | Description |
|-----------|------------|-------------|
| WAN | DHCP from ISP | ISP connection |
| LAN | 192.168.1.1/24 | Legacy flat network |
| VLAN10_MGMT | 192.168.10.1/24 | Management |
| VLAN20_MAIN | 192.168.20.1/24 | Client devices |
| VLAN30_SERVICES | 192.168.30.1/24 | All services |
| VLAN40_DMZ | 192.168.40.1/24 | Public-facing |
| VLAN50_MALWARE | 192.168.50.1/24 | Isolated sandbox |

---

## Device Inventory

| Device | IP Address | VLAN | Status |
|--------|------------|------|--------|
| Proxmox Server (Ryzen 5600X, 32GB) | 192.168.10.5 | 10 | ✅ Online |
| pfSense (AC8F N100) | 192.168.10.1 | 10 | ✅ Online |
| TP-Link TL-SG108E Switch | 192.168.1.20 | LAN | ✅ Online |
| UGREEN NAS (Kinmoon) | 192.168.1.15 | LAN | ✅ Online |
| Raspberry Pi 4 (Pi-hole) | 192.168.20.10 | 20→30 | ⏳ Migrating |

---

## Container Inventory

| CTID | Name | Target IP | Running | Purpose |
|------|------|-----------|---------|---------|
| 201 | nginx-proxy-manager | 192.168.30.201 | Yes | Reverse proxy, SSL |
| 202 | monitoring-prometheus | 192.168.30.202 | No | Metrics collection |
| 203 | monitoring-grafana | 192.168.30.203 | No | Dashboards |
| 204 | monitoring-loki | 192.168.30.204 | No | Log aggregation |
| 205 | monitoring-alertmanager | 192.168.30.205 | No | Alert routing |
| 206 | monitoring-uptime | 192.168.30.206 | Yes | Uptime Kuma |
| 207 | network-ddns | 192.168.30.207 | Yes | Cloudflare DDNS |
| 300 | gaming-panel | 192.168.30.210 | Yes | Pterodactyl Panel |
| 302 | gaming-wings-1 | 192.168.30.212 | Yes | Pterodactyl Wings |

---

## External Services

| Service | Records | Status |
|---------|---------|--------|
| Cloudflare DNS | grafana (proxied), panel/mc/terraria (DNS only) | ✅ Active |
| Cloudflare Access | Grafana (email OTP) | ✅ Active |
| Tailscale | pfSense subnet router | ✅ Active |

---

## Pending Changes

| Item | Current | Target | Priority |
|------|---------|--------|----------|
| All containers | Old VLANs | VLAN 30 | High |
| Pi-hole | 192.168.20.10 | 192.168.30.10 | High |
| Firewall rules | Allow-all (temp) | Proper segmentation | Medium |
| NAS | 192.168.1.15 | 192.168.10.15 | Medium |

---

*Last updated: March 5, 2026*
