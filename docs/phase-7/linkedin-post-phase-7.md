# LinkedIn Post — Phase 7: Nextcloud Deployment

---

## Post Content

🚀 **Homelab Update: Self-Hosted Cloud Storage is Live!**

Just completed Phase 7 of my homelab infrastructure project — deploying **Nextcloud** as a Google Drive replacement.

**What I Built:**
☁️ Nextcloud Hub 26 on Debian 12 LXC container
🔒 External access via **Cloudflare Tunnel** (zero port forwarding!)
📱 Mobile app + Desktop sync client connected
📊 Full monitoring integration with Prometheus & Uptime Kuma

**Technical Stack:**
• Apache 2.4 + PHP 8.2 + MariaDB 10.11
• APCu memory caching for performance
• Cloudflare Tunnel for secure external access
• Container autostart with boot order priority

**Key Learning:**
Cloudflare Access (Zero Trust) doesn't work with Nextcloud mobile apps — the OAuth flow can't complete outside a browser. Solution? Use Cloudflare Tunnel for connectivity + Nextcloud's built-in brute-force protection for security.

**Infrastructure Progress:**
✅ 10 containers now running with autostart
✅ 5-VLAN network fully operational
✅ Full monitoring stack (Prometheus/Grafana/Loki/Alertmanager)
✅ Gaming platform (Pterodactyl + Terraria)
✅ Cloud storage (Nextcloud)

**What's Next:**
Phase 7A: Automated backup strategy — because data without backups isn't really data.

Building enterprise-grade infrastructure one container at a time. 💪

🔗 Full documentation: https://github.com/muzakkir97/homelab-infrastructure

#homelab #devops #cloudengineering #selfhosted #nextcloud #cloudflare #infrastructure #linux #proxmox #networking #careerchange

---

## Suggested Image

Screenshot of Nextcloud dashboard or mobile app showing successful sync.

---

## Hashtags (Copy-Paste Ready)

```
#homelab #devops #cloudengineering #selfhosted #nextcloud #cloudflare #infrastructure #linux #proxmox #networking #careerchange
```

---

*Created: March 8, 2026*
