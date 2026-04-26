# Gilgamesh AI Agent - Feature Roadmap & Development Plan

## Overview

Gilgamesh is a comprehensive Telegram-based AI agent designed to replace Claude Pro by August 2026, providing advanced homelab management, personal assistance, and documentation capabilities with hybrid local/cloud AI routing.

**Primary Goal:** Replace claude.ai with Gilgamesh by August 2026 (16 weeks)
**Cost Target:** Reduce from $30-40/month to $5-10/month
**Platform:** Telegram Bot (@JhinGilgamesh_bot)
**Current Status:** Phase 41 Complete - Hybrid routing active

## Agent Ecosystem Architecture

### Fate Grand Order Servant Theme
Homelab agents follow Fate/Grand Order servant naming with locked 9-agent final roster:

| Servant              | Class    | Role                                   | Platform      | Status    |
|----------------------|----------|----------------------------------------|---------------|-----------|
| Gilgamesh 👑         | Archer   | Personal AI Assistant                  | Telegram      | ✅ Active  |
| Da Vinci 🎨          | Caster   | Chief Intelligence Officer             | n8n/Nextcloud | ⚡ Partial |
| Midas 💰             | Caster   | CFO — Cost Tracking & Optimization     | n8n           | 📋 Planned |
| MERLIN 🔮            | Caster   | Reminders & Scheduler                  | n8n           | 📋 Planned |
| Guardian 🛡          | —        | Security Monitoring                    | n8n           | 📋 Planned |
| Mash Kyrielight 🛡️  | Shielder | Gaming Server Manager + Wellbeing      | Discord       | 📋 Planned |
| Nexus 🔗             | —        | Cross-platform Automation              | n8n           | 📋 Planned |
| Oracle 🔮            | —        | Predictive Intelligence                | n8n           | 📋 Planned |
| EMIYA 🏹             | Archer   | CTO — Infrastructure Engineer          | n8n           | 📋 Planned |

**Build Order (Confirmed):** Midas → MERLIN → Guardian → Mash → Nexus → Oracle → EMIYA

## Current Gilgamesh Features (Active)

### Core AI Architecture

```
Telegram Input → Smart Router → Model Selection
                      ↓
    ┌─────────────────┼─────────────────┐
    ▼                 ▼                 ▼
Ollama qwen3:14b   Haiku 4.5        Sonnet 4
(Primary/Local)    (Fallback)       (Complex)
    └─────────────────┼─────────────────┘
                      ▼
              Response Processing
                      ↓
              Memory Storage (n8n Data Tables)
                      ↓
              Telegram Response
```

### Smart Routing Logic (Phase 41)

**Route Decision Matrix:**
- **Ollama qwen3:14b (Primary):** Simple queries, general chat, basic homelab commands
- **Haiku 4.5 (Fallback):** When Ollama is unavailable or fails
- **Sonnet 4 (Complex):** Triggered by:
  - Messages over 50 words
  - Complex keywords: "analyze", "explain", "architecture", "troubleshoot"
  - Technical documentation requests
  - Multi-step problem solving

**Cost Optimization:**
- Ollama: $0 (local inference on VM 400 with RX 6700 XT)
- Haiku: $0.80/$1.00 per 1M input/output tokens
- Sonnet: $3.00/$15.00 per 1M input/output tokens

### Memory System

**Implementation:** n8n Data Tables
- **Conversation Memory:** Last 20 messages with timestamps
- **Cost Tracking:** Token usage by model with USD calculations
- **Session Context:** Maintains conversation flow across interactions

**Memory Tables:**
```
gilgamesh_memory:
├── user_id (518832696)
├── message_text
├── response_text
├── timestamp
└── message_type

gilgamesh_costs:
├── timestamp
├── model_used
├── input_tokens
├── output_tokens
├── cost_usd
└── query_type
```

### Slash Commands System

| Command    | Purpose           | Implementation Status | Description                                   |
|------------|-------------------|-----------------------|-----------------------------------------------|
| /help      | Quick reference   | ✅ Complete            | Shows all available commands and menu options |
| /clear     | Memory reset      | ✅ Complete            | Clears conversation memory (last 20 messages) |
| /memory    | View history      | ✅ Complete            | Shows recent conversation messages            |
| /cost      | Usage tracking    | ✅ Complete            | Displays token usage and estimated costs      |
| /alerts    | System status     | ✅ Complete            | Shows active Alertmanager alerts              |
| /backup    | Backup status     | ✅ Complete            | Last backup times for all containers          |
| /update    | Session summary   | ✅ Complete            | Merges session into docs (3 files)            |
| /sync-docs | Full regeneration | ✅ Complete            | Regenerates all documentation (7 files)       |

### Inline Keyboard Menu System

```
Main Menu
├── 🏠 Homelab
│   ├── 📊 Status (container health, uptime)
│   ├── 📈 Metrics (CPU, RAM, disk usage)
│   ├── 🌡️ Temps (system temperatures)
│   └── 💾 Storage (disk usage by pool)
├── 🎮 Gaming
│   ├── Terraria Server Status
│   ├── Minecraft Server Status
│   └── Windrose Server Status
├── 🤖 Gilgamesh
│   ├── 💬 Chat Stats
│   ├── 💰 Cost Tracking
│   └── 🧠 Memory Status
├── 🛠️ Tools
│   ├── 🔧 Quick Commands
│   ├── 📋 System Info
│   └── 🔄 Restart Services
└── ❓ Help
    ├── 📖 Command Reference
    └── 🆘 Emergency Contacts
```

**Menu Status:** All submenus working, keyboard navigation fully functional

### Documentation Pipeline Integration

**Workflow Integration:**
```
/update command → Da Vinci Documentation Pipeline
                          ↓
    Session Summary → AI-CONTEXT.md merge → Nextcloud + GitHub
                          ↓
    Automated version control and backup
```

**Pipeline Features:**
- **Session Documentation:** Automatic session summaries
- **Multi-file Updates:** AI-CONTEXT.md, changelog.md, troubleshoot.md
- **Version Control:** Automatic GitHub commits
- **Obsidian Integration:** Session notes to knowledge base

## Planned Gilgamesh Enhancements

### Phase 7E: Extended Memory & RAG (In Progress)

**Objective:** 20+ message conversation retention via RAG (Retrieval Augmented Generation)

**Technical Stack:**
- **Vector Database:** Qdrant on VM 400
- **Embeddings:** nomic-embed-text model
- **RAG Implementation:** n8n native RAG nodes
- **Storage:** Obsidian knowledge base integration

**Features:**
- Long-term conversation memory
- Knowledge base recall from Obsidian vault
- Context-aware responses based on historical conversations
- Personal knowledge integration

### Phase 7F: File Generation Capabilities

**Planned Features:**
- **Code Generation:** Scripts, configuration files, docker-compose files
- **Documentation:** README files, troubleshooting guides
- **Infrastructure as Code:** Terraform, Ansible playbooks
- **Template System:** Reusable templates for common tasks

### Phase 7G: Vision API Integration

**Capabilities:**
- **Screenshot Analysis:** Analyze Grafana dashboards, error screens
- **Diagram Processing:** Network diagrams, architecture drawings
- **Text Extraction:** OCR from images
- **Visual Troubleshooting:** Identify issues from visual clues

### Phase 7H: Document Upload Processing

**File Type Support:**
- **PDF Processing:** Technical documentation, manuals
- **Text Files:** Configuration files, logs
- **Markdown:** Documentation files
- **CSV/JSON:** Data analysis and processing

### Proactive Monitoring & Alerts

**Planned Features:**
```
Proactive Intelligence:
├── Daily Infrastructure Summary
├── Anomaly Detection Alerts  
├── Predictive Maintenance Warnings
├── Security Threat Notifications
├── Performance Optimization Suggestions
└── Backup Status Reports
```

**Integration Points:**
- Prometheus metrics analysis
- Alertmanager alert correlation
- Log analysis from Loki
- Trend analysis and predictions

### Web Chat UI Integration

**Architecture:**
```
Homepage Dashboard → Embedded Chat Widget
                           ↓
              Shared Memory with Telegram Bot
                           ↓
              Same AI Backend & Routing
```

**Benefits:**
- Unified experience across platforms
- Shared conversation history
- Web-based access for desktop use
- Integration with homelab dashboard

## AI Model Evolution Plan

### Current Model Configuration

| Model        | Size  | VRAM Usage | Primary Use           | Cost    |
|--------------|-------|------------|----------------------|---------|
| qwen3:14b    | 9.3GB | 12GB       | General queries      | Free    |
| llama3.2     | 3GB   | 4GB        | Lightweight tasks    | Free    |

### Future Model Upgrades

**Planned Additions:**
- **Qwen3:32b** (when VRAM allows): Enhanced reasoning
- **CodeQwen** variants: Specialized code generation
- **Vision Models:** Image analysis capabilities
- **Specialized Models:** Domain-specific tasks

### Hybrid Routing Evolution

**Advanced Routing Criteria (Planned):**
```
Query Analysis:
├── Complexity Score (0-10)
├── Domain Classification (technical/personal/creative)
├── Context Requirements (memory-intensive/stateless)
├── Performance Needs (speed/quality)
└── Cost Optimization (budget-aware routing)
```

## Integration Architecture

### Proxmox API Integration

**Current Capabilities:**
- Container status monitoring
- Resource usage queries
- Basic lifecycle operations (with approval gates)

**Planned Enhancements:**
- Automated container management
- Resource optimization suggestions
- Backup automation
- Performance monitoring

### GitHub Integration

**Current Features:**
- Documentation updates via /update and /sync-docs
- Automated commit and push

**Planned Features:**
- Issue creation for problems
- Automated documentation generation
- Code review assistance
- Repository monitoring

### Nextcloud Integration

**Current Features:**
- File storage for documentation
- WebDAV access for updates
- Obsidian vault synchronization

**Planned Features:**
- File sharing automation
- Backup verification
- Collaborative document editing
- Knowledge base expansion

## Agent Ecosystem Development Plan

### Da Vinci - Chief Intelligence Officer

**Stage 1 (Complete):** Documentation Pipeline
- AI-CONTEXT.md maintenance
- Session summary processing
- Multi-file documentation updates

**Stage 2 (Planned):** RAG & Knowledge Base
- Qdrant vector database implementation
- Knowledge retrieval across all agents
- Obsidian vault indexing and search
- Cross-agent knowledge sharing

**Stage 3 (Future):** Intelligence Analysis
- Security threat monitoring
- Performance trend analysis
- Predictive maintenance insights

### EMIYA - CTO Infrastructure Engineer

**10 Core Features:**
1. **VM/LXC Lifecycle Management** (approval-gated)
2. **Alert Translation** (Alertmanager → Plain English)
3. **Container Updates** (Docker + apt, approval-gated)
4. **Proactive Monitoring** (anomaly detection)
5. **Security Management** (threat detection)
6. **Performance Optimization** (bottleneck identification)
7. **Backup Verification** (integrity checking)
8. **Change Management** (audit logging)
9. **Service Health Reporting** (daily summaries)
10. **Pre-flight Checks** (via Da Vinci consultation)

**Deployment Phases:**
- 24.1: Foundation (API + SSH + approval gates)
- 24.2: Alert Translation
- 24.3: Container Updates
- 24.4: Proactive Monitoring
- 24.5: Security Management
- 24.6-24.8: Advanced Features

### Midas - CFO Cost Optimization

**First Agent to Build (Build Order #1)**

**Core Features:**
- Real-time cost tracking across all services
- Budget alerts and notifications
- Resource optimization recommendations
- Cost projection and analysis
- ROI calculations for infrastructure changes

**Integration Points:**
- Cloud provider APIs
- Utility monitoring
- Hardware depreciation tracking
- Subscription management

### MERLIN - Reminder & Scheduler

**Build Order #2**

**Features:**
- SSL certificate renewal reminders
- Backup restoration test scheduling
- Maintenance window planning
- Infrastructure health check scheduling
- Proactive system maintenance alerts

### Mash - Gaming Discord Bot

**Build Order #4**

**Discord Bot Features:**
```
Gaming Commands:
├── !start <server> (Start game server)
├── !stop <server> (Stop game server)  
├── !status (Server status display)
├── !players (Current players list)
└── !schedule (Game night planning)
```

**Personality:** Mash Kyrielight themed
- "Senpai, PlayerX just joined Terraria!"
- Wellbeing reminders during long gaming sessions
- Friendly servant personality

## Success Metrics & Quality Assurance

### Performance Targets

| Metric                    | Current | Target   | Status |
|---------------------------|---------|----------|--------|
| Response Time (Ollama)    | 2-3s    | <2s      | 🟡      |
| Response Time (Claude)    | 1-2s    | <1.5s    | ✅      |
| Memory Accuracy           | 90%     | 95%      | 🟡      |
| Cost per Interaction      | $0.05   | <$0.02   | ✅      |
| Uptime                    | 98%     | 99.5%    | 🟡      |

### Quality Assurance Plan (Phase 7I)

**Testing Framework:**
- Side-by-side comparison with Claude Pro
- Response quality scoring (1-10)
- Accuracy verification for homelab queries
- User experience evaluation
- Cost-effectiveness analysis

**Validation Areas:**
- Technical documentation generation
- Problem diagnosis accuracy
- Code generation quality
- Conversation coherence
- Knowledge retention

### Migration Strategy (Phase 7J)

**Gradual Migration Plan:**
1. **Month 1-2:** Parallel usage (Gilgamesh + Claude Pro)
2. **Month 3:** Primary usage shift to Gilgamesh
3. **Month 4:** Claude Pro as backup only
4. **Month 5+:** Full migration (optional Claude Pro cancellation)

**Rollback Plan:**
- Keep Claude Pro subscription during transition
- Maintain dual-access during critical projects
- Quality threshold gates for full migration

## Known Limitations & Constraints

### Technical Constraints

| Limitation                | Impact        | Workaround/Solution           |
|---------------------------|---------------|-------------------------------|
| Telegram message format   | High          | Plain text only, no code blocks |
| Ollama context window     | Medium        | RAG implementation for history |
| GPU memory limits         | Medium        | Model size optimization       |
| n8n workflow complexity   | Medium        | Modular workflow design       |

### Cost Projections

| Component      | Current      | Target       | Annual Savings |
|----------------|--------------|--------------|----------------|
| Claude Pro     | $20/month    | $0/month     | $240          |
| Claude API     | $10-20/month | $5-10/month  | $60-120       |
| **Total**      | $30-40/month | $5-10/month  | $300-360      |

### Backup Plan

**If Quality Insufficient:**
- Retain Claude Pro for complex tasks
- Use Gilgamesh for routine operations
- Hybrid approach saves $15-25/month
- Continue development for future full migration

This comprehensive roadmap positions Gilgamesh as a capable replacement for Claude Pro while providing advanced homelab automation, cost optimization, and personal assistance capabilities through a coordinated multi-agent ecosystem.