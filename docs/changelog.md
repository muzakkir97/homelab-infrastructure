# Changelog

> **Format:** Newest first

---

## 2026-03-09 — Pterodactyl Panel & Game Server Migration Fixes

### Changes Made
- Fixed Pterodactyl Panel external access (panel.najhin-gaming.com)
- Created NPM proxy host for panel with SSL via DNS-01 challenge
- Removed orphaned NPM config file (5.conf) causing nginx conflicts
- Updated Cloudflare DNS records to current public IP (202.184.35.79)
- Changed `panel` DNS record from "DNS only" to "Proxied"
- Fixed pfSense NAT rules (80/443 pointed to Pi-hole instead of NPM)
- Updated Pterodactyl allocations from 192.168.50.12 to 192.168.30.212
- Reassigned Terraria server to 192.168.30.212:7777
- Reassigned Minecraft server to 192.168.30.212:25570
- Deleted old 192.168.50.12 allocations
- Updated pfSense NAT for game servers to new IPs
- Deleted obsolete "Pterodactyl Panel HTTPS" NAT rule (8443→443)
- Rolled Cloudflare API token after SSL setup

### pfSense NAT Rules (Final State)
| Description | Dest Port | NAT IP | NAT Port |
|-------------|-----------|--------|----------|
| Terraria Calamity Server | 7777 | 192.168.30.212 | 7777 |
| Minecraft Server | 25565 | 192.168.30.212 | 25570 |
| HTTPS to NPM | 443 | 192.168.30.201 | 443 |
| HTTP to NPM | 80 | 192.168.30.201 | 80 |

### NPM Proxy Hosts (Final State)
| Domain | Backend | SSL |
|--------|---------|-----|
| cloud.najhin-gaming.com | 192.168.30.220:80 | Cloudflare Tunnel |
| grafana.najhin-gaming.com | 192.168.30.203:3000 | Let's Encrypt |
| panel.najhin-gaming.com | 192.168.30.210:80 | Let's Encrypt |

### Issues Resolved
- Pterodactyl Panel timeout → NPM proxy host + correct NAT
- SSL certificate error → DNS-01 challenge with Cloudflare
- Pi-hole 403 error → Fixed NAT pointing to wrong IP
- Game server bind error → Updated allocations
- Game servers unreachable externally → Fixed NAT rules

---

## 2026-03-08 — Phase 7: Nextcloud Deployment

### Changes Made
- Created CT 220 (nextcloud) on VLAN 30 at 192.168.30.220
- Installed LAMP stack: Apache 2.4.66, PHP 8.2.30, MariaDB 10.11
- Deployed Nextcloud Hub 26 Winter (33.0.0)
- Configured PHP with APCu memcache and OPcache optimization
- Installed Cloudflare Tunnel (cloudflared 2026.2.0) for external access
- External URL: https://cloud.najhin-gaming.com
- Installed apps: Calendar, Contacts, Mail, Notes
- Fixed Uptime Kuma monitors (Grafana SSL, Proxmox SSL, Nextcloud endpoint)
- Configured container autostart with boot order for all 10 containers
- Fixed Pi-hole node_exporter (prometheus-node-exporter wasn't installed)
- Masked smartmontools on Pi-hole (not compatible with SD card)

### Key Decisions
- Cloudflare Tunnel instead of port forwarding (simpler, no ISP router changes)
- Removed Cloudflare Access for Nextcloud (incompatible with mobile app)
- Using Nextcloud built-in brute-force protection instead

### New Container
| CTID | Name | IP | Purpose |
|------|------|----|---------|
| 220 | nextcloud | 192.168.30.220 | Cloud storage (Google Drive replacement) |

---

## 2026-03-07 — Phase 6F: VLAN Migration Complete

### Changes Made
- Migrated all 9 containers from old IPs to VLAN 30 (192.168.30.x)
- Updated Proxmox container network configurations
- Updated pfSense DNS to point to new Pi-hole IP (192.168.30.10)
- Updated all NPM proxy hosts with new backend IPs
- Updated Prometheus scrape targets
- Updated Uptime Kuma monitors
- Verified Tailscale VPN access working

### IP Changes
| Container | Old IP | New IP |
|-----------|--------|--------|
| nginx-proxy-manager | 192.168.30.10 | 192.168.30.201 |
| monitoring-prometheus | 192.168.20.11 | 192.168.30.202 |
| monitoring-grafana | 192.168.20.12 | 192.168.30.203 |
| monitoring-loki | 192.168.20.x | 192.168.30.204 |
| monitoring-alertmanager | 192.168.20.14 | 192.168.30.205 |
| monitoring-uptime | 192.168.20.x | 192.168.30.206 |
| network-ddns | 192.168.20.x | 192.168.30.207 |
| gaming-panel | 192.168.50.10 | 192.168.30.210 |
| gaming-wings-1 | 192.168.50.12 | 192.168.30.212 |

---

## 2026-03-05 — Phase 6F VLAN Restructure (Partial)

### Changes Made
- Deleted old VLANs with incorrect names from pfSense
- Created new VLANs: VLAN10_MGMT, VLAN20_MAIN, VLAN30_SERVICES, VLAN40_DMZ, VLAN50_MALWARE
- Configured DHCP for VLANs 10, 20, 30, 40 (disabled on VLAN 50)
- Created temporary allow-all firewall rules
- Updated switch VLAN configuration (port 4 → VLAN 10, port 5 → VLAN 30)
- Moved Proxmox from 192.168.1.5 to 192.168.10.5 (VLAN 10)
- Created vmbr0.10 subinterface for management

---

## 2026-03-05 — Phase 6F Security Fixes

### Changes Made
- Deleted admin DNS records from Cloudflare (proxmox, pihole, npm, wildcard)
- Deleted orphaned "Proxmox" Cloudflare Access application
- Verified only Grafana proxy host exists in NPM

---

## 2026-03-03 — Phase 9 NAS Deployment

### Changes Made
- Set up UGREEN DXP2800 NAS "Kinmoon"
- Created 3.6TB storage pool (WD Purple 4TB)
- Created SMB share: proxmox-backups
- Added Proxmox storage: kinmoon-smb
- Tested backup of CT 201

### Issues Resolved
- NFS permission denied → switched to SMB
- NAS I/O errors → reboot after pool creation

---

## 2026-02 — Phase 6A-6D Gaming Platform

- Deployed Pterodactyl Panel (CT 300)
- Deployed Pterodactyl Wings (CT 302)
- Set up Terraria server with tModLoader + Calamity
- Configured game server port forwarding

---

## 2026-02 — Phase 5 Monitoring Stack

- Deployed Prometheus (CT 202), Grafana (CT 203), Loki (CT 204)
- Deployed Alertmanager (CT 205), Uptime Kuma (CT 206)
- Configured Telegram + Discord alert channels

---

## 2026-01 — Phases 1-4 Foundation

- Proxmox VE bare-metal installation
- pfSense firewall setup with VLANs
- Core services: Pi-hole, NPM, Tailscale, DDNS
- External access with SSL via Cloudflare

---

*This file is append-only. Add new entries at the top.*
