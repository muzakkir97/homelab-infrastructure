# Service Catalog

> **Last Updated:** March 9, 2026

---

## Core Infrastructure

| Service | Type | Target IP | Port | Role |
|---------|------|-----------|------|------|
| pfSense | Dedicated hardware (N100) | 192.168.10.1 | 443 | Firewall, Router, DHCP, NAT |
| Pi-hole | Bare metal (RPi4) | 192.168.30.10 | 80 | DNS filtering, ad blocking |
| Nginx Proxy Manager | LXC (CT 201) | 192.168.30.201 | 80/443/81 | Reverse proxy, SSL termination |
| Tailscale | On pfSense + Gaming PC | 100.110.165.45 | — | VPN subnet router |
| Cloudflare DDNS | LXC (CT 207) | 192.168.30.207 | — | Dynamic DNS updates |

## Monitoring Stack

| Service | CTID | Target IP | Port | Role |
|---------|------|-----------|------|------|
| Prometheus | 202 | 192.168.30.202 | 9090 | Metrics collection |
| Grafana | 203 | 192.168.30.203 | 3000 | Dashboards (external via CF Access) |
| Loki | 204 | 192.168.30.204 | 3100 | Log aggregation |
| Alertmanager | 205 | 192.168.30.205 | 9093 | Alert routing (Telegram + Discord) |
| Uptime Kuma | 206 | 192.168.30.206 | 3001 | Service availability monitoring |

## Productivity

| Service | CTID | Target IP | Port | Role |
|---------|------|-----------|------|------|
| Nextcloud | 220 | 192.168.30.220 | 80 | Cloud storage, calendar, contacts |

## Gaming Platform

| Service | CTID | Target IP | Port | Role |
|---------|------|-----------|------|------|
| Pterodactyl Panel | 300 | 192.168.30.210 | 443 | Game server management UI |
| Pterodactyl Wings | 302 | 192.168.30.212 | — | Game server daemon (10GB RAM) |
| Terraria (Calamity) | via Wings | — | 7777 | Game server |
| Minecraft | via Wings | — | 25570 | Game server |

## Storage

| ID | Type | Server | Content |
|----|------|--------|---------|
| local | Directory | /var/lib/vz | ISOs, templates |
| local-lvm | LVM-Thin | pve/data | VM/CT disks |
| kinmoon-smb | SMB/CIFS | 192.168.10.15 | Backups |

## External Services

| Service | Type | Endpoint | Purpose |
|---------|------|----------|---------|
| Cloudflare Tunnel | homelab-tunnel | cloud.najhin-gaming.com → localhost:80 | Nextcloud external access |
| Cloudflare Access | — | grafana.najhin-gaming.com | Zero-trust auth (email OTP) |
| Let's Encrypt | DNS-01 | — | SSL certificates via Cloudflare |

## pfSense Firewall Aliases

### IP Aliases

| Alias | Type | Values | Purpose |
|-------|------|--------|---------|
| RFC1918 | Network | 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 | Block private networks |
| DNS_SERVERS | Host | 192.168.30.10 | Pi-hole |
| PROXMOX | Host | 192.168.10.5 | Proxmox server |
| Monitoring_Servers | Host | 192.168.30.202-206 | Monitoring stack |

### Port Aliases

| Alias | Type | Values | Purpose |
|-------|------|--------|---------|
| SERVICE_PORTS | Port | 80, 443, 3000, 3001, 8443 | Web services |
| GAME_PORTS | Port | 7777, 25565, 25570 | Game servers |
| MONITORING_PORTS | Port | 8006, 9100 | Proxmox + Node Exporter |

## Service Dependencies

```
Internet → Cloudflare DNS → NPM → Grafana
                               → Pterodactyl Panel → Wings → Game Servers

Internet → Cloudflare Tunnel → Nextcloud (CT 220)

pfSense → Pi-hole (DNS) → All containers

Prometheus → Node exporters (all CTs + Pi-hole) → Alertmanager → Telegram / Discord
Loki → Grafana

Proxmox → Kinmoon NAS (backups)

Tailscale → pfSense (subnet router) → All VLANs (admin bypass)
```

## Port Reference

| Port | Service |
|------|---------|
| 80/443 | HTTP/HTTPS |
| 81 | NPM Admin |
| 3000 | Grafana |
| 3001 | Uptime Kuma |
| 3100 | Loki |
| 7777 | Terraria |
| 8006 | Proxmox |
| 9090 | Prometheus |
| 9093 | Alertmanager |
| 9100 | Node Exporter |
| 25570 | Minecraft |

## Container Boot Order

| Order | CTID | Name | Reason |
|-------|------|------|--------|
| 1 | 201 | nginx-proxy-manager | Reverse proxy first |
| 2 | 207 | network-ddns | DNS updates |
| 3 | 202 | monitoring-prometheus | Metrics collection |
| 4 | 204 | monitoring-loki | Log aggregation |
| 5 | 205 | monitoring-alertmanager | Alert routing |
| 6 | 203 | monitoring-grafana | Dashboards (needs Prometheus/Loki) |
| 7 | 206 | monitoring-uptime | Service monitoring |
| 8 | 220 | nextcloud | Cloud storage |
| 9 | 300 | gaming-panel | Game management |
| 10 | 302 | gaming-wings-1 | Game servers (needs Panel) |

---

*Last updated: March 9, 2026*
