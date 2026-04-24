# Gilgamesh AI Agent - Feature Roadmap

## Current Architecture

### Core Components
```
Telegram Bot (@JhinGilgamesh_bot)
         ↓
n8n Workflow Engine (CT 211)
         ↓
Claude API (Haiku 4.5 / Sonnet 4)
         ↓
Response + Memory Storage
```

### Data Flow
```
User Message → Telegram → n8n → Memory Retrieval → Claude API → Response
                                     ↓
                              Save to Data Tables
                                     ↓
                              Cost Tracking + Analytics
```

## Current Features (Status: WORKING)

### 🤖 Core Intelligence
- **Smart Model Routing:**
  - Simple queries → Claude Haiku 4.5 (`claude-haiku-4-5-20251001`)
  - Complex queries → Claude Sonnet 4 (`claude-sonnet-4-20250514`)
  - Automatic complexity detection based on message content
- **Conversation Memory:**
  - Last 20 messages stored in n8n Data Tables
  - Context preservation across sessions
  - Memory clearing via `/clear` command
- **Web Search Integration:**
  - Real-time information via Claude's web_search tool
  - Current events, technical documentation, news
- **Cost Tracking:**
  - Token usage monitoring (input/output)
  - Estimated cost calculation ($3/$15 per 1M tokens for Sonnet)
  - Monthly spend tracking (Note: cost_usd column bug - always returns 0)

### 💬 Telegram Interface
- **Chat ID:** 518832696
- **Bot Username:** @JhinGilgamesh_bot
- **Message Handling:** Plain text only (complex formatting breaks JSON parsing)
- **Response Format:** Markdown support, ASCII progress bars for mobile compatibility

### ⌨️ Slash Commands (Status: COMPLETE)

| Command | Function | Implementation Status |
|---------|----------|----------------------|
| `/help` | Command reference and menu guide | ✅ Working |
| `/clear` | Clear conversation memory | ✅ Working |
| `/memory` | View recent conversation history | ✅ Working |
| `/cost` | Display token usage and costs | ✅ Working |
| `/alerts` | Show active Alertmanager alerts | ✅ Working |
| `/backup` | Display last backup times | ✅ Working |
| `/update` | Push session summary to docs | ✅ Working |
| `/sync-docs` | Regenerate all documentation | ✅ Working |

### 🎛️ Inline Keyboard Menu System (Status: COMPLETE)

#### Main Menu
```
[ 🏠 Homelab ] [ 🎮 Gaming ] [ 🤖 Gilgamesh ]
[    🛠️ Tools    ] [    ❓ Help    ]
```

#### Homelab Submenu
```
[ 📊 Status ] [ 📈 Metrics ] [ 🌡️ Temps ] [ 💾 Storage ]
[              🔙 Back              ]
```

**Status:** Complete monitoring integration
- **Status:** Container status, uptime, autostart state
- **Metrics:** CPU usage, memory usage, load averages
- **Temps:** CPU/GPU temperatures via lm-sensors on Kuromoon
- **Storage:** ZFS pool usage, disk space, backup storage

#### Gaming Submenu
```
Terraria: ✅ Running (1 players)
Minecraft: ⏹️ Stopped
Windrose: ✅ Running (0 players)

[              🔙 Back              ]
```

**Status:** Real-time Docker container monitoring on CT 302

#### Gilgamesh Submenu (Information Panel)
```
🤖 Gilgamesh AI Agent
Version: Phase 7D + 15
Memory: 20 messages
Models: Haiku 4.5 + Sonnet 4
Platform: n8n + Claude API

[              🔙 Back              ]
```

#### Tools Submenu (Quick Links)
```
[ 📊 Grafana ] [ 🔧 Proxmox ] [ 📁 Nextcloud ]
[ 🔒 Vault ] [ 📝 n8n ] [ 🏠 Homepage ]
[              🔙 Back              ]
```

**Status:** Static link collection to main services

#### Help Panel
```
📖 Gilgamesh Help

💬 Chat: Send any message
⌨️ Commands: Use / commands
🎛️ Menu: Use buttons below
🧠 Memory: Last 20 messages saved
🔍 Web Search: Real-time info available

Available Commands:
/help /clear /memory /cost /alerts /backup
/update /sync-docs

[              🔙 Back              ]
```

## Memory System

### Storage Implementation
- **Platform:** n8n Data Tables
- **Table:** `gilgamesh_memory`
- **Schema:**
  ```json
  {
    "id": "integer (auto-increment)",
    "user_id": "string (chat_id)",
    "message": "text (user input)",
    "response": "text (bot response)",
    "timestamp": "datetime",
    "model": "string (haiku/sonnet)",
    "tokens_input": "integer",
    "tokens_output": "integer"
  }
  ```
- **Retention:** Last 20 messages per user
- **Cleanup:** Automatic via LIMIT in queries

### Cost Tracking
- **Table:** `gilgamesh_costs`
- **Issue:** `cost_usd` column always returns 0 (pre-existing bug)
- **Workaround:** Manual calculation using token counts × rates
- **Rates:** 
  - Haiku: $0.25/$1.25 per 1M tokens (input/output)
  - Sonnet: $3.00/$15.00 per 1M tokens (input/output)

## Documentation Pipeline

### Architecture
```
Session Summary → Claude API → Multi-file Generation
                      ↓
              Nextcloud + GitHub Push
```

### Workflow Triggers

#### /update Command
- **Purpose:** Daily session summaries
- **Files Updated:** 3
  - AI-CONTEXT.md (append session log)
  - changelog.md (add phase completion)
  - troubleshoot.md (add new issues/resolutions)
- **Webhook:** `doc-update`
- **Processing:** Merge session data into existing content

#### /sync-docs Command
- **Purpose:** Full documentation regeneration
- **Files Updated:** 7
  - AI-CONTEXT.md, changelog.md, troubleshoot.md (from /update)
  - README.md, roadmap.md, current-state.md, service-catalog.md
- **Webhook:** `doc-sync`
- **Processing:** Complete regeneration from AI-CONTEXT.md

### Integration Points
- **Nextcloud:** File storage via app password authentication
- **GitHub:** Direct API push to muzakkir97/homelab-infrastructure
- **Claude API:** Documentation processing and generation

## API Integrations

### Proxmox VE API
- **Authentication:** Token `root@pam!gilgamesh`
- **Node:** `muzakkir` (not `kuromoon` - hostname vs node name)
- **Endpoints:**
  - `/nodes/{node}/status` - System metrics
  - `/nodes/{node}/lxc` - Container status
  - `/nodes/{node}/storage` - Storage usage
- **Usage:** Status monitoring, metrics collection

### SSH Integrations
- **Kuromoon (Proxmox Host):** 192.168.10.5:22
  - Authentication: SSH key from CT 211
  - Purpose: Temperature monitoring, hardware stats
  - Command: `sensors` for CPU/GPU temperatures
- **CT 302 (Gaming Wings):** Password authentication
  - Purpose: Docker container monitoring
  - Command: `docker ps --format table` for game server status

## Technical Constraints

### Telegram Limitations
- **Message Format:** Plain text only
- **Formatting Issues:** Code blocks and backticks break JSON parsing in Claude API requests
- **Progress Bars:** ASCII characters only (= and -) - Unicode blocks don't render on mobile
- **File Uploads:** Not currently supported in workflow

### n8n Workflow Constraints
- **Single Webhook:** All Telegram update types (message + callback_query) must use one trigger
- **Data Tables:** Schema caching bug requires full table recreation for changes
- **Expression Scoping:** Use Code nodes with `this.helpers.httpRequest` for complex operations

### Claude API Limitations
- **Model Names:** Must use exact IDs (e.g., `claude-haiku-4-5-20251001`)
- **Web Search:** Response filtering required (`type === "text"`) for partial captures
- **Token Limits:** 200K input for Sonnet, 100K for Haiku

## Planned Features Roadmap

### Phase 1: Enhanced Intelligence (Weeks 1-4)
**Target:** Reduce API costs and improve capabilities

#### Phase 38: Ollama + ROCm Integration ✅ COMPLETE
- **Status:** Deployed VM 400 with RX 6700 XT passthrough
- **Models:** qwen2.5:14b (9GB), llama3.2:3b (2GB)
- **Performance:** ~20-50 tokens/second, 12GB VRAM capacity
- **Integration Point:** ollama.najhin-gaming.com API endpoint

#### Phase 41: Hybrid Routing Engine (HIGH PRIORITY)
**Goal:** Route 70% of queries to local Ollama, 30% to Claude API
- **Implementation:**
  ```
  Query → Complexity Analysis → Route Decision
           ↓                    ↓
       Simple/Fast          Complex/Precise
           ↓                    ↓
      Ollama (Local)        Claude API
           ↓                    ↓
       Response Merge ← Quality Check
  ```
- **Cost Impact:** Reduce monthly API spend from $30-40 to $5-10
- **Quality Assurance:** Compare outputs, fallback to Claude for critical queries

### Phase 2: Extended Memory System (Weeks 5-8)

#### Phase 7E: RAG Memory Extension
**Goal:** 20+ message conversation retention via vector database
- **Architecture:**
  ```
  Message → Embedding → Vector Store → RAG Retrieval → Context
  ```
- **Implementation Options:**
  - Qdrant on CT 215 (vector database)
  - Chroma (lightweight alternative)
  - LangChain integration via n8n
- **Memory Tiers:**
  - Short-term: Last 20 messages (current Data Tables)
  - Long-term: Vector embeddings for extended context
  - Knowledge base: Obsidian vault integration

#### Obsidian Integration
- **Vault Location:** C:\Users\muzak\Nextcloud\Obsidian\second-brain
- **Sync Method:** Nextcloud auto-sync
- **Knowledge Access:** Query homelab documentation, procedures, lessons learned
- **Content Types:** Session summaries, troubleshooting guides, infrastructure docs

### Phase 3: Advanced Capabilities (Weeks 9-12)

#### Phase 7F: Artifact Generation
**Goal:** Generate code, configurations, and documentation files
- **Capabilities:**
  - Bash scripts for common tasks
  - Docker Compose files
  - Proxmox LXC configurations
  - n8n workflow exports
  - Terraform configurations
- **Delivery:** File attachments via Telegram, auto-push to GitHub

#### Phase 7G: Vision API Integration
**Goal:** Analyze screenshots, diagrams, and infrastructure images
- **Use Cases:**
  - Screenshot analysis for troubleshooting
  - Network diagram interpretation
  - Grafana dashboard review
  - Error message OCR
- **Implementation:** Claude Vision API for images, local OCR for text extraction

#### Phase 7H: Document Processing
**Goal:** Upload and analyze PDF/text documents
- **Document Types:**
  - Hardware manuals
  - Software documentation
  - Configuration guides
  - Troubleshooting documents
- **Processing:** Text extraction, summarization, Q&A over document content

### Phase 4: Proactive Intelligence (Weeks 13-16)

#### Proactive Monitoring & Alerts
**Goal:** AI-driven infrastructure health analysis
- **Features:**
  - Predictive failure detection
  - Capacity planning recommendations
  - Performance optimization suggestions
  - Security vulnerability scanning
- **Integration:** Prometheus metrics → AI analysis → Proactive recommendations

#### Daily Infrastructure Summary
**Goal:** Automated daily status reports
- **Content:**
  - System health overview
  - Performance trends
  - Security status
  - Backup verification
  - Recommended actions
- **Delivery:** Morning Telegram message with executive summary

#### Phase 7I: Quality Assurance Testing
**Goal:** Validate Gilgamesh outputs against Claude Pro baseline
- **Testing Matrix:**
  - Technical accuracy
  - Response quality
  - Processing speed
  - Cost efficiency
- **Success Criteria:** 90% quality parity with 70% cost reduction

#### Phase 7J: Full Migration
**Goal:** Replace claude.ai dependency completely
- **Migration Steps:**
  1. Feature parity verification
  2. User experience optimization
  3. Performance tuning
  4. Claude Pro subscription cancellation
- **Backup Plan:** Hybrid approach if quality insufficient

## Web Interface Development

### Homepage Integration (Future)
**Goal:** Embedded chat UI sharing memory with Telegram bot
- **Architecture:**
  ```
  Telegram Bot ← Shared Memory → Web Chat UI
                      ↓
              n8n Workflow Engine
  ```
- **Benefits:**
  - Desktop-friendly interface
  - File upload capabilities
  - Better formatting support
  - Session continuity across platforms

### Authentication
- **Current:** Telegram-only (single user)
- **Future:** Cloudflare Access integration for web UI
- **Security:** Maintain single-user restriction, add 2FA

## Agent Ecosystem Integration

### Fate Grand Order Agent Framework
**Theme:** Homelab agents named after Fate/GO servants

#### Active Agents
- **Gilgamesh (Archer):** Personal AI Assistant ✅ Active
- **Caster (Da Vinci):** Knowledge Curator (n8n documentation pipeline) ✅ Active
- **Archer (EMIYA):** Infrastructure Translator (n8n workflows) ✅ Active

#### Planned Agents (Priority Order)

##### MERLIN (Caster) - Reminder Agent - #1 PRIORITY
**Noble Phantasm:** *Garden of Avalon* (Maintenance Windows)
- **Critical Need:** User memory issues require proactive reminders
- **Capabilities:**
  - SSL certificate renewal alerts (expires in X days)
  - Backup restoration test reminders
  - Memory limit warnings (Loki/Prometheus OOM predictions)
  - Scheduled maintenance windows
  - Infrastructure health checks
- **Implementation:** Cron-based n8n workflows with Telegram notifications

##### Mash Kyrielight (Shielder) - Gaming Server Manager
**Noble Phantasm:** *Lord Camelot* (Team Protection)
- **Platform:** Discord bot (separate from Gilgamesh)
- **Gaming Integration:** Phases 59-64
- **Capabilities:**
  - Discord commands: `!start`, `!stop`, `!status`
  - Player join/leave announcements
  - Auto-shutdown idle servers
  - Game update notifications
  - Personality: "Senpai, PlayerX just joined Windrose!"
- **Design Principle:** Keep gaming separate from homelab administration

### Future Agent Roles
- **Guardian:** Security monitoring and threat detection
- **Nexus:** Cross-platform automation and integration
- **Scribe:** Auto-documentation and knowledge management
- **Midas:** Cost tracking and optimization
- **Oracle:** Predictive intelligence and capacity planning

## Cost Analysis & Projections

### Current Costs (Monthly)
```
Claude Pro Subscription:    $20/month
Claude API Usage:          $10-20/month
Total Current:             $30-40/month ($360-480/year)
```

### Target Costs (Post-Migration)
```
Local GPU Power (estimated): $3-5/month
Occasional API Usage:        $2-5/month
Total Target:               $5-10/month ($60-120/year)
Annual Savings:             $300-360
```

### Cost Breakdown by Phase
- **Phase 38 (Complete):** GPU infrastructure ready, power costs only
- **Phase 41:** 70% local routing → ~$10-15/month savings
- **Phase 7E-7H:** Feature parity → enable full migration
- **Phase 7J:** Complete migration → full $300-360/year savings

### ROI Analysis
- **Hardware Investment:** RX 6700 XT (already owned)
- **Power Cost:** ~$50-60/year (GPU + increased server utilization)
- **Net Savings:** $250-300/year
- **Payback Period:** Immediate (hardware already owned)

## Success Criteria

### Technical Milestones
- ✅ 20-message conversation memory (Data Tables)
- ✅ Smart model routing (Haiku/Sonnet)
- ✅ Inline keyboard menu system
- ✅ Documentation pipeline automation
- ✅ Local LLM infrastructure (Ollama + ROCm)
- 🔄 Extended memory via RAG (20+ messages)
- 🔄 70% local routing with quality assurance
- 🔄 Vision and document processing
- 🔄 Proactive monitoring and alerts

### Quality Benchmarks
- **Response Accuracy:** 90% parity with Claude Pro
- **Processing Speed:** <10 seconds for complex queries
- **Uptime:** 99.5% availability
- **User Satisfaction:** Seamless experience vs. claude.ai

### Cost Targets
- **Monthly Spend:** <$10/month by August 2026
- **Annual Savings:** $300-360/year
- **Backup Plan:** Hybrid approach if full migration insufficient

## Migration Timeline

### 16-Week Plan (April 2026 - August 2026)

**Weeks 1-4: Foundation**
- ✅ Phase 38: Ollama + ROCm deployment
- ✅ Phase 22: Obsidian Knowledge Base
- 🔄 Phase 41: Hybrid routing implementation
- 🔄 Set $10 API spend limit (safety measure)

**Weeks 5-8: Intelligence**
- 📋 Phase 7E: RAG memory system
- 📋 Obsidian integration
- 📋 Extended context testing

**Weeks 9-12: Capabilities**
- 📋 Phase 7F: File generation
- 📋 Phase 7G: Vision API
- 📋 Phase 7H: Document upload
- 📋 Web UI development

**Weeks 13-16: Refinement**
- 📋 Phase 7I: Quality assurance
- 📋 Performance optimization
- 📋 Phase 7J: Full migration
- 📋 Claude Pro cancellation

### Risk Mitigation
- **Quality Insufficient:** Maintain hybrid approach, still save $10-15/month
- **Performance Issues:** Ollama model optimization, hardware upgrades
- **Feature Gaps:** Gradual migration, maintain Claude API for specific use cases
- **User Experience:** Extensive testing period before full switch