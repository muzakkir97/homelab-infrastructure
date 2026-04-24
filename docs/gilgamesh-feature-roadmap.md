# Gilgamesh AI Agent - Feature Roadmap

## Overview

Gilgamesh is a Telegram-based AI assistant for homelab infrastructure management, built on n8n workflows with Claude API integration. The primary goal is to **replace Claude Web interface by August 2026**, reducing monthly AI costs from $30-40 to $5-10.

## Current Architecture

```
Telegram Bot (@JhinGilgamesh_bot)
            ↓
    n8n Workflow Engine (CT 211)
            ↓
    ┌─────────────┬─────────────┐
Smart Routing    Memory System   Cost Tracking
(Haiku/Sonnet)   (Data Tables)   (Token Usage)
            ↓
        Claude API
            ↓
    Documentation Pipeline
    (GitHub + Nextcloud)
```

## Current Features (✅ WORKING)

### Core Chat Capabilities
- **Smart Model Routing:** Simple queries → Haiku 4.5, complex → Sonnet 4
- **Conversation Memory:** Last 20 messages stored in n8n Data Tables
- **Web Search:** Real-time information via Claude's web_search tool
- **Cost Tracking:** Token usage logged (estimated costs due to API bug)

### Telegram Interface
- **Bot Handle:** @JhinGilgamesh_bot
- **Chat ID:** 518832696
- **Message Format:** Plain text only (JSON parsing constraints)
- **Response Limit:** Telegram 4096 character maximum

### Slash Commands (8 Total)
| Command | Purpose | Status |
|---------|---------|--------|
| /help | Show all commands and menu options | ✅ Working |
| /clear | Clear conversation memory | ✅ Working |
| /memory | View recent conversation history | ✅ Working |
| /cost | Display token usage and estimated costs | ✅ Working |
| /alerts | Show active Alertmanager notifications | ✅ Working |
| /backup | Last backup times for all containers | ✅ Working |
| /update | Push session summary to docs (3 files) | ✅ Working |
| /sync-docs | Full documentation regeneration (7 files) | ✅ Working |

### Inline Keyboard Menu System

#### Main Menu
- 🏠 Homelab
- 🎮 Gaming
- 🤖 Gilgamesh
- 🔧 Tools
- ❓ Help

#### Homelab Submenu (All ✅ Working)
- **Status:** Container states via Proxmox API
- **Metrics:** CPU/RAM/Disk usage with progress bars
- **Temps:** Hardware temperatures via SSH + lm-sensors
- **Storage:** ZFS/LVM pool utilization

#### Gaming Submenu (✅ Working)
- **Server Status:** Docker container states on CT 302
- **Direct Display:** No nested submenus

#### Gilgamesh Submenu (✅ Working)
- **Info Panel:** Bot statistics and capabilities
- **Static Information:** Version, features, model routing

#### Tools Submenu (✅ Working)
- **Quick Links:** External service URLs
- **Static Reference:** No dynamic functionality

## Smart Routing Logic

### Model Selection Criteria
```python
# Simplified logic (actual implementation in n8n)
if message_length < 100 and no_technical_keywords:
    model = "claude-haiku-4-5-20251001"
    cost_per_token = lower
else:
    model = "claude-sonnet-4-20250514"
    cost_per_token = higher
```

### Technical Implementation
- **Haiku Use Cases:** Simple questions, status checks, casual chat
- **Sonnet Use Cases:** Complex analysis, troubleshooting, documentation
- **Override:** Manual model selection not implemented

## Memory System (Data Tables)

### Schema
```sql
Table: gilgamesh_memory
- id (auto-increment)
- chat_id (518832696)
- role (user/assistant)
- content (message text)
- timestamp (datetime)
- tokens_used (integer)
```

### Limitations
- **Capacity:** 20 messages maximum
- **Persistence:** Lost on n8n restart
- **Context:** No conversation threading
- **Cleanup:** Manual via /clear command

## Documentation Pipeline

### Update Workflow (/update)
```
Session Summary → Claude Processing → 3 Files Updated
- AI-CONTEXT.md (master context)
- changelog.md (version history)
- troubleshoot.md (lessons learned)
```

### Sync Docs Workflow (/sync-docs)
```
Full Regeneration → Claude Processing → 7 Files Updated
- All /update files PLUS
- README.md
- roadmap.md
- current-state.md
- service-catalog.md
```

### Integration Points
- **Nextcloud:** File storage via app password
- **GitHub:** Version control via API token
- **Webhook Triggers:** doc-update, doc-sync

## API Integration

### Proxmox VE
- **Endpoint:** https://192.168.10.5:8006/api2/json
- **Authentication:** root@pam!gilgamesh token
- **Node Name:** muzakkir (not kuromoon)
- **Capabilities:** Container status, resource metrics, storage info

### SSH Access
- **Kuromoon (Proxmox):** Key-based auth, port 22
- **CT 302 (Gaming):** Password auth (PermitRootLogin yes)
- **Commands:** lm-sensors, docker ps, system monitoring

### External APIs
- **Claude:** Haiku/Sonnet models with web search
- **GitHub:** Repository file updates
- **Nextcloud:** File upload/download

## Technical Constraints

### Known Limitations
- **Telegram Messages:** Plain text only (no markdown/code blocks)
- **Cost Tracking Bug:** cost_usd column always returns 0
- **Memory Persistence:** Lost on container restart
- **Model Context:** Limited to 20 messages
- **File Generation:** Text only, no binary/image support

### Performance Issues
- **Response Time:** 3-5 seconds for Haiku, 10-15 for Sonnet
- **Rate Limiting:** Anthropic API limits apply
- **Memory Usage:** n8n Data Tables grow over time
- **Error Handling:** Basic retry logic only

## 16-Week Evolution Plan (August 2026 Target)

### Phase 1: Foundation (Weeks 1-4)
#### Phase 38 - Ollama + ROCm Integration (CRITICAL)
- **GPU:** AMD RX 6700 XT (IOMMU Group 18)
- **Models:** Llama 3.3, Qwen 2.5, local inference
- **Cost Impact:** Eliminate 70% of API calls
- **Status:** Near Term #1 priority

#### Phase 22 - Obsidian Knowledge Base (✅ COMPLETE)
- **Vault:** C:\Users\muzak\Nextcloud\Obsidian\second-brain
- **Plugins:** Dataview, Tasks, Templater, Calendar, Excalidraw, Kanban
- **Integration:** Auto-sync via Nextcloud to CT 220

### Phase 2: Intelligence (Weeks 5-8)
#### Phase 7E - Extended Memory System
- **Capacity:** 20+ message conversations
- **Technology:** RAG (Retrieval Augmented Generation)
- **Storage:** Obsidian vault integration
- **Search:** Semantic similarity matching

#### Phase 41 - Hybrid Routing Logic
- **Local Models:** Simple queries, homelab-specific tasks
- **API Models:** Complex reasoning, latest information
- **Decision Tree:** Automated complexity assessment
- **Fallback:** API when local model confidence low

### Phase 3: Capabilities (Weeks 9-12)
#### Phase 7F - File Generation
- **Text Files:** Configs, scripts, documentation
- **Code:** YAML, JSON, shell scripts, Python
- **Diagrams:** Mermaid, ASCII art, network topology
- **Templates:** Service deployments, monitoring rules

#### Phase 7G - Vision API Integration
- **Image Analysis:** Screenshot troubleshooting
- **Diagram Recognition:** Network topology parsing
- **OCR:** Text extraction from images
- **Graph Reading:** Grafana dashboard analysis

#### Phase 7H - Document Upload Processing
- **PDF Support:** Manual uploads, knowledge extraction
- **Text Files:** Configuration analysis
- **Log Files:** Error pattern recognition
- **Integration:** Obsidian vault storage

### Phase 4: Refinement (Weeks 13-16)
#### Phase 7I - Quality Assurance
- **Comparison Testing:** Gilgamesh vs Claude Pro
- **Accuracy Metrics:** Technical response quality
- **Performance:** Response time benchmarking
- **User Experience:** Feature parity assessment

#### Phase 7J - Migration Complete
- **Cost Target:** $5-10/month total
- **Feature Parity:** Match Claude Pro capabilities
- **Backup Access:** Claude Pro retained for comparison
- **Success Validation:** 4-week production usage

## Planned Features

### Near-Term (Next 4 Weeks)

#### MERLIN Reminder Agent (#1 Priority)
- **Purpose:** Proactive infrastructure alerts
- **Examples:** SSL renewals, backup test reminders, memory warnings
- **Platform:** Separate n8n workflow with cron triggers
- **Integration:** Shared Data Tables with Gilgamesh

#### Proactive Monitoring
- **Daily Summary:** Automated system health report
- **Trend Analysis:** Resource usage patterns
- **Predictive Alerts:** Capacity planning warnings
- **Scheduled Reports:** Weekly infrastructure review

### Medium-Term (4-8 Weeks)

#### Web Chat UI
- **Platform:** Homepage dashboard embedded chat
- **Memory:** Shared with Telegram conversations
- **Authentication:** Cloudflare Access integration
- **Features:** File uploads, diagram viewing

#### Advanced Context
- **Knowledge Base:** Full Obsidian vault access
- **Service Documentation:** Auto-generated from deployments
- **Historical Data:** Past issue resolution database
- **Learning:** Continuous improvement from interactions

### Long-Term (8-16 Weeks)

#### Multi-Modal Capabilities
- **Voice Input:** Telegram voice message processing
- **Image Output:** Generated diagrams and charts
- **Video Analysis:** Screen recordings for troubleshooting
- **Document Generation:** PDF reports with formatting

#### Advanced Automation
- **Service Deployment:** One-command container creation
- **Configuration Management:** Automated updates with approval
- **Incident Response:** Automated troubleshooting steps
- **Capacity Management:** Predictive scaling recommendations

## Agent Ecosystem (Fate/Grand Order Theme)

### Active Servants
| Servant | Class | Role | Platform | Status |
|---------|--------|------|----------|--------|
| Gilgamesh 👑 | Archer | Personal AI Assistant | Telegram | ✅ Active |
| Caster (Da Vinci) 🎨 | Caster | Knowledge Curator | n8n/Nextcloud | ✅ Active |
| Archer (EMIYA) 🏹 | Archer | Infrastructure Translator | n8n | ✅ Active |

### Planned Servants (Priority Order)
| Servant | Class | Role | Noble Phantasm | Implementation |
|---------|--------|------|----------------|----------------|
| MERLIN 🔮 | Caster | Reminders & Scheduler | Garden of Avalon | Phase 24+ |
| Mash Kyrielight 🛡️ | Shielder | Gaming Server + Discord Bot | Lord Camelot | Phases 59-64 |
| Guardian | — | Security Monitoring | — | TBD |
| Nexus | — | Cross-platform Automation | — | TBD |
| Scribe | — | Auto-documentation | — | TBD |
| Midas | — | Cost Tracking & Optimization | — | TBD |
| Oracle | — | Predictive Intelligence | — | TBD |

### Mash Discord Bot (Phases 59-64)
- **Platform:** Discord for gaming friends
- **Commands:** !start, !stop, !status for game servers
- **Announcements:** Player joins/leaves, game updates
- **Personality:** "Senpai, PlayerX just joined Windrose!"
- **Separation:** Gaming isolated from homelab admin

## Success Metrics

### Cost Reduction Target
| Component | Current | Target | Annual Savings |
|-----------|---------|--------|----------------|
| Claude Pro | $20/month | $0/month | $240 |
| API Usage | $10-20/month | $5-10/month | $60-120 |
| **Total** | **$30-40/month** | **$5-10/month** | **$300-360** |

### Quality Benchmarks
- **Response Accuracy:** Match Claude Pro for homelab queries
- **Response Time:** < 5 seconds for 80% of queries
- **Memory Retention:** 20+ message conversations
- **Uptime:** 99%+ availability
- **User Satisfaction:** Preference over Claude Web interface

### Feature Parity Goals
- [ ] 20+ message conversation memory
- [ ] Knowledge base recall from Obsidian
- [ ] Vision capabilities for screenshots
- [ ] File generation (configs, scripts, docs)
- [ ] Monthly cost under $10
- [ ] Web chat interface
- [ ] Document upload processing
- [ ] Proactive monitoring alerts

## Risk Mitigation

### Backup Plan
- **Claude Pro:** Retained for quality comparison
- **API Access:** Maintained for complex queries
- **Gradual Migration:** Phased rollover to minimize disruption
- **Performance Monitoring:** Quality gates before full switch

### Technical Risks
- **GPU Performance:** RX 6700 XT may be insufficient for larger models
- **Memory Constraints:** 32GB RAM limits model size
- **Network Bandwidth:** Remote access may be slow for large responses
- **Model Quality:** Local models may not match Claude's capabilities

### Mitigation Strategies
- **Hybrid Approach:** Local + API combination
- **Model Selection:** Choose efficient models for available hardware
- **Caching:** Store common responses locally
- **Fallback Logic:** Automatic API fallback when local fails