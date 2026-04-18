# Phase 7B: n8n Workflow Automation

> **Status:** ✅ Complete  
> **Completed:** April 2, 2026  
> **Duration:** ~4 hours

---

## 📋 Overview

Deployment of n8n workflow automation platform as the foundation for AI-powered automation and the Gilgamesh Telegram bot.

---

## 🎯 Objectives

- [x] Deploy n8n as LXC container
- [x] Configure external access via subdomain
- [x] Set up Nginx Proxy Manager routing
- [x] Prepare for Gilgamesh bot integration

---

## 🏗️ Implementation

### Container Specification

| Property | Value |
|----------|-------|
| **CTID** | 211 |
| **Hostname** | automation-n8n |
| **IP Address** | 192.168.30.211 |
| **VLAN** | 30 (Services) |
| **CPU** | 2 cores |
| **RAM** | 2 GB |
| **Disk** | 20 GB (vmpool-fast) |
| **OS** | Ubuntu 22.04 |

### Installation Steps

1. **Create LXC Container**
   ```bash
   # On Proxmox
   pct create 211 local:vztmpl/ubuntu-22.04-standard_22.04-1_amd64.tar.zst \
     --hostname automation-n8n \
     --memory 2048 \
     --cores 2 \
     --rootfs vmpool-fast:20 \
     --net0 name=eth0,bridge=vmbr0,tag=30,ip=192.168.30.211/24,gw=192.168.30.1 \
     --features nesting=1 \
     --unprivileged 1 \
     --onboot 1
   ```

2. **Install Node.js & n8n**
   ```bash
   # Update system
   apt update && apt upgrade -y
   
   # Install Node.js 18.x
   curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
   apt install -y nodejs
   
   # Install n8n globally
   npm install -g n8n
   ```

3. **Create systemd Service**
   ```bash
   cat > /etc/systemd/system/n8n.service << 'EOF'
   [Unit]
   Description=n8n workflow automation
   After=network.target
   
   [Service]
   Type=simple
   User=root
   Environment="N8N_HOST=0.0.0.0"
   Environment="N8N_PORT=5678"
   Environment="N8N_PROTOCOL=https"
   Environment="WEBHOOK_URL=https://n8n.najhin-gaming.com/"
   Environment="N8N_EDITOR_BASE_URL=https://n8n.najhin-gaming.com/"
   ExecStart=/usr/bin/n8n
   Restart=always
   RestartSec=10
   
   [Install]
   WantedBy=multi-user.target
   EOF
   
   systemctl daemon-reload
   systemctl enable n8n
   systemctl start n8n
   ```

4. **Configure Nginx Proxy Manager**
   - Domain: n8n.najhin-gaming.com
   - Forward Hostname: 192.168.30.211
   - Forward Port: 5678
   - SSL: Let's Encrypt
   - Websockets Support: Enabled

---

## 🌐 External Access

### Subdomain Configuration

| Property | Value |
|----------|-------|
| **Subdomain** | n8n.najhin-gaming.com |
| **DNS** | Cloudflare (proxied) |
| **SSL** | Let's Encrypt via NPM |
| **Websockets** | Enabled |

### Security (Added in Phase 7D-Sec)

- Cloudflare Access protection
- Email OTP authentication
- Same model as Grafana

---

## 🔧 Configuration

### Environment Variables

| Variable | Value | Purpose |
|----------|-------|---------|
| N8N_HOST | 0.0.0.0 | Listen on all interfaces |
| N8N_PORT | 5678 | Default port |
| N8N_PROTOCOL | https | External protocol |
| WEBHOOK_URL | https://n8n.najhin-gaming.com/ | Webhook base URL |
| N8N_EDITOR_BASE_URL | https://n8n.najhin-gaming.com/ | Editor URL |

### Data Location

- **Database:** SQLite at ~/.n8n/database.sqlite
- **Workflows:** Stored in database
- **Credentials:** Encrypted in database

---

## ✅ Verification

| Check | Status |
|-------|--------|
| Container running | ✅ |
| n8n service active | ✅ |
| Web UI accessible (internal) | ✅ |
| Web UI accessible (external) | ✅ |
| SSL certificate valid | ✅ |
| Webhook URL working | ✅ |

---

## 📝 Notes

### Why n8n?

- Visual workflow builder
- Self-hosted (data privacy)
- Telegram integration built-in
- HTTP Request node for API calls
- Data Tables for persistence
- Free tier sufficient for homelab

### Key Learnings

1. **Webhook URL must match external URL** — Set `WEBHOOK_URL` to the public subdomain
2. **Websockets required** — Enable in NPM for real-time updates
3. **nesting=1 required** — For some n8n features

---

## 🔗 Related

- **Next Phase:** [Phase 7C: Gilgamesh Telegram Bot](phase-7c-gilgamesh-bot.md)
- **Security:** [Phase 7D-Sec: Cloudflare Access for n8n](phase-7d-sec-cloudflare-access.md)

---

*Completed: April 2, 2026*
