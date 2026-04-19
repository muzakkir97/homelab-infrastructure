# Gilgamesh AI Agent - Feature Roadmap

> **Last Updated:** December 19, 2024  
> **Bot Handle:** @JhinGilgamesh_bot  
> **Platform:** Telegram + n8n + Claude API  
> **Version:** Phase 7D with Menu System

## Overview

Gilgamesh is a conversational AI agent designed specifically for homelab infrastructure management and personal assistance. Built on n8n workflow automation, it integrates with Claude API for natural language processing and maintains persistent memory through n8n Data Tables.

### Architecture Flow

```
Telegram User (@JhinGilgamesh_bot)
    ↓
n8n Webhook Trigger (CT 211: automation-n8n)
    ↓
Memory Retrieval (Data Tables: gilgamesh_memory)
    ↓ 
Smart Routing Decision (Haiku vs Sonnet)
    ↓
Claude API Request (with conversation context)
    ↓
Response Processing & Memory Update
    ↓
Telegram Response (text + inline keyboard)
```

## Current Features (✅ Working)

### Core Conversational AI

| Feature | Status | Details |
|---------|--------|---------|
| **Natural Language Chat** | ✅ Working | Full conversational AI via Claude API |
| **Conversation Memory** | ✅ Working | Last 20 messages stored in n8n Data Tables |
| **Smart Model Routing** | ✅ Working | Simple queries → Haiku 4.5, Complex → Sonnet 4 |
| **Web Search Integration** | ✅ Working | Real-time information via Claude's web_search tool |
| **Cost Tracking** | ✅ Working | Token usage logged to gilgamesh_costs table |
| **Multi-Update Type Support** | ✅ Working | Handles both messages and callback queries |

### Technical Specifications

| Component | Current Implementation |
|-----------|----------------------|
| **Telegram Bot Token** | Configured in n8n credentials |
| **Chat ID** | 510832696 (single user) |
| **Haiku Model** | claude-haiku-4-5-20251001 |
| **Sonnet Model** | claude-sonnet-4-20250514 |
| **Memory Storage** | n8n Data Tables (gilgamesh_memory) |
| **Cost Tracking** | n8n Data Tables (gilgamesh_costs) |
| **Hosting** | CT 211 (automation-n8n) |

### Smart Routing Logic

**Routing Decision Tree:**
```
User Message Length & Complexity
├── Simple (< 50 chars, basic questions)
│   └── Route to Haiku 4.5 ($0.003/1k tokens)
├── Medium (50-200 chars, specific queries)  
│   └── Route to Haiku 4.5 (cost-optimized)
└── Complex (> 200 chars, detailed requests)
    └── Route to Sonnet 4 ($0.015/1k tokens)
```

**Override Conditions:**
- Web search required → Always Sonnet 4
- Homelab API calls → Always Sonnet 4  
- Code generation → Always Sonnet 4
- Analysis/reasoning → Always Sonnet 4

### Memory System Architecture

**Data Tables Structure:**

**gilgamesh_memory:**
| Field | Type | Purpose |
|-------|------|---------|
| id | Integer | Unique message ID |
| timestamp | Datetime | Message timestamp |
| role | String | 'user' or 'assistant' |
| content | Text | Message content |
| model_used | String | Which Claude model processed |

**gilgamesh_costs:**
| Field | Type | Purpose |
|-------|------|---------|
| id | Integer | Unique cost entry ID |
| timestamp | Datetime | Request timestamp |
| model | String | Claude model used |
| input_tokens | Integer | Tokens sent to model |
| output_tokens | Integer | Tokens received |
| cost_usd | Float | Calculated cost in USD |

**Memory Management:**
- Maintains rolling 20-message window
- Older messages automatically pruned
- Context preserved across conversations
- No conversation state loss on n8n restart

## Inline Keyboard Menu System

### Main Menu Structure

```
🏠 Homelab | 🎮 Gaming | 🤖 Gilgamesh | 🛠️ Tools | ❓ Help
```

### Menu Implementation Status

| Menu Level | Feature | Status | Description |
|------------|---------|---------|-------------|
| **Main Menu** | Root Menu | ✅ Working | 5-button layout with category icons |
| **Homelab** | Status Check | ✅ Working | Proxmox API integration, container states |
| **Homelab** | Metrics Dashboard | 📋 Pending | CPU, memory, disk usage graphs |
| **Homelab** | Temperature Monitor | 📋 Pending | Hardware temperature readings |
| **Homelab** | Storage Overview | 📋 Pending | ZFS, LVM, NAS storage status |
| **Gaming** | Server Status | 📋 Pending | Terraria, Minecraft, Windrose status |
| **Gaming** | Player Counts | 📋 Pending | Active players across game servers |
| **Gaming** | Server Controls | 📋 Pending | Start/stop/restart game servers |
| **Gilgamesh** | Memory Stats | 📋 Pending | Conversation count, memory usage |
| **Gilgamesh** | Cost Analytics | 📋 Pending | Token usage, API costs breakdown |
| **Gilgamesh** | Model Selection | 📋 Pending | Force Haiku/Sonnet for session |
| **Tools** | Quick Commands | 📋 Pending | Common administrative shortcuts |
| **Tools** | Documentation | 📋 Pending | Link to GitHub docs, quick refs |
| **Tools** | System Shortcuts | 📋 Pending | Container restart, backup status |
| **Help** | Command List | 📋 Pending | All available commands and usage |
| **Help** | Tutorial Mode | 📋 Pending | Interactive homelab learning |

### Working Features Detail

**Homelab → Status (✅ Working):**
- Queries Proxmox API via HTTP request
- Authentication: `root@pam!gilgamesh` token
- Returns all container states (running/stopped)
- Node name: `muzakkir` (not kuromoon)
- Response format: Clean, tabulated container list

**Menu Navigation:**
- Callback query handling without Answer Callback node
- Inline keyboard updates after button press
- Seamless menu transitions
- Back/Home button support

## Slash Commands

### Current Slash Commands

| Command | Status | Purpose | Implementation |
|---------|---------|---------|----------------|
| **/start** | ✅ Working | Initialize bot, show main menu | Telegram built-in |
| **/update** | ✅ Working | Push session summary to GitHub | n8n workflow trigger |
| **/sync-docs** | 📋 Planned | Sync documentation pipeline | Future n8n workflow |
| **/status** | 📋 Planned | Quick homelab status check | Proxmox API shortcut |
| **/costs** | 📋 Planned | Show API usage and costs | Query gilgamesh_costs table |
| **/clear** | 📋 Planned | Clear conversation memory | Truncate memory table |
| **/backup** | 📋 Planned | Check backup job status | Parse Proxmox backup logs |
| **/alerts** | 📋 Planned | Show active alerts | Query Alertmanager API |

### Documentation Pipeline Commands

**/update Command Flow:**
```
User: /update
    ↓
n8n: Generate session summary via Claude
    ↓ 
n8n: Append to AI-CONTEXT.md session log
    ↓
n8n: Push to Nextcloud (cloud.najhin-gaming.com)
    ↓
n8n: Commit to GitHub (homelab-infrastructure)
    ↓
Telegram: Confirmation message
```

**Current /update Implementation:**
- ✅ Session summary generation
- ✅ AI-CONTEXT.md appending  
- ✅ Nextcloud file update
- ✅ GitHub repository sync
- ❌ Section-aware merging (appends to Session Log only)

**/sync-docs (Planned):**
- Bidirectional sync between Nextcloud and GitHub
- Full documentation pipeline trigger
- Version conflict resolution
- Multiple file format support

## Integration Architecture

### Proxmox API Integration

**Connection Details:**
- **Endpoint:** https://192.168.10.5:8006/api2/json
- **Authentication:** Token (root@pam!gilgamesh)
- **Node Name:** muzakkir (critical - not kuromoon)
- **SSL Verification:** Disabled (self-signed certificate)

**Current API Calls:**
- **Container Status:** `/nodes/muzakkir/lxc` - List all LXC containers
- **Node Status:** `/nodes/muzakkir/status` - System resources

**Planned API Integration:**
- Container control (start/stop/restart)
- Resource monitoring (CPU, RAM, disk)
- Backup job status queries
- Storage pool monitoring
- Network statistics

### GitHub Integration

**Repository:** homelab-infrastructure
**Branch:** main
**Credentials:** Fine-grained PAT (muzakkir97)

**Current Operations:**
- ✅ File updates (AI-CONTEXT.md)
- ✅ Commit with automated messages
- ✅ Push to remote repository

**Planned Operations:**
- Multi-file documentation updates
- Branch management for different docs
- Pull request automation
- Release tagging for major phases

### Nextcloud Integration

**Server:** cloud.najhin-gaming.com
**Path:** /AI-CONTEXT.md (root directory)
**Protocol:** WebDAV

**Current Operations:**
- ✅ File content updates
- ✅ WebDAV PUT operations
- ✅ Conflict detection

**Planned Operations:**
- Multiple file management
- Directory structure sync
- Version history preservation
- Automated backups of documentation

### Claude API Integration

**Provider:** Anthropic Claude API
**Models:** Haiku 4.5, Sonnet 4
**Features:** Conversation, web_search tool

**Request Structure:**
```json
{
  "model": "claude-haiku-4-5-20251001",
  "max_tokens": 4000,
  "temperature": 0.7,
  "messages": [
    {"role": "system", "content": "Homelab AI context..."},
    {"role": "user", "content": "User message"},
    {"role": "assistant", "content": "Previous response"},
    {"role": "user", "content": "Latest message"}
  ],
  "tools": [
    {
      "name": "web_search", 
      "description": "Search the web for current information"
    }
  ]
}
```

**Response Handling:**
- Text content extraction from API response
- Tool usage detection and processing
- Token counting for cost tracking
- Error handling for API failures

## Planned Features (📋 Roadmap)

### Phase 1: Menu System Completion

**Timeline:** Q1 2025
**Priority:** High

| Feature | Description | Technical Requirements |
|---------|-------------|------------------------|
| **Metrics Dashboard** | CPU, memory, disk graphs via Grafana API | Grafana API integration, image generation |
| **Temperature Monitor** | Hardware temperature readings | Proxmox sensor API, alert thresholds |
| **Storage Overview** | ZFS pools, LVM usage, NAS status | Multiple API endpoints, data aggregation |
| **Gaming Server Status** | Terraria, Minecraft, Windrose status | Docker API, game-specific queries |

### Phase 2: Proactive Intelligence

**Timeline:** Q2 2025
**Priority:** High

| Feature | Description | Technical Requirements |
|---------|-------------|------------------------|
| **Proactive Alerts** | Push notifications for system issues | Alertmanager webhook, alert routing |
| **Daily Summary Reports** | Automated homelab status summaries | Scheduled n8n workflow, multi-API data |
| **Predictive Warnings** | Disk space, certificate expiry alerts | Trend analysis, threshold monitoring |
| **Maintenance Reminders** | Update schedules, backup verification | Calendar integration, task scheduling |

### Phase 3: Advanced Interfaces

**Timeline:** Q3 2025
**Priority:** Medium

| Feature | Description | Technical Requirements |
|---------|-------------|------------------------|
| **Web Chat UI** | Homepage-embedded chat interface | React/Vue frontend, shared memory |
| **Mobile App** | Native Android/iOS Gilgamesh app | React Native, push notifications |
| **Voice Interface** | Voice commands via Telegram | Speech-to-text, voice response |
| **Multi-User Support** | Family member access with permissions | User management, role-based access |

### Phase 4: Hybrid AI Architecture

**Timeline:** Q4 2025
**Priority:** Medium

| Feature | Description | Technical Requirements |
|---------|-------------|------------------------|
| **Ollama Integration** | Local LLM on RX 6700 XT via ROCm | Phase 11 completion, model deployment |
| **Hybrid Routing** | Local vs Cloud model selection | Latency testing, privacy classification |
| **Model Switching** | Dynamic model selection based on context | Performance monitoring, cost optimization |
| **Local Privacy Mode** | Sensitive queries stay on-premises | Data classification, routing rules |

## Technical Constraints & Limitations

### Known Issues

| Issue | Impact | Workaround | Resolution Plan |
|-------|--------|------------|-----------------|
| **Plain Text Only** | No code blocks in Telegram responses | Use text formatting alternatives | Investigate Telegram API alternatives |
| **Single User** | Bot limited to one Telegram account | Use shared account | Implement multi-user authentication |
| **Memory Limit** | Only 20 message rolling window | Context summarization | Implement conversation archival |
| **No File Upload** | Cannot process document attachments | Use /update for text content | Plan file handling workflow |
| **API Rate Limits** | Claude API throttling during high usage | Message queuing | Implement request throttling |

### Architecture Limitations

**n8n Constraints:**
- Data Tables have schema caching bugs (require recreation)
- Expression scoping issues require Code nodes
- Single webhook per bot (all update types)
- Limited error handling for API failures

**Telegram Constraints:**
- Message length limits (4096 characters)
- Inline keyboard button limits (8 per row, 100 total)
- No native file upload handling in current workflow
- Single bot token per Telegram account

**API Dependencies:**
- Claude API availability and pricing changes
- Proxmox API authentication token expiry
- GitHub rate limiting on repository operations
- Nextcloud WebDAV connection reliability

## Security Considerations

### Authentication & Authorization

**Current Security:**
- Telegram bot token stored in n8n credentials
- Single authorized user (Chat ID validation)
- Proxmox API token with limited scope
- GitHub fine-grained PAT with repository access only

**Security Improvements Planned:**
- Multi-factor authentication for sensitive operations
- Role-based access control for different users
- API token rotation automation
- Audit logging for all operations

### Data Privacy

**Current Approach:**
- Conversation data stored locally in n8n
- No data sent to third parties except Claude API
- Memory purged after 20 messages
- Cost data retained for analytics

**Privacy Enhancements:**
- Option for local-only processing (Ollama)
- Data retention policy configuration
- Export/delete personal data functionality
- Conversation encryption at rest

## Performance & Monitoring

### Current Metrics

| Metric | Current Status | Target |
|--------|----------------|---------|
| **Response Time** | ~2-3 seconds | < 2 seconds |
| **Uptime** | 99%+ (n8n dependent) | 99.9% |
| **API Costs** | ~$5-10/month | < $15/month |
| **Memory Usage** | 20 messages max | Configurable |

### Monitoring Implementation

**Current Monitoring:**
- n8n workflow execution logs
- Claude API response tracking
- Cost accumulation in database
- Basic error alerting via Telegram

**Enhanced Monitoring (Planned):**
- Grafana dashboard for Gilgamesh metrics
- Alertmanager integration for failures
- Performance trend analysis
- User satisfaction tracking

## Development Workflow

### Current Development Process

1. **Feature Design** - Document in AI-CONTEXT.md
2. **n8n Workflow Update** - Modify existing workflow nodes
3. **Testing** - Manual testing via Telegram
4. **Documentation Update** - Update this roadmap
5. **Deployment** - n8n auto-saves, immediate deployment

### Planned Improvements

**Version Control:**
- n8n workflow version control via GitHub
- Staging environment for testing
- Automated rollback capability
- Change approval process

**Testing Framework:**
- Automated testing of critical workflows
- Mock API responses for testing
- Performance regression testing
- User acceptance testing scenarios

---

*This roadmap represents the current state and planned evolution of the Gilgamesh AI agent. Features marked as "Pending" are prioritized based on user needs and technical feasibility.*