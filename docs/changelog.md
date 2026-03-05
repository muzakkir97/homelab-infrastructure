# Changelog

> **Format:** Newest first

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

### Pending
- Container network migration (9 containers → VLAN 30)
- Pi-hole IP update, pfSense DNS update
- Service configuration updates

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
