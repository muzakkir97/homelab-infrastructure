# n8n Workflows — Complete Documentation

> **Last Updated:** April 27, 2026  
> **Purpose:** Complete documentation of all n8n workflows, architecture, and dependencies  
> **Platform:** n8n automation (CT 211, 192.168.30.211)  

## Workflow Overview

**Total Active Workflows:** 12  
**Platform:** n8n v1.x on LXC container (CT 211)  
**Status:** 100% operational, all workflows enabled  
**Integration Hub:** Central orchestration for all homelab agents  

## Core Agent Workflows

### 1. Telegram Agent (Primary)
**Purpose:** Gilgamesh AI agent — main chat interface, hybrid routing, menu system  
**Trigger:** Telegram webhook (@JhinGilgamesh_bot)  
**Node Count:** 15+ (complex branching logic)  
**Status:** ✅ ACTIVE — Primary interface

**Architecture:**
```
Telegram Message → Route Check → Smart Router
                                      ↓
                   ┌──────────────────────────────────────┐
                   ▼                  ▼                   ▼
             Ollama qwen3:14b    Haiku 4.5          Sonnet 4
             (local primary)   (API fallback)   (complex queries)
                   └──────────────────────────────────────┘
                                      ▼
                              Memory Storage (Data Tables)
                                      ▼
                          Response + Menu/Command Handling
```

**Key Features:**
- **Smart Routing:** Complexity analysis determines model selection
- **Conversation Memory:** Last 20 messages stored in gilgamesh_memory table
- **Cost Tracking:** Token usage logged to gilgamesh_costs table
- **Inline Keyboards:** Full menu system with homelab status, metrics, gaming
- **Slash Commands:** 10 commands for direct actions (/help, /clear, /cost, etc.)
- **Integration Calls:** Proxmox API, Prometheus metrics, Alertmanager alerts

**Routing Logic:**
- Messages >50 words OR complexity keywords → Sonnet
- Ollama available → qwen3:14b (local)  
- Ollama unavailable → Haiku (fallback)

**Dependencies:**
- Vault: kv/gilgamesh (Telegram token, Claude API key)
- Proxmox: root@pam!gilgamesh token
- Data Tables: gilgamesh_memory, gilgamesh_costs

### 2. Documentation Pipeline — Update
**Purpose:** Session summary integration (/update command)  
**Trigger:** Webhook (doc-update)  
**Node Count:** 7  
**Status:** ✅ ACTIVE — Used after every session

**Workflow:**
```
/update Command → Session Summary → Raw Append → AI-CONTEXT-staging.md
                                                         ↓
                                              Trigger Da Vinci Workflow
                                                         ↓
                                    Formatted Updates → 3 Files Updated
```

**Files Updated:**
1. AI-CONTEXT.md (master context)
2. changelog.md (version history)  
3. troubleshoot.md (lessons learned)

**Dependencies:**
- Da Vinci Documentation Pipeline workflow
- Nextcloud WebDAV (admin credentials)
- GitHub API (muzakkir97 token)

### 3. Documentation Pipeline — Sync Docs  
**Purpose:** Full documentation regeneration (/sync-docs command)  
**Trigger:** Webhook (doc-sync)  
**Node Count:** 7  
**Status:** ✅ ACTIVE — Used for major deployments

**Files Generated (7 total):**
1. AI-CONTEXT.md 
2. changelog.md
3. troubleshoot.md  
4. README.md
5. roadmap.md
6. current-state.md
7. service-catalog.md

**Process:**
- Fetches current AI-CONTEXT.md from Nextcloud
- Claude API call (max_tokens: 32000)
- Generates all 7 files from master context
- Pushes to both Nextcloud and GitHub

**Dependencies:**
- Same as Update workflow
- Higher token usage (~15K tokens per run)

### 4. Da Vinci Documentation Pipeline
**Purpose:** Raw summary processing and document formatting (Stage 1)  
**Trigger:** Webhook (da-vinci)  
**Node Count:** 11  
**Status:** ✅ ACTIVE — Processes all /update summaries

**Architecture:**
```
Raw Summaries → AI-CONTEXT-staging.md (rolling append)
                              ↓
                    Da Vinci n8n Workflow
                              ↓
               Claude API (max_tokens: 32000) + Grounding
                              ↓
               Formatted AI-CONTEXT.md → Nextcloud + GitHub
```

**Key Features:**
- **Staging File:** AI-CONTEXT-staging.md preserves raw summaries
- **Grounding:** Fetches current AI-CONTEXT.md before Claude API call
- **High Token Limit:** 32000 tokens prevents content truncation
- **Direct Node References:** Uses `$('Extract Response').first().json.updatedDoc`

**Technical Notes:**
- **Bug Fix Applied:** Direct node references instead of `$json` after Merge
- **Pronouns:** Da Vinci uses she/her pronouns consistently
- **Error Handling:** Robust retry logic for Claude API failures

**Dependencies:**
- Vault: kv/gilgamesh (Claude API key)
- Nextcloud: admin WebDAV credentials
- GitHub: muzakkir97 API token

## Agent Workflows

### 5. Midas — CFO Report
**Purpose:** Cost analysis and savings reporting (/midas command)  
**Trigger:** Webhook (midas-report)  
**Node Count:** 6  
**Status:** ✅ ACTIVE — Financial intelligence

**Features:**
- **Token Usage Analysis:** Breakdown by model (Sonnet, Haiku, Ollama)
- **Cost Calculation:** USD rates applied, converted to MYR (rate: 4.7)
- **Ollama Savings:** Calculated at Haiku equivalent rate
- **Spend Monitoring:** $10 monthly limit tracking
- **Command Breakdown:** Costs by command_type for detailed analysis

**SQL Queries:**
- Aggregates gilgamesh_costs table by model and command_type
- Calculates savings from local Ollama inference
- Tracks monthly spend against budget limits

### 6. Midas — Daily Brief
**Purpose:** Scheduled morning cost summary  
**Trigger:** Schedule (daily 9:00 AM)  
**Node Count:** 4  
**Status:** ✅ ACTIVE — Proactive reporting

**Content:**
- Previous day token usage and costs
- Month-to-date spending vs budget
- Ollama vs API usage ratio
- Cost optimization recommendations

### 7. MERLIN — Reminders  
**Purpose:** Daily infrastructure health checks  
**Trigger:** Schedule (daily 8:00 AM)  
**Node Count:** 8  
**Status:** ✅ ACTIVE — Proactive monitoring

**Current Checks:**
- **SSL Certificate:** Hardcoded expiry July 14, 2026 (days remaining)
- **Backup Restore Test:** Last test baseline 2026-01-01 (overdue days)  
- **Prometheus Memory:** 85% threshold check (OOM prevention)
- **Vault Seal Status:** Confirms Vault is unsealed and accessible

**Technical Implementation:**
- **Direct Node References:** Reads specific nodes by name for reliability
- **Error Handling:** Individual check failures don't break entire workflow
- **Alerts:** Telegram notifications for items requiring attention

**Future Expansion:**
- Container update notifications
- Scheduled maintenance windows  
- Security patch alerts

### 8. Daily Note Creator
**Purpose:** Midnight Obsidian daily note creation  
**Trigger:** Schedule (daily 00:00)  
**Node Count:** 4  
**Status:** ✅ ACTIVE — Knowledge management automation

**Process:**
- Generates daily note with template (date, weather placeholder, tasks)
- Writes to Obsidian vault via Nextcloud WebDAV
- Path: `/07-daily/YYYY-MM-DD.md`
- Template includes sections for: Summary, Key Events, Tasks, Notes

### 9. Morning Briefing  
**Purpose:** Daily 7am summary to Telegram  
**Trigger:** Schedule (daily 07:00)  
**Node Count:** 5  
**Status:** ✅ ACTIVE — Daily intelligence briefing

**Content:**
- Infrastructure health summary
- Previous day key metrics
- Agenda items for the day
- Weather and schedule information (placeholder)

## Legacy Workflows (Unpublished)

### 10. Update Nextcloud File
**Purpose:** Direct file updates to Nextcloud (superseded by Da Vinci)  
**Trigger:** Webhook  
**Node Count:** 5  
**Status:** 🔄 LEGACY — Kept for reference

### 11. Push to GitHub  
**Purpose:** Direct GitHub pushes (superseded by Da Vinci)  
**Trigger:** Webhook  
**Node Count:** 4  
**Status:** 🔄 LEGACY — Integrated into main pipelines

### 12. Service Down Alert (Inactive)
**Purpose:** Container failure notifications  
**Trigger:** Webhook (from Alertmanager)  
**Node Count:** 3  
**Status:** ⚠️ INACTIVE — Replaced by MERLIN checks

**Note:** Will be reactivated when Alertmanager alerts route through n8n (planned enhancement)

## Data Tables Schema

### gilgamesh_memory
**Purpose:** Conversation context storage  
**Retention:** Last 20 messages per conversation  
**Columns:**
- `id` (Auto-increment)
- `timestamp` (DateTime)  
- `role` (Text: user/assistant)
- `content` (Long Text: message content)

### gilgamesh_costs  
**Purpose:** Token usage and cost tracking  
**Columns:**
- `id` (Auto-increment)
- `timestamp` (DateTime)
- `model` (Text: sonnet/haiku/ollama)
- `input_tokens` (Number)
- `output_tokens` (Number)  
- `cost_usd` (Number: calculated from tokens)
- `command_type` (Text: derived from message)

**Schema Notes:**
- Data Tables have caching bugs — delete/recreate if schema issues occur
- 10,000 row soft limit per table
- Manual cleanup required for old conversation history

## Credential Management

### Vault Integration (8 Paths)
- **kv/gilgamesh** — Telegram bot token, Claude API keys
- **kv/nextcloud** — WebDAV admin credentials  
- **kv/github** — API token (muzakkir97 user required in config)
- **kv/proxmox** — root@pam!gilgamesh token
- **kv/cloudflare** — DNS API token (PENDING FIX: truncated to 7 chars)
- **kv/alertmanager** — Webhook URLs
- **kv/n8n** — Service-specific credentials
- **kv/pihole** — Admin passwords

### API Tokens & Authentication
- **Telegram Bot:** @JhinGilgamesh_bot token from BotFather
- **Claude API:** Anthropic API key with Sonnet/Haiku access
- **GitHub:** Personal access token with repo read/write permissions
- **Proxmox:** API token (NOT cookie auth) for reliable automation
- **Nextcloud:** Admin user WebDAV (not muzakkir user)

## Integration Architecture

### External APIs
```
n8n (CT 211) ←→ Telegram Bot API (chat interface)
            ←→ Claude API (Anthropic - intelligence)  
            ←→ Ollama API (VM 400 - local inference)
            ←→ Proxmox API (infrastructure control)
            ←→ Prometheus API (metrics queries)
            ←→ Nextcloud WebDAV (file storage)
            ←→ GitHub API (version control)
            ←→ Vault API (secrets management)
```

### Internal Services
```
CT 211 (n8n) ←→ CT 213 (Vault) — secrets retrieval
            ←→ CT 220 (Nextcloud) — file operations
            ←→ CT 202 (Prometheus) — metrics queries  
            ←→ CT 205 (Alertmanager) — alert queries
            ←→ VM 400 (Ollama) — local AI inference
```

## Known Issues & Lessons Learned

### n8n Platform Issues
| Issue                                    | Resolution                                                    |
|------------------------------------------|---------------------------------------------------------------|
| Data Tables schema caching bug          | Delete and recreate table entirely                           |
| Expression scoping after Merge node     | Use Code node with `this.helpers.httpRequest`                |
| input.first()/input.last() unreliable   | Use direct node references: `$('Node Name').first().json`    |
| Telegram single webhook constraint      | All update types (message + callback) must use one trigger   |
| Answer Callback node loses data         | Bypass it; handle callback acknowledgment separately         |

### API Integration Issues  
| Issue                              | Resolution                                                        |
|------------------------------------|-------------------------------------------------------------------|
| Claude API 404 model not found    | Use exact model ID: `claude-haiku-4-5-20251001`                  |
| GitHub credential 401              | User field (`muzakkir97`) must be populated                      |
| Ollama tokens always 0             | Read data.input_tokens, not prompt_eval_count (already mapped)   |
| Nextcloud WebDAV 401               | Use admin username, not muzakkir                                 |
| Proxmox API intermittent failures  | Implement retry logic with exponential backoff                   |

### Workflow Design Issues
| Issue                                | Resolution                                                      |
|--------------------------------------|-----------------------------------------------------------------|
| Web search partial response capture | Add Extract Response Code node, filter by `type === "text"`    |
| Da Vinci grounding bug               | Fetch current AI-CONTEXT.md before Claude API call             |
| Max tokens too low                   | Set to 32000 to prevent content replacement with hallucinations|
| Cost tracking missing command type   | Add command_type column, derive from Telegram message text     |

## Performance Metrics

### Resource Usage
- **CPU:** 2-4% average, 15-25% during AI inference calls
- **Memory:** 512MB base, 1GB peak during large document processing
- **Disk I/O:** Low except during backup operations
- **Network:** 10-50Mbps during file uploads to GitHub/Nextcloud

### Response Times
- **Simple Queries (Ollama):** 2-8 seconds
- **Complex Queries (Sonnet):** 5-15 seconds  
- **Menu Operations:** <1 second
- **File Operations:** 2-5 seconds (WebDAV)
- **Documentation Pipeline:** 30-60 seconds (/sync-docs)

### Reliability Metrics
- **Uptime:** 99.5% (excluding planned maintenance)
- **Success Rate:** 98% for API calls (with retries)
- **Error Recovery:** Automatic for transient failures
- **Data Integrity:** 100% (no data loss incidents)

## Future Enhancements

### Planned Workflow Additions
- **Guardian Security Monitoring** — Agent #3 in build order
- **Monthly Infrastructure Audit** — Automated health reporting
- **Alertmanager Integration** — Route all alerts through n8n
- **Sherlock Holmes Web Scraper** — RSS feeds, Reddit, news monitoring

### Architecture Improvements  
- **Central Alert Hub** — All homelab alerts processed through n8n
- **Agent Communication** — Inter-agent messaging and coordination
- **Workflow Templates** — Reusable patterns for new agents
- **Advanced Error Handling** — Circuit breakers and fallback chains

### Performance Optimizations
- **Workflow Caching** — Cache frequent API responses
- **Batch Processing** — Group similar operations
- **Resource Pooling** — Shared connections for external APIs
- **Monitoring Integration** — Prometheus metrics for n8n workflows

---

**Deployment Status:** All 9 active workflows operational, 3 legacy workflows preserved  
**Next Development:** Guardian security monitoring agent (build order #3)  
**Maintenance Schedule:** Weekly restart, monthly Data Tables cleanup  
**Documentation:** This file updated with every new workflow deployment