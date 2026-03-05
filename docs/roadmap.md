# Roadmap

> **Last Updated:** March 5, 2026

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

---

## Current Phase

### Phase 6F: Infrastructure Audit & Correction 🔧
**Progress:** 75%

**Completed:**
- Security audit of all components
- Removed admin DNS records and wildcard from Cloudflare
- Created correct VLANs in pfSense (10, 20, 30, 40, 50)
- Updated switch VLAN configuration
- Moved Proxmox to VLAN 10 (192.168.10.5)

**In Progress:**
- Container network migration to VLAN 30 (9 containers)
- Pi-hole IP migration to 192.168.30.10
- pfSense DNS update

**Pending:**
- Replace allow-all firewall rules with proper segmentation
- Update monitoring targets and monitors
- Configure container autostart

---

## Planned Phases

| Phase | Description | Priority | Depends On |
|-------|-------------|----------|------------|
| 6E | Homepage Dashboard (gethomepage.dev) | Medium | 6F |
| 7A | Backup Strategy (scheduled, rotation, recovery) | High | 6F |
| 7B | n8n Workflow Automation (Discord/Telegram bots) | Medium | 7A |
| 7C | AI Agent Deployment | Low | 7B |
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
| Nextcloud | Productivity | High | Cloud storage alternative |
| Vaultwarden | Security | Low | Password manager |
| Obsidian + Syncthing | Knowledge Mgmt | Low | Self-hosted note sync for markdown docs |

---

## Discarded Ideas

| Idea | Reason |
|------|--------|
| Separate Gaming VLAN | Adds complexity, merged into VLAN 30 |
| Public Web Hosting | Use cloud providers instead |

---

## Timeline

```
2026
├── Jan-Feb: Phases 1-5 (Foundation)
├── Feb: Phase 6A-6D (Gaming)
├── Mar 3: Phase 9 (NAS) ✅
├── Mar 5: Phase 6F (Audit) 🔧 ← CURRENT
├── Mar: Phase 6E (Homepage) 📋
├── Apr: Phase 7A (Backups) 📋
├── Apr-May: Phase 7B (n8n Automation) 📋
└── Q2-Q3: Phase 8 (Gaming Expansion) 📋
```

---

*Last updated: March 5, 2026*
