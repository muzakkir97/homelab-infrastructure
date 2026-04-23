# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** April 23, 2026
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## ✅ Completed Phases

| Phase    | Title                                                      | Completed        |
|----------|------------------------------------------------------------|------------------|
| 1        | Proxmox VE Installation                                    | Jan 2026         |
| 2        | pfSense Firewall & VLAN Setup                              | Jan 2026         |
| 3        | Core Services (Pi-hole, NPM, Tailscale, DDNS)              | Jan 2026         |
| 4        | External Access & SSL                                      | Feb 2026         |
| 5        | Monitoring Stack (Prometheus, Grafana, Loki, Alertmanager) | Feb 2026         |
| 6A–6D    | Gaming Platform (Pterodactyl, Terraria, Minecraft)         | Feb 2026         |
| 6E       | Homepage Dashboard (gethomepage.dev)                       | Mar 2026         |
| 6F       | Infrastructure Audit, VLAN Migration & Firewall Hardening  | Mar 9, 2026      |
| 7        | Nextcloud Deployment                                       | Mar 8, 2026      |
| 7A       | Backup Strategy (Proxmox vzdump, NAS + local)              | Mar 13, 2026     |
| 7B       | n8n Automation Hub                                         | Apr 2, 2026      |
| 7C       | Gilgamesh Telegram Bot + GitHub Integration                | Apr 2, 2026      |
| 7D       | Gilgamesh Enhancements (Memory, Routing, Web Search)       | Apr 6, 2026      |
| 7D-Sec   | Cloudflare Access for n8n                                  | Apr 7, 2026      |
| 9        | NAS Deployment (Kinmoon — UGREEN DXP2800)                  | Mar 3, 2026      |
| 13       | HashiCorp Vault — Secrets Manager                          | Apr 18, 2026     |
| 16.1     | Documentation Pipeline - Update Workflow                   | Apr 19, 2026     |
| 16.2     | Documentation Pipeline - Sync Docs Workflow                | Apr 19, 2026     |
| 23       | Vaultwarden + Secrets Audit & Cleanup                      | Apr 18, 2026     |
| 58       | Windrose Server Deployment                                 | Apr 19, 2026     |

---

## 🔄 In Progress

| Phase    | Title                          | Notes                                                           |
|----------|--------------------------------|-----------------------------------------------------------------|
| 7D-Menu  | Gilgamesh Inline Keyboard Menu | Status working. Metrics, Temps, Storage, Gaming, Tools pending. |

---

## 📋 Planned Phases

> Phases continue sequentially, with Gilgamesh Evolution integrated as primary near-term focus.
> Sub-phases use dot notation: e.g. 22.1, 22.2
> Completed phases retain original numbering for historical continuity.

---

## 🎯 **PRIMARY GOAL: Gilgamesh Evolution**

**Mission:** Replace claude.ai entirely with Gilgamesh as primary AI interface via Telegram

**Timeline:** 16 weeks (~August 2026)  
**Target Cost:** $5-10/month (down from $20+ Claude Pro subscription)

**Success Criteria:**
- 20+ message conversations without context loss
- RAG recalls decisions from weeks-old sessions
- Vision API handles screenshot analysis
- Artifact generation (code/docs downloadable)
- 90% response quality match with Claude Web
- API cost consistently <$10/month

---

### **Phase 1: Foundation (Weeks 1-4)**

| Phase | Title                          | Description                                                                       | Depends On | Evolution Stage |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|-----------------|
| 14    | Gilgamesh Menu — Remaining     | Complete Metrics, Temps, Storage, Gaming, Gilgamesh, Tools, Help submenus         | 7D-Menu    | Prerequisites   |
| 15    | Gilgamesh Additional Commands  | /temps, /storage, /alerts, /logs, /backup, /cost, /memory, /clear, /help          | 14         | Prerequisites   |
| **38** | **Ollama + ROCm Local LLM** | **RX 6700 XT on Kuromoon, 7B-32B models, FREE inference for 80% of queries** | — | **🔥 CRITICAL** |
| **22** | **Obsidian Knowledge Base** | **Obsidian + Nextcloud WebDAV + claude-obsidian plugin + RAG + Dataview subscription tracker** | — | **🔥 CRITICAL** |

**Deliverable:** Gilgamesh handles documentation FREE, Obsidian becomes knowledge layer with RAG

---

### **Phase 2: Intelligence (Weeks 5-7)**

| Phase | Title                          | Description                                                                       | Depends On | Evolution Stage |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|-----------------|
| **7E** | **Extended Memory (50-100 msgs)** | **Long planning sessions, conversation context expansion** | 22, 38 | **Intelligence** |
| **41** | **Gilgamesh Hybrid Routing** | **Smart routing: Ollama (free) → Claude API (paid) only when needed, cost optimization** | 38 | **Intelligence** |
| 16    | Gilgamesh /update Redesign     | Accept .md file attachment via Telegram, replace GitHub + Nextcloud workflow      | 38, 41     | Workflow        |

**Deliverable:** Gilgamesh can handle complex multi-turn conversations with full context, cost <$5/month

---

### **Phase 3: Capabilities (Weeks 8-12)**

| Phase | Title                          | Description                                                                       | Depends On | Evolution Stage |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|-----------------|
| 17    | Gilgamesh Web Chat             | Embedded chat UI on homepage, shared memory with Telegram                         | 16         | Enhancement     |
| 18    | Gilgamesh Proactive Alerts     | Alertmanager → n8n → Telegram for critical infrastructure events                  | —          | Enhancement     |
| 19    | Gilgamesh Daily Summary        | Scheduled morning summary — containers, metrics, alerts, backup status            | 18         | Enhancement     |
| **7F** | **File Generation (Artifacts)** | **Generate code, docs, diagrams via n8n, downloadable from Telegram** | 41 | **Capabilities** |
| **7G** | **Vision API Integration** | **Image analysis via Telegram upload, screenshot troubleshooting** | 41 | **Capabilities** |
| **7H** | **Document Upload Handling** | **PDF/doc context via Telegram attachment, full document analysis** | 41 | **Capabilities** |

**Deliverable:** Feature parity with Claude Web (artifacts, vision, file uploads)

---

### **Phase 4: Refinement (Weeks 13-16)**

| Phase | Title                          | Description                                                                       | Depends On | Evolution Stage |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|-----------------|
| 20    | n8n Notification Hub           | Migrate Alertmanager alerts through n8n, route by severity to Telegram + Discord  | 18         | Enhancement     |
| 21    | n8n Workflow Suite             | Game server notifications, backup notifications, weekly report                    | 20         | Enhancement     |
| **7I** | **Quality Assurance & Testing** | **Side-by-side comparison with Claude Web, response quality validation** | 7F, 7G, 7H | **Refinement** |
| **7J** | **Claude Web Migration** | **Full switchover to Gilgamesh, cancel Claude Pro subscription** | 7I | **🎯 COMPLETION** |

**Deliverable:** Confidently cancel Claude Pro, save $20/month, Gilgamesh as sole AI interface

---

### Near Term — Personal & Knowledge *(Post-Evolution)*

| Phase | Title                          | Description                                                                       | Depends On |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|
| 24    | Network Cleanup                | Switch IP migration (192.168.1.20 → 192.168.10.20), remove legacy LAN 192.168.1.0/24 | 7J      |
| 25    | WiFi Access Point              | TP-Link EAP610 deployment, hardware already on hand                               | 24         |
| 26    | Homepage Security              | Cloudflare Access protection for home.najhin-gaming.com                           | —          |
| 27    | Vault Integration with n8n     | Gilgamesh and n8n workflows fetch API secrets from Vault dynamically              | 13         |

---

### Medium Term — Core Services Expansion

| Phase | Title                          | Description                                                                       | Depends On |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|
| 28    | Authentik SSO                  | Single sign-on for all services, replaces per-service logins, MFA support         | —          |
| 29    | Gitea + Woodpecker CI          | Self-hosted Git server + CI/CD pipeline, career portfolio piece                   | —          |
| 30    | Ansible + Semaphore UI         | Infrastructure automation for all containers, career portfolio piece              | —          |
| 31    | Paperless-ngx                  | Document management with OCR, personal documents and receipts                     | —          |
| 32    | Immich                         | Self-hosted Google Photos alternative, ~1.5GB RAM                                 | —          |
| 33    | Jellyfin                       | Self-hosted media server (deploy if media collection warrants it)                 | —          |
| 34    | FreshRSS                       | Self-hosted RSS reader, follow blogs/news without algorithm                       | —          |
| 35    | Memos                          | Lightweight quick-capture notes, pairs with Obsidian                              | 22         |
| 36    | Off-site Backup                | Backblaze B2 for critical container backups                                       | —          |
| 37    | Nextcloud 2FA                  | TOTP two-factor authentication on Nextcloud                                       | —          |
| 39    | Open WebUI                     | Web frontend for Ollama (optional — may be redundant with Gilgamesh)              | 38         |
| 40    | Karakeep                       | AI-powered bookmarking with local Ollama integration                              | 38         |

---

### Medium Term — Career Development

| Phase | Title                          | Description                                                                       | Depends On |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|
| 42    | k3s Kubernetes Cluster         | 2 LXCs on Kuromoon (1 master + 1 worker), industry-standard Kubernetes learning   | —          |
| 43    | Coolify                        | Self-hosted PaaS (Heroku/Vercel alternative), one-click app deployment            | —          |
| 44    | Traefik                        | Code-driven reverse proxy, migrate from NPM when running k3s workloads            | 42         |
| 45    | Headscale                      | Self-hosted Tailscale control server, removes last cloud VPN dependency           | —          |
| 46    | Netbox                         | Network documentation and IP address management (IPAM)                            | —          |
| 47    | SigNoz                         | Advanced observability — logs + traces + metrics, Datadog alternative             | 42         |
| 48    | Postiz                         | Self-hosted social media scheduler, automate LinkedIn phase announcements          | —          |

---

### Long Term — Testing & Advanced

| Phase | Title                          | Description                                                                       | Depends On |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|
| 49    | Windows 11 VM                  | Proxmox VM, 4 cores / 8GB RAM / 64GB disk, testing only                           | —          |
| 50    | macOS VM                       | OpenCore on Proxmox, legally grey, testing only — expect complexity               | —          |
| 51    | Portainer                      | Docker management GUI, useful if Docker workloads grow significantly              | —          |
| 52    | Wazuh SIEM                     | Security monitoring across all containers, ~4GB RAM minimum                       | —          |
| 53    | Docmost                        | Team-facing wiki / Notion alternative for shared documentation                    | —          |
| 54    | BookLore                       | Personal ebook library with OPDS e-reader support                                 | —          |

---

### Gaming Platform Pipeline *(Parallel Track)*

| Phase | Title                          | Description                                                                       | Depends On |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|
| 57    | Gaming Platform Deployment Guide | Standardized deployment docs for all game servers                              | —          |
| 58    | Windrose Server                | ✅ Complete — Docker deployment on CT 302                                         | —          |
| 59    | Discord Gaming Bot (Mash)      | !start/!stop/!status commands for friends                                         | —          |
| 60    | Auto-shutdown Idle Servers     | Detect idle state, graceful shutdown                                              | 59         |
| 60.1  | Backup Before Shutdown         | Automated backup trigger before server stops                                      | 60         |
| 61    | Discord Status Embed           | Rich embed with player counts, uptime, server health                              | 59         |
| 62    | Scheduled Game Nights          | Calendar integration, auto-start servers, reminders                               | 59         |
| 63    | Game Update Notifications      | Detect patch releases, notify Discord                                             | 59         |
| 64    | Gaming Grafana Dashboard       | Metrics visualization for all game servers                                        | —          |

---

### Hardware

| Phase | Title                          | Description                                                                       | Depends On |
|-------|--------------------------------|-----------------------------------------------------------------------------------|------------|
| 55    | P400S Repurpose                | Z270E + i7-7700K build, potential SAS NAS or secondary hypervisor                 | —          |
| 56    | Fedora Dual-boot on Minimoon   | Requires kernel 6.12+ for RDNA 4 (RX 9070 XT)                                    | —          |

---

## 📊 Summary

| Status | Count |
|--------|-------|
| ✅ Complete | 20 |
| 🔄 In Progress | 1 |
| 📋 Planned | 53 |
| **Total LXC Containers** | **14** |

---

## 🎯 Recommended Execution Order

| Priority | Phase | Title | Why | Weeks |
|----------|-------|-------|-----|-------|
| 1 | 7D-Menu → 14 | Complete Gilgamesh Menu | Already in progress | 1-2 |
| 2 | **38** | **Ollama + ROCm** | **🔥 CRITICAL: Stop API cost bleeding** | 2-3 |
| 3 | **22** | **Obsidian + RAG** | **🔥 Foundation for intelligence** | 3-4 |
| 4 | 15 | Additional Commands | Quality of life improvements | 4-5 |
| 5 | **7E** | **Extended Memory** | **Enable long conversations** | 5-6 |
| 6 | **41** | **Hybrid Routing** | **Cost optimization, <$5/month** | 6-7 |
| 7 | 16 | /update Redesign | Better workflow | 7-8 |
| 8 | 18-19 | Proactive Alerts + Daily Summary | Homelab intelligence | 8-10 |
| 9 | **7F** | **File Generation** | **Artifacts capability** | 10-12 |
| 10 | **7G** | **Vision API** | **Image analysis** | 12-13 |
| 11 | **7H** | **Document Upload** | **Full context** | 13-14 |
| 12 | **7I** | **Quality Assurance** | **Testing vs Claude Web** | 14-15 |
| 13 | **7J** | **Migration** | **🎯 Cancel Claude Pro** | 15-16 |

---

## 💰 Cost Projection

| Timeline | Claude Pro | Claude API | Ollama | Total |
|----------|-----------|------------|--------|-------|
| **Today** | $20 | $10-20 | $0 | **$30-40/month** |
| **After Phase 38** (Week 3) | $20 | $2-5 | $0 | **$22-25/month** |
| **After Phase 41** (Week 7) | $20 | $1-3 | $0 | **$21-23/month** |
| **After Phase 7J** (Week 16) | $0 | $5-10 | $0 | **$5-10/month** |

**Annual Savings:** $240-360/year

---

## 🎴 Fate Grand Order Agent Ecosystem

**Servant naming theme for all AI agents (April 21-22, 2026)**

### Active Servants

| Servant | Class | Role | Platform | Status |
|---------|-------|------|----------|--------|
| **Gilgamesh** 👑 | Archer | Personal AI Assistant | Telegram | ✅ Active |
| **Da Vinci** 🎨 | Caster | Knowledge Curator | n8n/Nextcloud | ✅ Active |
| **EMIYA** 🏹 | Archer | Infrastructure Translator | n8n | ✅ Active |

### Planned Servants (Priority Order)

**Phase 2: Safety Net**
- **MERLIN** 🔮 (Caster) — #1 Priority: Reminders & Scheduler (SSL certs, backups, maintenance windows)
- **Mash Kyrielight** 🛡️ (Shielder) — Discord Gaming Bot (Phases 59-64)
- **Guardian** — Security Monitoring

**Phase 3: Efficiency**
- **Nexus** — Cross-platform Automation
- **Scribe** — Auto-documentation
- **Midas** — Cost Tracking & Optimization

**Phase 4: Intelligence**
- **Oracle** — Predictive Intelligence

**Design Principle:** Gaming agents (Mash) stay on Discord, homelab admin (Gilgamesh) stays on Telegram — keep them separate.

---

*Last updated: April 23, 2026*
