# Phase 6A-6D: Gaming Platform Deployment

> **Status:** ✅ Complete | **Completed:** February 2026

---

## Overview

Phase 6 deployed a professional gaming server management platform using Pterodactyl Panel and Wings, with the first game server being Terraria with tModLoader and the Calamity mod.

### Objectives

| Objective | Status |
|-----------|--------|
| Create isolated Gaming VLAN | ✅ Complete (VLAN 50, later merged to VLAN 30) |
| Deploy Pterodactyl Panel | ✅ Complete (CT 300) |
| Deploy Pterodactyl Wings | ✅ Complete (CT 302) |
| Configure Terraria server | ✅ Complete (tModLoader + Calamity) |
| External access via domain | ✅ Complete (terraria.najhin-gaming.com) |

---

## Sub-Phases

### Phase 6A: Gaming Network Foundation
- Created VLAN 50 on pfSense with security isolation
- Configured TP-Link switch for VLAN tagging
- Set up firewall rules with default-deny approach
- Established NAT port forwarding chains

### Phase 6B: Minecraft Server (Initial Test)
- Deployed Minecraft Paper 1.21.4 in Docker
- Tested external access via mc.najhin-gaming.com
- Server currently offline (used for testing only)

### Phase 6C: Pterodactyl Panel Deployment
- Deployed CT 300 (gaming-panel) with:
  - MariaDB database
  - Redis cache
  - Nginx web server
  - PHP 8.1
- Configured SSL via Nginx Proxy Manager
- Added monitoring (node_exporter for Prometheus)

### Phase 6D: Pterodactyl Wings & Terraria
- Deployed CT 302 (gaming-wings-1) with Docker Engine
- Configured Panel-to-Wings communication
- Deployed Terraria server with:
  - tModLoader (Preview v2026.1.2.1)
  - Calamity mod
  - Port 7777 external access

---

## Architecture

```
Internet
    │
    ▼ (Port 7777)
pfSense NAT
    │
    ▼
┌─────────────────────────────────────────┐
│           Pterodactyl Wings             │
│              (CT 302)                   │
│  ┌─────────────────────────────────┐   │
│  │         Docker Engine            │   │
│  │  ┌─────────────────────────┐    │   │
│  │  │   Terraria Container    │    │   │
│  │  │   tModLoader + Calamity │    │   │
│  │  └─────────────────────────┘    │   │
│  └─────────────────────────────────┘   │
└─────────────────────────────────────────┘
           ▲
           │ API
           ▼
┌─────────────────────────────────────────┐
│          Pterodactyl Panel              │
│              (CT 300)                   │
│    MariaDB │ Redis │ Nginx │ PHP       │
└─────────────────────────────────────────┘
           ▲
           │ HTTPS (panel.najhin-gaming.com)
           ▼
       NPM + SSL
```

---

## Configuration Details

### Pterodactyl Panel (CT 300)

| Setting | Value |
|---------|-------|
| Container ID | 300 |
| Hostname | gaming-panel |
| IP Address | 192.168.50.10 → 192.168.30.210 (target) |
| OS | Debian 12 |
| Resources | 2 cores, 2GB RAM, 20GB disk |
| External URL | panel.najhin-gaming.com |

### Pterodactyl Wings (CT 302)

| Setting | Value |
|---------|-------|
| Container ID | 302 |
| Hostname | gaming-wings-1 |
| IP Address | 192.168.50.12 → 192.168.30.212 (target) |
| OS | Debian 12 |
| Resources | 4 cores, 10GB RAM, 80GB disk |
| Docker | Enabled (nested containers) |

### Terraria Server

| Setting | Value |
|---------|-------|
| Version | tModLoader Preview v2026.1.2.1 |
| Mod | Calamity |
| Port | 7777 |
| External | terraria.najhin-gaming.com:7777 |
| Max Players | 8 |

---

## Issues Encountered & Resolutions

| Issue | Resolution |
|-------|------------|
| Docker image wrong runtime | Modern tModLoader needs .NET, not Mono |
| Mod upload failing (413 error) | Increase nginx `client_max_body_size` to 500M |
| Panel-Wings version mismatch | Pin both to matching release versions |
| Game server unreachable externally | Cloudflare must be "DNS only" (grey cloud) |
| World generation appearing stuck | Actually slow chunk loading — increase RAM allocation |
| enabled.json being overwritten | Mod loading order issue — manual config |

---

## Port Forwarding Chain

```
Internet:7777 → ISP Router:7777 → pfSense:7777 → Wings:7777 → Terraria Container
```

**Cloudflare DNS:** Must use "DNS only" mode (grey cloud) for game server traffic — Cloudflare proxy doesn't support arbitrary TCP ports.

---

## Lessons Learned

1. **Nested Docker in LXC** requires privileged container with specific AppArmor settings
2. **Game server DNS** cannot use Cloudflare proxy — must be DNS only
3. **Double NAT** requires port forwarding at both ISP router and pfSense
4. **tModLoader versions** must match exactly between client and server
5. **Large file uploads** need both Nginx and PHP limits increased

---

## Future Enhancements (Phase 8)

- Discord bot for server start/stop
- Additional servers (Minecraft expansion, Project Zomboid)
- Automated backup via Pterodactyl schedules
- On-demand server wake with infrared proxy

---

*Documentation created: March 2026*
