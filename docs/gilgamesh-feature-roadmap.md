# 🤖 Gilgamesh Feature Roadmap

> **Last Updated:** April 14, 2026  
> **Current Version:** 1.0 (Phase 7D Complete)  
> **Platform:** n8n + Claude API + Telegram

---

## 📋 Overview

Gilgamesh is a custom AI-powered Telegram bot for homelab management and personal assistance. This document tracks planned features, enhancements, and the development roadmap.

---

## ✅ Current Features (v1.0)

| Feature | Status | Phase |
|---------|--------|-------|
| Basic conversation | ✅ Complete | 7C |
| Claude API integration | ✅ Complete | 7C |
| /update command (GitHub + Nextcloud sync) | ✅ Complete | 7C |
| Conversation memory (last 20 messages) | ✅ Complete | 7D |
| Smart routing (Haiku/Sonnet) | ✅ Complete | 7D |
| Web search capability | ✅ Complete | 7D |
| Cost/token tracking | ✅ Complete | 7D |
| Cloudflare Access protection | ✅ Complete | 7D-Sec |

---

## 🔄 In Progress

### Phase 7D-Menu: Inline Keyboard Menu System

**Status:** 🔄 In Progress  
**Estimated:** 3-4 hours remaining

Build a nested Telegram inline keyboard menu for quick homelab actions without typing commands.

#### Menu Structure

```
🏠 Main Menu
├── 🖥️ Homelab
│   ├── 📊 Status (container list)
│   ├── 📈 Metrics (CPU/RAM/Disk)
│   ├── 🌡️ Temps (hardware temps)
│   ├── 💾 Storage (disk usage)
│   └── ⬅️ Back
├── 🎮 Gaming
│   ├── 🟢 Server Status
│   ├── 👥 Players Online
│   └── ⬅️ Back
├── 🤖 Gilgamesh
│   ├── 💰 Cost Summary
│   ├── 🧠 Memory Status
│   ├── 🗑️ Clear Memory
│   └── ⬅️ Back
├── 🛠️ Tools
│   ├── 🌐 Speedtest
│   ├── 🔔 Test Alert
│   └── ⬅️ Back
└── ❓ Help
```

#### Current Implementation

| Component | Status |
|-----------|--------|
| Main Menu (send) | ✅ Done |
| Main Menu (edit) | ✅ Done |
| Homelab submenu | ✅ Done |
| Status action (Proxmox API) | ✅ Done |
| Gaming submenu | 📋 Pending |
| Gilgamesh submenu | 📋 Pending |
| Tools submenu | 📋 Pending |
| Help submenu | 📋 Pending |
| Metrics action | 📋 Pending |
| Temps action | 📋 Pending |
| Storage action | 📋 Pending |

#### Technical Notes

- Single webhook constraint: All interactions in one workflow
- Use Edit Message (not Send) for submenu navigation
- Answer Callback removed (strips callback data)
- Proxmox API: 192.168.10.5:8006, token root@pam!gilgamesh

---

## 📋 Planned Features

### Phase 7D-Cmds: Additional Commands

**Priority:** 🟡 Medium  
**Estimated:** 4-6 hours

| Command | Purpose | API Required |
|---------|---------|--------------|
| /temps | Show hardware temperatures | Proxmox API |
| /storage | Show disk usage | Proxmox API |
| /alerts | Show active alerts | Alertmanager API |
| /logs [service] | Recent logs | Loki API |
| /backup | Backup status | Proxmox API |
| /speedtest | Network speed test | speedtest-cli |
| /wake [device] | Wake-on-LAN | Local script |
| /cost | Token usage summary | n8n Data Table |
| /memory | Show memory status | n8n Data Table |
| /clear | Clear conversation memory | n8n Data Table |
| /help | List all commands | Static text |

---

### Phase 7D-Proactive: Proactive Alerts

**Priority:** 🟡 Medium  
**Estimated:** 3-4 hours

Gilgamesh messages YOU when issues arise (instead of waiting for you to ask).

#### Triggers

| Trigger | Threshold | Action |
|---------|-----------|--------|
| Container down | Any container stops | Immediate alert |
| High CPU | >90% for 5 minutes | Warning message |
| High Memory | >85% for 5 minutes | Warning message |
| Disk Space | >80% usage | Daily warning |
| Backup failure | Job fails | Immediate alert |
| SSL expiry | <14 days | Daily warning |

#### Implementation

```
Alertmanager → Webhook → n8n → Gilgamesh → Telegram
```

- Configure Alertmanager webhook receiver
- n8n workflow receives alert JSON
- Format friendly message
- Send via Gilgamesh Telegram

---

### Phase 7D-Daily: Daily Summary

**Priority:** 🟡 Medium  
**Estimated:** 2-3 hours

Automated morning summary message sent to Telegram.

#### Daily Summary Contents

```
☀️ Good morning! Here's your homelab status:

🖥️ Containers: 12/12 running
💾 Storage: 45% used (vmpool-fast), 23% (kinmoon-smb)  
📊 24h Stats: CPU avg 12%, RAM avg 45%
🎮 Game Servers: Terraria ✅, Minecraft ✅
🔔 Alerts: 0 active
💰 Yesterday's AI cost: $0.03 (142 requests)

Have a great day! 🚀
```

#### Schedule

- Time: 07:00 MYT (Malaysia Time)
- Trigger: n8n Cron node
- Skip weekends: Optional setting

---

### Phase 7E: Pain Point Scraper (AI Vision)

**Priority:** 🟢 High  
**Estimated:** 4-6 hours  
**Cost:** ~RM 2-5/month API

Use Claude Vision to extract business opportunities from social media complaints.

#### Workflow

```
📱 You screenshot a complaint (Reddit/FB/X)
         │
         ▼
📤 Send to Gilgamesh via Telegram
         │
         ▼
🤖 Claude Vision analyzes image:
   - Extract text
   - Identify pain point
   - Categorize (tech/service/product)
   - Suggest business angle
         │
         ▼
💾 Store in n8n Data Table:
   - Source platform
   - Pain point summary
   - Category tags
   - Business opportunity
   - Timestamp
         │
         ▼
📊 Weekly summary of top pain points
```

#### Data Table: `pain_points`

| Column | Type | Purpose |
|--------|------|---------|
| id | Auto | Primary key |
| source | String | Platform (Reddit, FB, X) |
| screenshot_url | String | Telegram file URL |
| extracted_text | String | OCR'd text |
| pain_point | String | Core complaint |
| category | String | Tech, Service, Product |
| business_angle | String | Opportunity description |
| sentiment | String | Angry, Frustrated, Confused |
| timestamp | DateTime | When captured |

---

### Phase 7L: Ollama Hybrid Routing

**Priority:** 🟡 Medium  
**Estimated:** 1 day  
**Dependency:** Phase 11 (Ollama deployment)

Route queries between local Ollama and Claude API for cost optimization.

#### Routing Logic

```
┌─────────────────┐
│  User Message   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Complexity     │
│  Assessment     │
└────────┬────────┘
         │
    ┌────┴────┬────────────┐
    │         │            │
    ▼         ▼            ▼
 Simple    Medium      Complex
    │         │            │
    ▼         ▼            ▼
┌────────┐ ┌────────┐ ┌────────┐
│ Ollama │ │ Haiku  │ │ Sonnet │
│ (free) │ │ (cheap)│ │ (best) │
│ 7B     │ │        │ │        │
└────────┘ └────────┘ └────────┘
```

#### Model Allocation

| Complexity | Examples | Model | Cost |
|------------|----------|-------|------|
| Simple | Greetings, time, basic Q&A | Ollama 7B | Free |
| Medium | Summaries, explanations | Haiku | ~$0.001 |
| Complex | Technical, analysis, code | Sonnet | ~$0.01 |

---

### Phase 7H: Obsidian + claude-obsidian (Second Brain)

**Priority:** 🟢 High  
**Estimated:** 1 day  
**Dependency:** None

Full "second brain" setup using Obsidian with claude-obsidian plugin.

#### What claude-obsidian Does

| Feature | Description |
|---------|-------------|
| `/wiki` | Initialize vault structure (concepts, sources, entities, sessions) |
| `/save` | After conversation, extract key ideas → wiki pages with wikilinks |
| `/autoresearch` | 3-5 rounds of web research → structured wiki with citations |
| `/canvas` | Create visual knowledge graphs from vault content |
| `ingest` | Drop PDF/URL/transcript → 8-15 interconnected wiki pages |

#### Vault Structure

```
obsidian-vault/
├── wiki/
│   ├── concepts/       (frameworks, patterns, ideas)
│   ├── sources/        (ingested documents)
│   ├── entities/       (people, tools, organizations)
│   ├── sessions/       (saved conversations)
│   ├── log.md          (operation history)
│   ├── index.md        (one-line summary per page)
│   └── hot.md          (session context cache, ~500 words)
├── gilgamesh/
│   ├── sessions/       (Telegram session summaries)
│   └── costs/          (monthly cost tracking)
├── homelab/
│   └── AI-CONTEXT.md   (master context document)
└── .raw/               (original source files, never modified)
```

#### Why This Replaces RAG (Phase 7M)

| Traditional RAG | claude-obsidian |
|-----------------|-----------------|
| Embeddings + vector DB | Index.md + hot.md (text-based) |
| Complex infrastructure | Just files in a folder |
| Query → retrieve → generate | Query → scan index → load relevant pages |
| Separate from notes | IS your notes |

#### Setup Steps

1. Install Obsidian on desktop/mobile
2. Create vault in Nextcloud (cloud.najhin-gaming.com)
3. Install Claude Code CLI
4. Install claude-obsidian plugin
5. Run `/wiki` to initialize structure
6. Migrate AI-CONTEXT.md to vault
7. Test `/save`, `/autoresearch`, `/canvas`

---

### Phase 7H-Gil: Gilgamesh → Obsidian Integration

**Priority:** 🟢 High  
**Estimated:** 3-4 hours  
**Dependency:** Phase 7H

Connect Gilgamesh (Telegram/n8n) to write session summaries directly to Obsidian vault.

#### Architecture

```
Telegram (Gilgamesh)
         │
         ▼
┌─────────────────┐
│      n8n        │
│   (CT 211)      │
└────────┬────────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
┌────────┐ ┌────────────────┐
│n8n Data│ │ Nextcloud      │
│Tables  │ │ WebDAV         │
│(live)  │ │ (persistent)   │
└────────┘ └────────┬───────┘
                    │
                    ▼
           ┌────────────────┐
           │ Obsidian Vault │
           │ gilgamesh/     │
           │ sessions/      │
           └────────────────┘
                    │
                    ▼
           claude-obsidian
           indexes & links
```

#### Data Split (Hybrid Approach)

| Data Type | Storage | Why |
|-----------|---------|-----|
| Last 20 messages | n8n Data Tables | Fast retrieval for live context |
| Cost tracking | n8n Data Tables | Structured queries |
| Session summaries | Obsidian | Searchable, compounding knowledge |
| Decisions & learnings | Obsidian | Wiki links, cross-references |
| AI-CONTEXT.md | Obsidian | Single source of truth |

#### n8n → Obsidian via WebDAV

```javascript
// Save session summary to Obsidian via Nextcloud
const date = new Date().toISOString().split('T')[0];
const filename = `gilgamesh/sessions/${date}-session.md`;

const content = `---
date: ${date}
tokens: ${inputTokens + outputTokens}
model: ${model}
tags: [gilgamesh, session]
---

## Session Summary

${summary}

## Key Decisions

${decisions}

## Links

- [[AI-CONTEXT]]
- [[homelab-infrastructure]]
`;

await this.helpers.httpRequest({
  method: 'PUT',
  url: `https://cloud.najhin-gaming.com/remote.php/dav/files/user/Obsidian/${filename}`,
  body: content,
  headers: {
    'Authorization': 'Basic ...',
    'Content-Type': 'text/markdown'
  }
});
```

---

### Phase 7N-Gil: Image Analysis

**Priority:** 🟡 Medium  
**Estimated:** 2-3 hours

Send screenshots to Gilgamesh for analysis.

#### Use Cases

- Error screenshot → Explanation + fix
- Dashboard screenshot → Summary
- Config screenshot → Validation
- Network diagram → Analysis

#### Implementation

```javascript
// Check if message contains photo
if ($json.message.photo) {
  const photoId = $json.message.photo.slice(-1)[0].file_id;
  // Download via Telegram API
  // Convert to base64
  // Send to Claude Vision API
}
```

---

## 🔮 Future Ideas (Backlog)

| Feature | Description | Complexity |
|---------|-------------|------------|
| Voice commands | Whisper STT → Gilgamesh → Piper TTS | High |
| Multi-user support | Different chat IDs, different permissions | Medium |
| Scheduled reports | Weekly/monthly infrastructure reports | Low |
| Integration with Home Assistant | Smart home control | Medium |
| Expense tracking | Track homelab costs over time | Low |
| Learning mode | Gilgamesh learns from corrections | High |
| Workflow suggestions | "You often do X, want me to automate?" | High |

---

## 📊 Cost Projections

### Current (v1.0)

| Component | Monthly Cost |
|-----------|--------------|
| Claude Haiku | ~$1-2 |
| Claude Sonnet | ~$3-5 |
| **Total** | **~$4-7** |

### With Vision (v1.1)

| Component | Monthly Cost |
|-----------|--------------|
| Claude Haiku | ~$1-2 |
| Claude Sonnet | ~$3-5 |
| Claude Vision | ~$2-5 |
| **Total** | **~$6-12** |

### With Ollama Hybrid (v2.0)

| Component | Monthly Cost |
|-----------|--------------|
| Ollama (local) | $0 |
| Claude Haiku (reduced) | ~$0.50 |
| Claude Sonnet (complex only) | ~$2-3 |
| **Total** | **~$2-4** |

---

## 🛠️ Technical Stack

| Component | Technology |
|-----------|------------|
| **Platform** | n8n (CT 211) |
| **Messaging** | Telegram Bot API |
| **AI - Cloud** | Claude API (Haiku, Sonnet) |
| **AI - Local** | Ollama (planned) |
| **Memory** | n8n Data Tables |
| **Vector DB** | ChromaDB (planned) |
| **Monitoring** | Proxmox API, Prometheus API |
| **Alerts** | Alertmanager webhooks |

---

## 📝 Development Notes

### Key Learnings

1. **Single webhook per bot** — All Telegram interactions must route through one workflow
2. **Plain text only** — Markdown code blocks break JSON in Telegram messages
3. **Edit vs Send** — Use Edit Message for menu navigation to update in-place
4. **Multi-block responses** — Filter Claude responses by `type === "text"`
5. **n8n schema caching** — Delete and recreate Data Tables if columns change

### API Credentials Required

| Service | Credential Type | Location |
|---------|-----------------|----------|
| Telegram | Bot Token | n8n Credentials |
| Claude | API Key | n8n Credentials |
| Proxmox | API Token | n8n Credentials |
| GitHub | Personal Access Token | n8n Credentials |
| Nextcloud | App Password | n8n Credentials |

### Useful n8n Patterns

```javascript
// Multi-line text in Telegram
{{ "Line 1\n\nLine 2" }}

// Filter Claude text blocks
const text = $json.content
  .filter(b => b.type === 'text')
  .map(b => b.text)
  .join('\n\n');

// Access upstream node data
const chatId = $('Telegram Trigger').first().json.message.chat.id;
```

---

## 📅 Development Timeline

| Phase | Feature | Target | Status |
|-------|---------|--------|--------|
| 7D-Menu | Inline keyboard menus | April 2026 | 🔄 In Progress |
| 13 | HashiCorp Vault (API keys) | April 2026 | 📋 Next |
| 7H | Obsidian + claude-obsidian | April 2026 | 📋 Planned |
| 7H-Gil | Gilgamesh → Obsidian | April 2026 | 📋 Planned |
| 7D-Cmds | Additional commands | May 2026 | 📋 Planned |
| 7D-Proactive | Proactive alerts | May 2026 | 📋 Planned |
| 7D-Daily | Daily summary | May 2026 | 📋 Planned |
| 7E | Pain point scraper | May 2026 | 📋 Planned |
| 11 | Ollama deployment | June 2026 | 📋 Planned |
| 7L | Ollama hybrid routing | June 2026 | 📋 Planned |

---

## 🔗 Related Documents

- [Phase 7B: n8n Automation](phase-7b-n8n-automation.md)
- [Phase 7C: Gilgamesh Bot](phase-7c-gilgamesh-bot.md)
- [Phase 7D: Gilgamesh Enhancements](phase-7d-gilgamesh-enhancements.md)
- [Phase 7D-Sec: Cloudflare Access](phase-7d-sec-cloudflare-access.md)
- [Main Roadmap](../ROADMAP.md)

---

*Last Updated: April 14, 2026*
