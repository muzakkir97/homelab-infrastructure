# Phase 7C: Gilgamesh Telegram Bot

> **Status:** ✅ Complete  
> **Completed:** April 2, 2026  
> **Duration:** ~6 hours

---

## 📋 Overview

Creation of Gilgamesh, an AI-powered Telegram bot that serves as a personal assistant for homelab management, powered by Claude API and orchestrated through n8n.

---

## 🎯 Objectives

- [x] Create Telegram bot via BotFather
- [x] Build n8n workflow for message handling
- [x] Integrate Claude API for AI responses
- [x] Implement /update command for context sync
- [x] Connect to GitHub and Nextcloud for storage

---

## 🤖 Bot Details

| Property | Value |
|----------|-------|
| **Bot Name** | Gilgamesh |
| **Username** | @JhinGilgamesh_bot |
| **Platform** | Telegram |
| **Backend** | n8n (CT 211) |
| **AI Model** | Claude (Anthropic API) |
| **Chat ID** | 510832696 |

---

## 🏗️ Architecture

```
┌─────────────────────┐
│  Telegram User      │
│  (Muzakkir)         │
└──────────┬──────────┘
           │ Message
           ▼
┌─────────────────────┐
│  Telegram API       │
│  @JhinGilgamesh_bot │
└──────────┬──────────┘
           │ Webhook
           ▼
┌─────────────────────┐
│  n8n Workflow       │
│  (CT 211)           │
│                     │
│  ┌───────────────┐  │
│  │ Telegram      │  │
│  │ Trigger       │  │
│  └───────┬───────┘  │
│          │          │
│  ┌───────▼───────┐  │
│  │ Route by      │  │
│  │ Command       │  │
│  └───────┬───────┘  │
│          │          │
│    ┌─────┴─────┐    │
│    │           │    │
│    ▼           ▼    │
│ /update     Regular │
│ Command     Message │
│    │           │    │
│    │     ┌─────▼───┐│
│    │     │Claude   ││
│    │     │API Call ││
│    │     └─────┬───┘│
│    │           │    │
│    ▼           ▼    │
│ GitHub    Telegram  │
│ + Cloud   Response  │
└─────────────────────┘
```

---

## 🔧 Implementation

### Step 1: Create Telegram Bot

1. Open Telegram, search for @BotFather
2. Send `/newbot`
3. Name: `Gilgamesh`
4. Username: `JhinGilgamesh_bot`
5. Save the API token

### Step 2: n8n Workflow Structure

#### Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| Telegram Trigger | Trigger | Receive messages |
| Switch | Router | Route by command/callback |
| HTTP Request | Action | Call Claude API |
| Send Message | Action | Reply to user |
| GitHub Push | Action | Update AI-CONTEXT.md |
| Nextcloud Upload | Action | Backup context |

#### Telegram Trigger Configuration

```json
{
  "updates": ["message", "callback_query"],
  "additionalFields": {}
}
```

#### Claude API Request

```json
{
  "method": "POST",
  "url": "https://api.anthropic.com/v1/messages",
  "headers": {
    "x-api-key": "{{ $credentials.anthropicApi.apiKey }}",
    "anthropic-version": "2023-06-01",
    "content-type": "application/json"
  },
  "body": {
    "model": "claude-sonnet-4-20250514",
    "max_tokens": 4096,
    "system": "You are Gilgamesh, an AI assistant...",
    "messages": [
      {
        "role": "user",
        "content": "{{ $json.message.text }}"
      }
    ]
  }
}
```

### Step 3: /update Command

The `/update` command syncs session context to both GitHub and Nextcloud:

1. User sends `/update <session summary>`
2. n8n extracts summary text
3. Appends to AI-CONTEXT.md
4. Pushes to GitHub (via API)
5. Uploads to Nextcloud (via WebDAV)
6. Confirms to user

#### GitHub Push Logic

```javascript
// Prepare GitHub Push node
const content = $json.contextContent;
const encoded = Buffer.from(content).toString('base64');

return {
  contentBase64: encoded,
  sha: $json.existingSha // from previous GET request
};
```

---

## 📝 System Prompt

```
You are Gilgamesh, an AI assistant for Muzakkir's homelab infrastructure project. 

Your role:
- Help with homelab questions and troubleshooting
- Provide technical guidance on Proxmox, pfSense, networking, containers
- Assist with documentation and planning
- Track session context for /update command

Current infrastructure:
- Proxmox server (Kuromoon) with 12 LXC containers
- pfSense firewall with 5 VLANs
- Monitoring stack (Prometheus, Grafana, Loki)
- n8n for automation
- Nextcloud for file storage
- Game servers (Terraria, Minecraft)

Respond concisely. Use plain text (no markdown code blocks in Telegram).
```

---

## ✅ Verification

| Check | Status |
|-------|--------|
| Bot responds to messages | ✅ |
| Claude API integration working | ✅ |
| /update command syncs to GitHub | ✅ |
| /update command syncs to Nextcloud | ✅ |
| Response formatting correct | ✅ |

---

## 🐛 Issues Encountered

### Issue 1: Telegram Message Formatting

**Problem:** Markdown code blocks break in Telegram  
**Solution:** Use plain text formatting, avoid backticks in responses

### Issue 2: GitHub Push SHA Requirement

**Problem:** GitHub API requires existing file SHA for updates  
**Solution:** First GET the file to retrieve SHA, then PUT with SHA

### Issue 3: JSON Parsing in HTTP Request

**Problem:** Session summaries with special characters break JSON  
**Solution:** Use plain text without code blocks in /update summaries

---

## 📝 Lessons Learned

1. **Single webhook per bot** — All message types must be handled in one workflow
2. **Plain text for Telegram** — Avoid markdown code blocks
3. **GitHub needs SHA** — Fetch existing file before updating
4. **n8n expressions** — Use `{{ }}` syntax for dynamic values

---

## 🔗 Related

- **Prerequisite:** [Phase 7B: n8n Workflow Automation](phase-7b-n8n-automation.md)
- **Next Phase:** [Phase 7D: Gilgamesh Enhancements](phase-7d-gilgamesh-enhancements.md)

---

*Completed: April 2, 2026*
