# Service Catalog

> **Last Updated:** March 5, 2026

---

## Core Infrastructure

| Service | Type | Target IP | Port | Role |
|---------|------|-----------|------|------|
| pfSense | Dedicated hardware (N100) | 192.168.10.1 | 443 | Firewall, Router, DHCP, NAT |
| Pi-hole | Bare metal (RPi4) | 192.168.30.10 | 80 | DNS filtering, ad blocking |
| Nginx Proxy Manager | LXC (CT 201) | 192.168.30.201 | 80/443/81 | Reverse proxy, SSL termination |
| Tailscale | On pfSense | — | — | VPN subnet router |
| Cloudflare DDNS | LXC (CT 207) | 192.168.30.207 | — | Dynamic DNS updates |

## Monitoring Stack

| Service | CTID | Target IP | Port | Role |
|---------|------|-----------|------|------|
| Prometheus | 202 | 192.168.30.202 | 9090 | Metrics collection |
| Grafana | 203 | 192.168.30.203 | 3000 | Dashboards (external via CF Access) |
| Loki | 204 | 192.168.30.204 | 3100 | Log aggregation |
| Alertmanager | 205 | 192.168.30.205 | 9093 | Alert routing (Telegram + Discord) |
| Uptime Kuma | 206 | 192.168.30.206 | 3001 | Service availability monitoring |

## Gaming Platform

| Service | CTID | Target IP | Port | Role |
|---------|------|-----------|------|------|
| Pterodactyl Panel | 300 | 192.168.30.210 | 443 | Game server management UI |
| Pterodactyl Wings | 302 | 192.168.30.212 | — | Game server daemon (10GB RAM) |
| Terraria (Calamity) | via Wings | — | 7777 | Game server (offline) |
| Minecraft | via Wings | — | 25570 | Game server (offline) |

## Storage

| ID | Type | Server | Content |
|----|------|--------|---------|
| local | Directory | /var/lib/vz | ISOs, templates |
| local-lvm | LVM-Thin | pve/data | VM/CT disks |
| kinmoon-smb | SMB/CIFS | 192.168.1.15 | Backups |

## Service Dependencies

```
Internet → Cloudflare DNS → NPM → Grafana
                                 → Pterodactyl Panel → Wings → Game Servers

pfSense → Pi-hole (DNS) → All containers

Prometheus → Node exporters (all CTs) → Alertmanager → Telegram / Discord
Loki → Grafana

Proxmox → Kinmoon NAS (backups)
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

---

*Last updated: March 5, 2026*
