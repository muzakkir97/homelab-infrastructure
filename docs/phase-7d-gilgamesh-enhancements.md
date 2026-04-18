# Phase 7D: Gilgamesh Enhancements

> **Status:** ✅ Complete  
> **Completed:** April 6, 2026  
> **Duration:** ~8 hours (across multiple sessions)

---

## 📋 Overview

Major enhancements to the Gilgamesh AI agent including conversation memory, smart model routing, web search capability, and cost tracking.

---

## 🎯 Objectives

- [x] Implement conversation memory (last 20 messages)
- [x] Add smart routing (Haiku for simple, Sonnet for complex)
- [x] Enable web search capability
- [x] Add cost/token tracking
- [x] Fix multi-block response handling

---

## 🏗️ Enhanced Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Gilgamesh Workflow                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────┐                                           │
│  │   Telegram   │                                           │
│  │   Trigger    │                                           │
│  └──────┬───────┘                                           │
│         │                                                   │
│         ▼                                                   │
│  ┌──────────────┐    ┌──────────────┐                       │
│  │ Load Memory  │───▶│ Build        │                       │
│  │ (Last 20)    │    │ Messages     │                       │
│  └──────────────┘    │ Array        │                       │
│                      └──────┬───────┘                       │
│                             │                               │
│                             ▼                               │
│                      ┌──────────────┐                       │
│                      │ Smart Router │                       │
│                      │ (Complexity) │                       │
│                      └──────┬───────┘                       │
│                             │                               │
│              ┌──────────────┴──────────────┐                │
│              │                             │                │
│              ▼                             ▼                │
│       ┌────────────┐               ┌────────────┐           │
│       │   Haiku    │               │   Sonnet   │           │
│       │  (Simple)  │               │ (Complex)  │           │
│       │ greetings  │               │ technical  │           │
│       │ short Q&A  │               │ analysis   │           │
│       └─────┬──────┘               └─────┬──────┘           │
│             │                            │                  │
│             └────────────┬───────────────┘                  │
│                          │                                  │
│                          ▼                                  │
│                   ┌──────────────┐                          │
│                   │ Web Search   │ (if needed)              │
│                   │ Tool         │                          │
│                   └──────┬───────┘                          │
│                          │                                  │
│                          ▼                                  │
│                   ┌──────────────┐                          │
│                   │ Extract      │                          │
│                   │ Response     │                          │
│                   │ (Multi-block)│                          │
│                   └──────┬───────┘                          │
│                          │                                  │
│         ┌────────────────┼────────────────┐                 │
│         │                │                │                 │
│         ▼                ▼                ▼                 │
│  ┌────────────┐   ┌────────────┐   ┌────────────┐           │
│  │Save User   │   │Save Asst   │   │ Save Cost  │           │
│  │Message     │   │Message     │   │ Tokens     │           │
│  └────────────┘   └────────────┘   └────────────┘           │
│         │                │                                  │
│         └────────┬───────┘                                  │
│                  ▼                                          │
│           ┌────────────┐                                    │
│           │ Send       │                                    │
│           │ Telegram   │                                    │
│           └────────────┘                                    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 🧠 Conversation Memory

### Implementation

Memory is stored in **n8n Data Tables** (`gilgamesh_conversations`).

| Column | Type | Purpose |
|--------|------|---------|
| id | Auto | Primary key |
| chat_id | String | Telegram chat ID |
| role | String | "user" or "assistant" |
| content | String | Message text |
| timestamp | DateTime | When sent |

### Memory Loading

```javascript
// Load last 20 messages for this chat
const messages = $('Load Memory').all()
  .sort((a, b) => new Date(a.json.timestamp) - new Date(b.json.timestamp))
  .slice(-20)
  .map(msg => ({
    role: msg.json.role,
    content: msg.json.content
  }));

return { messages };
```

### Memory Saving

After each exchange:
1. Save user message with role "user"
2. Save assistant response with role "assistant"
3. Both include chat_id and timestamp

---

## 🔀 Smart Routing

### Model Selection Logic

```javascript
const message = $json.message.text.toLowerCase();

// Simple patterns → Haiku (fast, cheap)
const simplePatterns = [
  /^(hi|hello|hey|morning|evening|thanks|thank you|ok|okay|bye|good)/,
  /^(what time|what date|how are you)/,
  /^.{1,20}$/ // Very short messages
];

const isSimple = simplePatterns.some(p => p.test(message));

return {
  model: isSimple 
    ? 'claude-haiku-4-5-20251001' 
    : 'claude-sonnet-4-20250514',
  isSimple
};
```

### Model Specifications

| Model | Use Case | Speed | Cost |
|-------|----------|-------|------|
| claude-haiku-4-5-20251001 | Greetings, simple Q&A | Fast | Low |
| claude-sonnet-4-20250514 | Technical, analysis, complex | Medium | Medium |

---

## 🔍 Web Search

### Enabling Web Search

```json
{
  "model": "claude-sonnet-4-20250514",
  "max_tokens": 4096,
  "tools": [
    {
      "type": "web_search_20250305",
      "name": "web_search"
    }
  ],
  "messages": [...]
}
```

### Handling Multi-Block Responses

When web search is used, Claude returns multiple content blocks:

```json
{
  "content": [
    { "type": "web_search_tool_result", "..." },
    { "type": "text", "text": "Based on my search..." },
    { "type": "text", "text": "Additional information..." }
  ]
}
```

**Solution:** Filter for text blocks and join:

```javascript
const content = $json.content;

// Filter text blocks and join
const textResponse = content
  .filter(block => block.type === 'text')
  .map(block => block.text)
  .join('\n\n');

return { response: textResponse };
```

---

## 💰 Cost Tracking

### Cost Table Structure

Table: `gilgamesh_costs`

| Column | Type | Purpose |
|--------|------|---------|
| id | Auto | Primary key |
| timestamp | DateTime | When request made |
| model | String | Model used |
| input_tokens | Number | Tokens in prompt |
| output_tokens | Number | Tokens in response |
| chat_id | String | Telegram chat ID |

### Extracting Token Usage

```javascript
const usage = $json.usage;

return {
  input_tokens: usage.input_tokens,
  output_tokens: usage.output_tokens,
  model: $json.model,
  timestamp: new Date().toISOString(),
  chat_id: $('Telegram Trigger').first().json.message.chat.id
};
```

---

## 🐛 Issues & Fixes

### Issue 1: Data Table Schema Caching

**Problem:** After modifying gilgamesh_costs columns, n8n still tried to use old schema  
**Solution:** Delete and recreate the table fresh — n8n caches schema

### Issue 2: Multi-Block Response Handling

**Problem:** Web search responses have multiple content blocks; saving only first block lost data  
**Solution:** Add Extract Response node to filter and join text blocks

### Issue 3: If Node with Optional Fields

**Problem:** Switch node failed when checking optional message.text field  
**Solution:** Enable "Convert types where required" in Switch node settings

### Issue 4: Expression Scoping

**Problem:** Accessing `$('NodeName').first().json.x` from distant nodes returned undefined  
**Solution:** Use intermediate Code nodes to pass data forward explicitly

---

## ✅ Verification

| Feature | Test | Status |
|---------|------|--------|
| Memory | Send multiple messages, verify context retained | ✅ |
| Smart routing | Send "hi" → Haiku, send technical question → Sonnet | ✅ |
| Web search | Ask about current events | ✅ |
| Cost tracking | Check gilgamesh_costs table after queries | ✅ |
| Multi-block | Web search response displays correctly | ✅ |

---

## 📊 Example Interaction

```
User: hi
Bot: [Haiku] Hello! How can I help you today?

User: What's the latest news about Proxmox?
Bot: [Sonnet + Web Search] Based on my search, Proxmox VE 8.2 
     was recently released with improvements to...

User: How do I configure ZFS on Proxmox?
Bot: [Sonnet] To configure ZFS on Proxmox, you'll want to...
```

---

## 📝 Lessons Learned

1. **n8n Data Tables cache schema** — Delete and recreate if columns change
2. **Claude multi-block responses** — Filter by `type === "text"` and join
3. **Switch node optional fields** — Enable type conversion
4. **Expression scoping in n8n** — Use Code nodes to pass data between distant nodes

---

## 🔗 Related

- **Prerequisite:** [Phase 7C: Gilgamesh Telegram Bot](phase-7c-gilgamesh-bot.md)
- **Security:** [Phase 7D-Sec: Cloudflare Access for n8n](phase-7d-sec-cloudflare-access.md)
- **Next:** Phase 7D-Menu (Inline Keyboard Menu)

---

*Completed: April 6, 2026*
