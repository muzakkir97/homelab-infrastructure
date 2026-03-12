# Current State (Verified)

> **Last Verified:** March 10, 2026  
> **Status:** Phase 6F complete (including firewall hardening), all services operational

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
| Tailscale | 100.110.165.45 | VPN subnet router |

### pfSense Firewall Rules (Hardened)

| Interface | Rules | Policy |
|-----------|-------|--------|
| VLAN10_MGMT | 4 | DNS → VLAN30 services → Block RFC1918 → Internet |
| VLAN20_MAIN | 6 | Gateway → DNS → Services → Games → Block RFC1918 → Internet |
| VLAN30_SERVICES | 5 | DNS → Proxmox metrics → Intra-VLAN → Block RFC1918 → Internet |
| VLAN40_DMZ | 3 | DNS → Block RFC1918 → Internet |
| VLAN50_MALWARE | 0 | Implicit deny (air-gapped) |
| Tailscale | 2 | Full access + pfSense WebUI |

### pfSense Firewall Aliases

| Alias | Type | Values |
|-------|------|--------|
| RFC1918 | Network | 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 |
| DNS_SERVERS | Host | 192.168.30.10 |
| PROXMOX | Host | 192.168.10.5 |
| Monitoring_Servers | Host | 192.168.30.202-206 |
| SERVICE_PORTS | Port | 80, 443, 3000, 3001, 8443 |
| GAME_PORTS | Port | 7777, 25565, 25570 |
| MONITORING_PORTS | Port | 8006, 9100 |

### pfSense NAT Port Forward Rules

| Description | Dest Port | NAT IP | NAT Port | Status |
|-------------|-----------|--------|----------|--------|
| Terraria Calamity Server | 7777 | 192.168.30.212 | 7777 | ✅ Active |
| Minecraft Server | 25565 | 192.168.30.212 | 25570 | ✅ Active |
| HTTPS to NPM | 443 | 192.168.30.201 | 443 | ✅ Active |
| HTTP to NPM | 80 | 192.168.30.201 | 80 | ✅ Active |

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

### Client Devices

| Device | IP Address | VLAN | Tailscale IP |
|--------|------------|------|--------------|
| Gaming PC (Minimoon) | 192.168.20.101 | 20 | 100.106.109.4 |
| Work Laptop | DHCP | 20 | — |

---

## Container Inventory

| CTID | Name | IP Address | VLAN | Cores | RAM | Autostart | Boot Order | Status |
|------|------|------------|------|-------|-----|-----------|------------|--------|
| 201 | nginx-proxy-manager | 192.168.30.201 | 30 | 1 | 512MB | ✅ | 1 | ✅ Running |
| 202 | monitoring-prometheus | 192.168.30.202 | 30 | 1 | 1GB | ✅ | 3 | ✅ Running |
| 203 | monitoring-grafana | 192.168.30.203 | 30 | 1 | 512MB | ✅ | 6 | ✅ Running |
| 204 | monitoring-loki | 192.168.30.204 | 30 | 1 | 1GB | ✅ | 4 | ✅ Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | 30 | 1 | 512MB | ✅ | 5 | ✅ Running |
| 206 | monitoring-uptime | 192.168.30.206 | 30 | **2** | **768MB** | ✅ | 7 | ✅ Running |
| 207 | network-ddns | 192.168.30.207 | 30 | 1 | 256MB | ✅ | 2 | ✅ Running |
| 220 | nextcloud | 192.168.30.220 | 30 | 2 | 4GB | ✅ | 8 | ✅ Running |
| 300 | gaming-panel | 192.168.30.210 | 30 | 2 | 2GB | ✅ | 9 | ✅ Running |
| 302 | gaming-wings-1 | 192.168.30.212 | 30 | 2 | 10GB | ✅ | 10 | ✅ Running |

> **Note:** CT 206 (monitoring-uptime) upgraded from 1 core/512MB to 2 cores/768MB on March 10, 2026 to resolve high CPU alerts during periodic maintenance tasks.

---

## External Services

| Service | Details | Status |
|---------|---------|--------|
| Cloudflare DNS | grafana, cloud, panel (proxied), mc/terraria (DNS only) | ✅ Active |
| Cloudflare Access | Grafana (email OTP) | ✅ Active |
| Cloudflare Tunnel | homelab-tunnel → Nextcloud (cloud.najhin-gaming.com) | ✅ Active |
| Tailscale | pfSense (100.110.165.45) + Gaming PC (100.106.109.4) | ✅ Active |

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

## Inter-VLAN Access Matrix

| From → To | VLAN 10 | VLAN 20 | VLAN 30 | VLAN 40 | VLAN 50 | Internet |
|-----------|---------|---------|---------|---------|---------|----------|
| VLAN 10 | ✅ | ❌ | ✅ Limited | ❌ | ❌ | ✅ |
| VLAN 20 | ❌ | ✅ | ✅ Limited | ❌ | ❌ | ✅ |
| VLAN 30 | ✅ Metrics | ❌ | ✅ | ❌ | ❌ | ✅ |
| VLAN 40 | ❌ | ❌ | ❌ | ✅ | ❌ | ✅ |
| VLAN 50 | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| Tailscale | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## Pending Changes

| Item | Current | Target | Priority |
|------|---------|--------|----------|
| Backup strategy | Not implemented | Phase 7A | High |
| Switch management IP | 192.168.1.20 | 192.168.10.20 | Medium |
| Legacy LAN | Active | Remove | Medium |
| Nextcloud 2FA | Disabled | TOTP enabled | Low |
| Nextcloud data → NAS | Local disk | NAS mount | Low |

---

*Last updated: March 10, 2026*
