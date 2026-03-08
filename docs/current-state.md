# Current State (Verified)

> **Last Verified:** March 8, 2026  
> **Status:** Phase 7 complete, all services operational

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
| UGREEN NAS (Kinmoon) | 192.168.10.15 | 10 | ✅ Online |
| Raspberry Pi 4 (Pi-hole) | 192.168.30.10 | 30 | ✅ Online |

---

## Container Inventory

| CTID | Name | IP Address | VLAN | Autostart | Boot Order | Status |
|------|------|------------|------|-----------|------------|--------|
| 201 | nginx-proxy-manager | 192.168.30.201 | 30 | ✅ | 1 | ✅ Running |
| 202 | monitoring-prometheus | 192.168.30.202 | 30 | ✅ | 3 | ✅ Running |
| 203 | monitoring-grafana | 192.168.30.203 | 30 | ✅ | 6 | ✅ Running |
| 204 | monitoring-loki | 192.168.30.204 | 30 | ✅ | 4 | ✅ Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | 30 | ✅ | 5 | ✅ Running |
| 206 | monitoring-uptime | 192.168.30.206 | 30 | ✅ | 7 | ✅ Running |
| 207 | network-ddns | 192.168.30.207 | 30 | ✅ | 2 | ✅ Running |
| 220 | nextcloud | 192.168.30.220 | 30 | ✅ | 8 | ✅ Running |
| 300 | gaming-panel | 192.168.30.210 | 30 | ✅ | 9 | ✅ Running |
| 302 | gaming-wings-1 | 192.168.30.212 | 30 | ✅ | 10 | ✅ Running |

---

## External Services

| Service | Details | Status |
|---------|---------|--------|
| Cloudflare DNS | grafana, cloud (proxied), panel/mc/terraria (DNS only) | ✅ Active |
| Cloudflare Access | Grafana (email OTP) | ✅ Active |
| Cloudflare Tunnel | homelab-tunnel → Nextcloud (cloud.najhin-gaming.com) | ✅ Active |
| Tailscale | pfSense subnet router (100.110.165.45) | ✅ Active |

---

## Uptime Kuma Monitors

| Monitor | Target | Status |
|---------|--------|--------|
| Alertmanager | http://192.168.30.205:9093 | ✅ UP |
| Grafana | http://192.168.30.203:3000 | ✅ UP |
| NextCloud | http://192.168.30.220/status.php | ✅ UP |
| Pi-hole | http://192.168.30.10 | ✅ UP |
| Prometheus | http://192.168.30.202:9090 | ✅ UP |
| Proxmox | https://192.168.10.5:8006 (ignore SSL) | ✅ UP |

---

## Pending Changes

| Item | Current | Target | Priority |
|------|---------|--------|----------|
| Firewall rules | Allow-all | Proper segmentation | High |
| Switch management IP | 192.168.1.20 | 192.168.10.20 | Low |
| Legacy LAN | Active | Remove | Low |
| Nextcloud 2FA | Disabled | TOTP enabled | Low |
| Nextcloud data → NAS | Local disk | NAS mount | Medium |

---

*Last updated: March 8, 2026*
