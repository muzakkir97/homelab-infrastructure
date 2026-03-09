# Roadmap

> **Last Updated:** March 9, 2026

---

## Completed Phases

| Phase | Description | Completed |
|-------|-------------|-----------|
| 1 | Proxmox VE bare-metal installation | Jan 2026 |
| 2 | pfSense firewall & VLAN setup (router-on-a-stick) | Jan 2026 |
| 3 | Core services: Pi-hole, NPM, Tailscale, Cloudflare DDNS | Jan 2026 |
| 4 | External access: domain, SSL (Let's Encrypt), Cloudflare | Feb 2026 |
| 5 | Monitoring: Prometheus, Grafana, Loki, Alertmanager, Uptime Kuma | Feb 2026 |
| 6A-6D | Gaming: Pterodactyl Panel + Wings, Terraria (Calamity) | Feb 2026 |
| 9 | NAS: UGREEN DXP2800 (Kinmoon), SMB backups | Mar 3, 2026 |
| 6F | Infrastructure Audit, VLAN Migration & Firewall Hardening | Mar 9, 2026 |
| 7 | Nextcloud Deployment + Cloudflare Tunnel | Mar 8, 2026 |

---

## Current Status

**All major infrastructure work complete.** 10 containers running with autostart configured. VLAN segmentation operational with proper firewall rules enforced. External access via Cloudflare Tunnel (Nextcloud) and Cloudflare Access (Grafana). Admin access via Tailscale only.

---

## Planned Phases

| Phase | Description | Priority | Depends On |
|-------|-------------|----------|------------|
| 7A | Backup Strategy (scheduled, rotation, recovery) | High | — |
| 6E | Homepage Dashboard (gethomepage.dev) | Medium | — |
| 7B | n8n Workflow Automation (Discord/Telegram bots) | Medium | 7A |
| 7C | AI Agent Deployment (OpenClaw) | Low | 7B |
| 8 | Gaming Expansion (Minecraft, Project Zomboid) | Low | — |

---

## Future Services Under Consideration

| Service | Category | Effort | Notes |
|---------|----------|--------|-------|
| Gitea | DevOps / Git | Low | CI/CD foundation |
| Drone CI / Woodpecker | DevOps / CI | Medium | Integrates with Gitea |
| Ansible Semaphore | IaC | Medium | Replace manual commands |
| Authentik | Identity / SSO | Medium | Central auth for all services |
| Mailcow / Mox | Email | Medium | Self-hosted email |
| Netbox | Network / IPAM | Low | Professional IP management |
| HashiCorp Vault | Security | Low | Secrets management |
| Homepage Dashboard | Dashboard | Medium | Phase 6E |
| Jellyfin | Media | Medium | Needs storage planning |
| Vaultwarden | Security | Low | Password manager |
| Obsidian + Syncthing | Knowledge Mgmt | Low | Self-hosted note sync |

---

## Discarded Ideas

| Idea | Reason |
|------|--------|
| Separate Gaming VLAN | Adds complexity, merged into VLAN 30 |
| Public Web Hosting | Use cloud providers instead |
| Cloudflare Access for Nextcloud | Incompatible with mobile app |

---

## Timeline

```
2026
├── Jan-Feb: Phases 1-5 (Foundation)
├── Feb: Phase 6A-6D (Gaming)
├── Mar 3: Phase 9 (NAS) ✅
├── Mar 7-9: Phase 6F (VLAN Migration + Firewall Hardening) ✅
├── Mar 8: Phase 7 (Nextcloud) ✅
├── Mar-Apr: Phase 7A (Backups) 📋 ← NEXT
├── Apr: Phase 6E (Homepage) 📋
├── Apr-May: Phase 7B (n8n Automation) 📋
└── Q2-Q3: Phase 8 (Gaming Expansion) 📋
```

---

*Last updated: March 9, 2026*
