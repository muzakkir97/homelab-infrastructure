# Gilgamesh AI Agent — Feature Roadmap

## Overview

Gilgamesh is a Telegram-based AI agent built on n8n workflows with Claude API integration. The primary goal is to **replace claude.ai subscription by August 2026**, reducing costs from $30-40/month to $5-10/month while maintaining equivalent capabilities.

## Current Architecture

```
Telegram Bot (@JhinGilgamesh_bot)
    ↓
n8n Workflow (CT 211: automation-n8n)
    ↓
Claude API (Haiku 4.5 / Sonnet 4)
    ↓
Response + Memory Storage (n8n Data Tables)
    ↓
Documentation Pipeline (GitHub + Nextcloud)
```

### Technical Specifications

| Component         | Value                       |
|-------------------|-----------------------------|
| Telegram Bot      | @JhinGilgamesh_bot          |
| Primary Chat ID   | 518832696                   |
| Haiku Model       | claude-haiku-4-5-20251001   |
| Sonnet Model      | claude-sonnet-4-20250514    |
| n8n Container     | CT 211 (192.168.30.211)    |
| Proxmox Node      | muzakkir (not kuromoon)     |
| API Token         | root@pam!gilgamesh          |

## Current Features (Status: Working ✅)

### 1. Conversation Memory System
- **Implementation**: n8n Data Tables (gilgamesh_memory)
- **Retention**: Last 20 messages per conversation
- **Storage**: Message content, timestamp, role (user/assistant)
- **Commands**: `/memory` (view history), `/clear` (reset memory)

### 2. Smart Message Routing
- **Simple Queries** → Claude Haiku 4.5 (faster, cheaper)
- **Complex Queries** → Claude Sonnet 4 (better reasoning)
- **Routing Logic**: Message length, keywords, context complexity

### 3. Real-time Web Search
- **Tool**: Claude's native web_search capability
- **Trigger**: Current events, recent information needs
- **Integration**: Seamless within conversation flow

### 4. Cost Tracking
- **Logging**: Token usage per request stored in gilgamesh_costs table
- **Display**: `/cost` command shows usage statistics
- **Known Issue**: cost_usd column returns 0 (pre-existing bug)
- **Workaround**: Estimated costs from token counts (Sonnet $3/$15 per 1M tokens)

### 5. Inline Keyboard Menu System

#### Main Menu Options
- **🏠 Homelab** → Status, Metrics, Temps, Storage submenus
- **🎮 Gaming** → Server status (Terraria, Minecraft, Windrose)
- **🤖 Gilgamesh** → Agent information and statistics
- **🛠️ Tools** → Quick access links to services
- **❓ Help** → Command reference and documentation

#### Homelab Submenu (All Working ✅)
- **Status**: Container status via Proxmox API
- **Metrics**: CPU, memory, uptime statistics
- **Temps**: Hardware temperatures via SSH to Kuromoon
- **Storage**: Disk usage across all storage pools

#### Gaming Submenu (Working ✅)
- **Server Status**: Docker container status on CT 302
- **Direct Display**: No additional submenus (streamlined)

#### Technical Implementation
- **SSH Access**: n8n → Kuromoon (port 22), n8n → CT 302 (password auth)
- **API Integration**: Proxmox API for container and storage metrics
- **Progress Bars**: ASCII characters (= and -) for Telegram mobile compatibility

### 6. Slash Commands (6 Commands Active)

| Command   | Purpose                  | Description                           |
|-----------|--------------------------|---------------------------------------|
| `/help`   | Quick reference          | Shows all commands and menu options   |
| `/clear`  | Memory reset             | Clears last 20 messages              |
| `/memory` | View conversation history| Shows recent message log              |
| `/cost`   | Usage tracking           | Token usage and estimated costs       |
| `/alerts` | System monitoring        | Active Alertmanager alerts            |
| `/backup` | Backup verification      | Last backup times for all containers  |

### 7. Documentation Pipeline (Working ✅)

#### Session Summary Workflow (`/update`)
- **Trigger**: After each session
- **Files Updated**: 3 (AI-CONTEXT.md, changelog.md, troubleshoot.md)
- **Process**: Claude merges session summary into existing documentation

#### Full Regeneration Workflow (`/sync-docs`)
- **Trigger**: After deployment sessions
- **Files Updated**: 7 (full documentation suite)
- **Process**: Complete regeneration from AI-CONTEXT.md master file

#### Pipeline Architecture
```
Telegram Command (/update or /sync-docs)
    ↓
n8n Webhook (doc-update or doc-sync)
    ↓
Claude API (intelligent document merging)
    ↓
Nextcloud (file storage via app password)
    ↓
GitHub API (version control push)
```

## Planned Features (4-Phase Roadmap)

### PRIMARY GOAL: Replace claude.ai by August 2026 (16 weeks)
**Cost Reduction**: $30-40/month → $5-10/month ($300-360/year savings)

### Phase 1: Foundation (Weeks 1-4)
- **Phase 38** — Ollama + ROCm on Kuromoon RX 6700 XT (CRITICAL)
- **Phase 22** — Obsidian Knowledge Base integration

### Phase 2: Intelligence (Weeks 5-8)
- **Phase 7E** — Extended Memory (20+ message conversations via RAG)
- **Phase 41** — Hybrid Routing (local/API based on complexity)

### Phase 3: Capabilities (Weeks 9-12)
- **Phase 7F** — File Generation (code, configs, documentation)
- **Phase 7G** — Vision API (image analysis, screenshots)
- **Phase 7H** — Document Upload (PDF/text processing)

### Phase 4: Refinement (Weeks 13-16)
- **Phase 7I** — Quality Assurance (compare outputs with Claude Pro)
- **Phase 7J** — Migration (complete switch to Gilgamesh)

### Future Enhancements (Post-Migration)
- **Phase 7K** — AI News Scraper (RSS/Reddit → Ollama → Telegram alerts)
- **Web Chat UI** — Homepage embedded interface (shared memory)
- **Proactive Alerts** — System health monitoring with AI analysis
- **Daily Summary** — Automated infrastructure status reports

## Cost Projection

| Component           | Current      | Target       | Annual Savings |
|---------------------|--------------|--------------|----------------|
| Claude Pro          | $20/month    | $0/month     | $240           |
| API Usage           | $10-20/month | $5-10/month  | $60-120        |
| **Total Monthly**   | **$30-40**   | **$5-10**    | **—**          |
| **Annual Savings**  | **—**        | **—**        | **$300-360**  |

## Integration Points

### Active Integrations
- **Proxmox API**: Container status, metrics, storage information
- **Telegram Bot API**: Message handling, inline keyboards, webhooks
- **Claude API**: Haiku 4.5 and Sonnet 4 routing
- **GitHub API**: Documentation pipeline pushes
- **Nextcloud API**: File storage for documentation
- **SSH Access**: Hardware monitoring (Kuromoon, CT 302)

### Planned Integrations
- **Ollama API**: Local LLM inference (Phase 38)
- **Obsidian Vault**: Knowledge base synchronization (Phase 22)
- **Alertmanager API**: Proactive system monitoring
- **HashiCorp Vault**: Secrets management integration
- **Discord API**: Gaming server notifications (separate bot: Mash)

## Memory System Architecture

### Current Implementation
- **Storage**: n8n Data Tables (gilgamesh_memory)
- **Capacity**: Last 20 messages
- **Structure**: message_id, chat_id, role, content, timestamp
- **Limitation**: Conversation-bound, no long-term knowledge retention

### Planned Enhancement (Phase 7E)
- **RAG System**: Vector embeddings for semantic search
- **Knowledge Base**: Obsidian vault integration
- **Context Expansion**: 20+ message conversations with relevant retrieval
- **Persistent Learning**: Cross-conversation knowledge retention

## Smart Routing Logic

### Current Routing (Working)
```python
def route_message(content, context):
    if len(content) < 100:
        return "haiku"  # Fast, simple queries
    elif contains_complex_keywords(content):
        return "sonnet"  # Reasoning, analysis
    elif has_conversation_context(context):
        return "sonnet"  # Contextual understanding
    else:
        return "haiku"   # Default to cheaper model
```

### Planned Hybrid Routing (Phase 41)
```python
def hybrid_route(content, context, local_capability):
    complexity_score = analyze_complexity(content)
    
    if complexity_score < 3:
        return "ollama_local"      # Simple tasks
    elif complexity_score < 7:
        return "claude_haiku"      # Medium complexity
    else:
        return "claude_sonnet"     # High complexity
```

## Known Issues & Constraints

### Technical Limitations
- **Telegram Formatting**: Plain text only — code blocks break JSON parsing
- **Progress Bars**: ASCII characters only (Unicode fails on mobile)
- **Cost Tracking**: gilgamesh_costs.cost_usd column always returns 0
- **Memory Scope**: Limited to 20 messages per conversation

### Performance Notes
- **Response Time**: Haiku ~2-3s, Sonnet ~5-8s
- **Token Limits**: 200K context window for both models
- **Rate Limits**: 5 requests/minute (Anthropic tier 1)

## Success Metrics

### Quantitative Goals
- **Response Quality**: 95% satisfaction compared to Claude Pro
- **Cost Reduction**: Monthly spending under $10
- **Response Time**: Average under 10 seconds
- **Uptime**: 99.5% availability

### Qualitative Goals
- **Knowledge Retention**: Multi-session conversation continuity
- **Document Generation**: Code, configs, and documentation artifacts
- **Visual Capabilities**: Image analysis and interpretation
- **Proactive Intelligence**: System health insights and recommendations

## Backup Strategy

If local LLM quality proves insufficient:
- **Hybrid Approach**: Keep Claude Pro for complex tasks
- **Still Save**: $10-15/month with smart routing
- **Gradual Migration**: Phase out Claude Pro over 6-12 months