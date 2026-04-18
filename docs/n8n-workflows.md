# ⚙️ n8n Workflow Catalog

> **Last Updated:** April 18, 2026
> **n8n Instance:** CT 211, n8n.najhin-gaming.com
> **Purpose:** Master list of all n8n workflows — built, in progress, and planned

---

## Overview

n8n is the central automation hub for the homelab. It serves multiple roles:

- **Gilgamesh AI Agent** — Telegram bot powered by Claude API
- **Notification Hub** — Routes alerts from Alertmanager to Telegram and Discord
- **Infrastructure Automation** — Scheduled tasks, backups, reports
- **Game Server Management** — Game server status and notifications
- **Document Sync** — GitHub and Nextcloud file updates

---

## ✅ Built & Working

### 1. Gilgamesh — Main Conversation Workflow
**Trigger:** Telegram message or callback query
**Flow:** Telegram Trigger → Get Memory → Format Messages → Route Model → Claude API → Extract Response → Save Memory → Save Cost → IF → Send Telegram / Parse File → GitHub + Nextcloud
**Features:**
- Conversation memory (last 20 messages, n8n Data Tables)
- Smart routing: Haiku 4.5 (simple) / Sonnet 4 (complex)
- Web search via Claude web_search tool
- Cost tracking (gilgamesh_costs table)
- Inline keyboard menu (Main, Homelab → Status working)
- /update command → append to AI-CONTEXT.md → push to Nextcloud + GitHub

**Known constraints:**
- Telegram single webhook — handles both messages and callbacks in one trigger
- Plain text only — no code blocks or backticks in messages (breaks JSON)
- Answer Callback node bypassed (strips callback_query data)

---

## 🔄 In Progress

### 2. Gilgamesh — Inline Keyboard Menu (Phase 7D-Menu)
**Trigger:** Telegram callback query (button press)
**Flow:** Callback Router → Proxmox API / Grafana API / etc. → Edit Message
**Status:**
| Submenu | Action | Status |
|---------|--------|--------|
| Homelab | Status (Proxmox API) | ✅ Working |
| Homelab | Metrics | 📋 Pending |
| Homelab | Temps | 📋 Pending |
| Homelab | Storage | 📋 Pending |
| Gaming | Server Status | 📋 Pending |
| Gaming | Players Online | 📋 Pending |
| Gilgamesh | Cost Summary | 📋 Pending |
| Gilgamesh | Memory Status | 📋 Pending |
| Gilgamesh | Clear Memory | 📋 Pending |
| Tools | Speedtest | 📋 Pending |
| Tools | Test Alert | 📋 Pending |
| Help | Help text | 📋 Pending |

---

## 📋 Planned Workflows

### Gilgamesh Commands (Phase 7D-Cmds)

| Command | Purpose | API / Source |
|---------|---------|--------------|
| /temps | CPU, GPU, NVMe temperatures | Proxmox API |
| /storage | Disk usage across all pools | Proxmox API |
| /alerts | Recent Alertmanager alerts | Alertmanager API |
| /logs [service] | Recent logs from container | Loki API |
| /backup | Last backup job status | Proxmox API |
| /speedtest | Network speed test | speedtest-cli |
| /cost | Claude API spend this month | n8n Data Table |
| /memory | Show conversation memory size | n8n Data Table |
| /clear | Clear conversation memory | n8n Data Table |
| /help | List all commands | Static text |

---

### Gilgamesh /update Redesign (Phase 7D-Update)
**Trigger:** Telegram document message (`.md` file attachment)
**New Flow:** Telegram Trigger → Detect document type → Download file via Telegram getFile API → Push to GitHub (replace file) → Write to Nextcloud WebDAV → Confirm to user
**Replaces:** Current text-append approach
**Why:** Eliminates format mixing, size limits, and session log pollution
**Note:** Claude Project upload remains manual (no API available)

---

### Gilgamesh Web Chat (Phase 7D-WebChat)
**Trigger:** HTTP webhook from homepage chat UI
**Flow:** Webhook → Get Memory (same chat ID as Telegram) → Claude API → Return response to UI
**Key design:** Uses same Telegram chat ID (510832696) as user identifier — shared memory across Telegram and web chat

---

### Gilgamesh Proactive Alerts (Phase 7D-Proactive)
**Trigger:** Alertmanager webhook
**Flow:** Alertmanager → n8n webhook → Parse alert → Format message → Route by severity → Telegram (critical) / Discord (warning)
**Note:** This is part of the broader n8n notification hub migration below

---

### Gilgamesh Daily Summary (Phase 7D-Daily)
**Trigger:** Schedule — daily 08:00
**Flow:** Schedule → Proxmox API (container states) → Prometheus API (metrics) → Uptime Kuma API (service health) → Format summary → Send Telegram
**Content:**
- All container states
- CPU / RAM / Disk usage summary
- Any active alerts
- Last backup status

---

### n8n Central Notification Hub (Phase 7E)
**Goal:** All alerts route through n8n instead of Alertmanager sending directly to Telegram/Discord
**Current:** Alertmanager → Telegram + Discord (direct)
**New:** Alertmanager → n8n webhook → route by severity → Telegram + Discord

**Routing logic:**
| Severity | Destination |
|----------|-------------|
| Critical | Telegram + Discord |
| Warning | Discord only |
| Info | Discord only (batched) |

**Future:** Forward critical alerts to Gilgamesh with AI context (e.g. "CT 203 Grafana is down. Last backup was 2 hours ago.")

---

### Game Server Notifications (Phase 7F)
**Trigger:** Pterodactyl webhook (server start / stop / crash)
**Flow:** Pterodactyl webhook → n8n → Format message → Discord #game-servers channel
**Notifications:**
| Event | Message |
|-------|---------|
| Server online | "✅ Terraria is now online — join at terraria.najhin-gaming.com:7777" |
| Server offline | "🔴 Terraria has gone offline" |
| Server crash | "⚠️ Minecraft crashed — check Pterodactyl panel" |
| Player join | "👤 [player] joined Minecraft (2/20 players)" |
| Player leave | "👤 [player] left Minecraft (1/20 players)" |

---

### Daily Backup Notification (Phase 7F)
**Trigger:** Schedule — daily 03:30 (after backup jobs complete)
**Flow:** Schedule → Proxmox API (backup job results) → Format summary → Send Telegram
**Content:**
- Which containers were backed up
- Success / failure status
- Backup size
- Storage usage remaining

---

### Weekly Infrastructure Report (Phase 7F)
**Trigger:** Schedule — Monday 09:00
**Flow:** Schedule → Prometheus API → Proxmox API → GitHub API → Format report → Send Telegram + Discord
**Content:**
- Week's uptime across all services
- Resource usage trends
- New phases/changes this week
- GitHub commit count
- Top alerts fired

---

### Postiz — LinkedIn Post Scheduler (Phase 32)
**Trigger:** n8n schedule or manual webhook
**Flow:** n8n → Postiz API → Schedule LinkedIn post
**Use case:** Automate posting homelab phase announcements to LinkedIn portfolio

---

## 🔧 Technical Reference

### Credentials in Use
| Credential | Used By | Stored In |
|------------|---------|-----------|
| Claude API key | Gilgamesh HTTP Request | Vault kv/gilgamesh |
| Telegram bot token | All Telegram nodes | Vault kv/gilgamesh |
| GitHub PAT | GitHub push nodes | Vault kv/gilgamesh |
| Proxmox API token | Status/Metrics nodes | Vault kv/proxmox |
| Cloudflare API key | DDNS workflow | Vault kv/cloudflare |

### Key n8n Data Tables
| Table | Purpose | Retention |
|-------|---------|-----------|
| gilgamesh_conversations | Conversation memory | Last 20 messages |
| gilgamesh_costs | Claude API token usage | All time |

### Proxmox API Details
- **URL:** `https://192.168.x.x:8006/api2/json/nodes/muzakkir/lxc`
- **Auth:** Header — `Authorization: PVEAPIToken=root@pam!gilgamesh=<token>`
- **Note:** Enable "Ignore SSL Issues" for self-signed cert
- **Node name:** `muzakkir` (not kuromoon)

### Telegram Webhook Constraints
- Only ONE webhook per bot — all update types in single trigger
- Handle both `message` and `callback_query` in same workflow
- Conditional chat ID: `{{ $json.callback_query ? $json.callback_query.message.chat.id : $json.message.chat.id }}`
- Multi-line text: `{{ "Line1\n\nLine2" }}` expression format

---

*Last updated: April 18, 2026*
