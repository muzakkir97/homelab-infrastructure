# 🏗️ Homelab Infrastructure Project

> Enterprise-grade homelab built for learning cloud engineering, cybersecurity, and infrastructure management

[![Status](https://img.shields.io/badge/Status-Active-success)](/)
[![Proxmox](https://img.shields.io/badge/Proxmox-VE%208.x-orange)](/)
[![Phase](https://img.shields.io/badge/Phase-7%20Complete-green)](/)
[![Containers](https://img.shields.io/badge/Containers-10%20LXC-blue)](/)
[![VLANs](https://img.shields.io/badge/VLANs-5%20Segments-purple)](/)

---

## 👤 About Me

**Name**: Muzakkir Kholil  
**Role**: Customer Service Engineer @ F-Secure  
**Location**: Petaling Jaya, Malaysia  
**Domain**: [najhin-gaming.com](https://najhin-gaming.com)  
**Goal**: Cloud Engineering / DevOps career transition

Building this homelab to develop hands-on skills in virtualization, networking, monitoring, automation, and cybersecurity — all documented as a living portfolio.

---

## 📋 What This Project Demonstrates

- **Network Segmentation** — 5 VLANs with router-on-a-stick topology via pfSense
- **Infrastructure Monitoring** — Full Prometheus + Grafana + Loki + Alertmanager stack
- **Reverse Proxy & SSL** — Nginx Proxy Manager with Let's Encrypt via Cloudflare DNS-01
- **Cloud Storage** — Self-hosted Nextcloud with Cloudflare Tunnel for external access
- **Game Server Hosting** — Pterodactyl Panel + Wings managing Terraria & Minecraft servers
- **Remote Access** — Tailscale VPN (subnet router on pfSense) + Cloudflare Zero Trust
- **Backup & Storage** — NAS-backed container backups via SMB
- **Security Practices** — No admin interfaces exposed, DNS record cleanup, firewall segmentation

---

## 🖥️ Hardware

| Component | Specification |
|-----------|---------------|
| **Server** | AMD Ryzen 5 5600X · 32GB DDR4 · Proxmox VE |
| **Firewall** | AC8F Mini PC · Intel N100 · pfSense 2.7.2 |
| **Switch** | TP-Link TL-SG108E · 8-port managed · 802.1Q |
| **NAS** | UGREEN DXP2800 "Kinmoon" · 3.6TB WD Purple |
| **DNS** | Raspberry Pi 4 · Pi-hole v6.3 |
| **ISP** | 600 Mbps / 200 Mbps (Time Internet) |

---

## 🌐 Network Architecture

```
                    ┌─────────────┐
                    │   Internet  │
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │ ISP Router  │
                    │192.168.100.1│
                    └──────┬──────┘
                           │
                    ┌──────┴──────┐
                    │  pfSense    │
                    │  (N100)     │
                    │Router-on-   │
                    │  a-Stick    │
                    └──────┬──────┘
                           │ 802.1Q Trunk
                    ┌──────┴──────┐
                    │ TP-Link     │
                    │ TL-SG108E   │
                    └──────┬──────┘
           ┌───────┬───────┼───────┬───────┐
           │       │       │       │       │
        VLAN10  VLAN20  VLAN30  VLAN40  VLAN50
         Mgmt   Main   Services  DMZ   Malware
```

### VLAN Design

| VLAN | Name | Subnet | Purpose | Security |
|------|------|--------|---------|----------|
| 10 | Management | 192.168.10.0/24 | Proxmox, pfSense | Highest — admin only |
| 20 | Main Network | 192.168.20.0/24 | Client devices | Medium — trusted users |
| 30 | Services | 192.168.30.0/24 | All hosted services | Medium — controlled |
| 40 | DMZ | 192.168.40.0/24 | Future public-facing | Low trust |
| 50 | Malware Lab | 192.168.50.0/24 | Security research | Air-gapped — isolated |

---

## 📦 Services & Containers

### LXC Containers (Proxmox)

| CTID | Name | Service | IP | Status |
|------|------|---------|-----|--------|
| 201 | nginx-proxy-manager | Reverse Proxy & SSL | 192.168.30.201 | ✅ Running |
| 202 | monitoring-prometheus | Metrics Collection | 192.168.30.202 | ✅ Running |
| 203 | monitoring-grafana | Dashboards & Viz | 192.168.30.203 | ✅ Running |
| 204 | monitoring-loki | Log Aggregation | 192.168.30.204 | ✅ Running |
| 205 | monitoring-alertmanager | Alert Routing | 192.168.30.205 | ✅ Running |
| 206 | monitoring-uptime | Uptime Kuma | 192.168.30.206 | ✅ Running |
| 207 | network-ddns | Cloudflare DDNS | 192.168.30.207 | ✅ Running |
| 220 | nextcloud | Cloud Storage | 192.168.30.220 | ✅ Running |
| 300 | gaming-panel | Pterodactyl Panel | 192.168.30.210 | ✅ Running |
| 302 | gaming-wings-1 | Pterodactyl Wings | 192.168.30.212 | ✅ Running |

### Physical Devices

| Device | Role | IP | Status |
|--------|------|-----|--------|
| Proxmox Server | Hypervisor | 192.168.10.5 | ✅ Online |
| pfSense | Firewall/Router | 192.168.10.1 | ✅ Online |
| Pi-hole (RPi4) | DNS Filtering | 192.168.30.10 | ✅ Online |
| Kinmoon NAS | Backup Storage | 192.168.10.15 | ✅ Online |
| TP-Link Switch | Layer 2 | 192.168.1.20 | ✅ Online |

### External Services

| Service | Purpose |
|---------|---------|
| Cloudflare | DNS, CDN, Zero Trust Access, Tunnel |
| Tailscale | VPN (subnet router on pfSense) |
| Let's Encrypt | SSL certificates (DNS-01 via Cloudflare) |

---

## 🔒 Security Architecture

| Layer | Implementation |
|-------|----------------|
| Perimeter | ISP router → pfSense firewall |
| Network | 5 VLAN segments (air-gapped malware lab) |
| DNS | Pi-hole ad/tracker blocking |
| Access | Tailscale VPN + Cloudflare Access (Zero Trust) |
| Transport | TLS everywhere via NPM + Let's Encrypt |
| Monitoring | Prometheus + Alertmanager (Telegram & Discord) |
| Logging | Loki + Promtail centralized log aggregation |

**Key security decisions:**
- No admin interfaces (Proxmox, pfSense, Pi-hole) exposed to internet
- All external access via Tailscale VPN or Cloudflare Access with email OTP
- Nextcloud external access via Cloudflare Tunnel (no port forwarding)
- Wildcard and admin DNS records deleted from Cloudflare
- VLAN 50 (Malware Lab) completely air-gapped — no routing, no DHCP

---

## 🗺️ Project Phases

### Completed ✅

| Phase | Description | Date |
|-------|-------------|------|
| 1 | Proxmox VE bare-metal installation | Jan 2026 |
| 2 | pfSense firewall & initial VLAN setup | Jan 2026 |
| 3 | Core services (Pi-hole, NPM, Tailscale, DDNS) | Jan 2026 |
| 4 | External access & SSL (Cloudflare, Let's Encrypt) | Feb 2026 |
| 5 | Monitoring stack (Prometheus, Grafana, Loki, Alertmanager, Uptime Kuma) | Feb 2026 |
| 6A-6D | Gaming platform (Pterodactyl Panel + Wings, Terraria) | Feb 2026 |
| 9 | NAS deployment (UGREEN DXP2800, SMB backups) | Mar 2026 |
| 6F | Infrastructure Audit & VLAN Migration | Mar 2026 |
| 7 | Nextcloud deployment with Cloudflare Tunnel | Mar 2026 |

### Planned 📋

| Phase | Description | Priority |
|-------|-------------|----------|
| 7A | Backup strategy (scheduled, rotation, recovery testing) | High |
| 6E | Homepage Dashboard (gethomepage.dev) | Medium |
| 7B | n8n workflow automation (Discord/Telegram bots, GitHub sync) | Medium |
| 7C | AI Agent deployment (OpenClaw, portfolio documentation) | Low |
| 8 | Gaming expansion (Minecraft, Project Zomboid) | Low |

### Future Services Under Consideration

- **Gitea** — Self-hosted Git for IaC repos and CI/CD
- **Drone CI / Woodpecker** — CI/CD pipelines integrated with Gitea
- **Ansible Semaphore** — Web UI for infrastructure-as-code management
- **Authentik** — SSO/Identity provider (LDAP/SAML/OIDC)
- **Self-hosted Email** — Mailcow or Mox (replace Gmail)
- **Netbox** — IP Address Management (replace manual tracking)
- **HashiCorp Vault** — Secrets management
- **Obsidian + Syncthing** — Self-hosted note sync for knowledge management

---

## 📊 Architecture Decision Records

| ADR | Decision | Rationale |
|-----|----------|-----------|
| 001 | LXC over VMs | 32GB RAM constraint; LXC uses ~256-512MB vs VM 2-4GB |
| 002 | One container per service | Mirrors microservices; independent backup/update/firewall |
| 003 | Router-on-a-stick | Cost-effective; single managed switch handles L2 |
| 004 | Gaming on VLAN 30 | Reduces complexity; same access patterns as other services |
| 005 | Cloudflare for external | DDoS protection, SSL termination, Zero Trust (free tier) |
| 006 | Tailscale for admin | No exposed SSH; WireGuard-based; works through NAT |
| 007 | Cloudflare Tunnel for Nextcloud | No port forwarding needed; mobile app compatible |

---

## 📂 Documentation

Detailed documentation is maintained in the [`docs/`](./docs/) folder:

| File | Purpose |
|------|---------|
| `AI-CONTEXT.md` | Full project context for AI assistants |
| `current-state.md` | Verified live infrastructure (source of truth) |
| `architecture.md` | Target design, ADRs, network diagrams |
| `roadmap.md` | Phase status, timeline, ideas backlog |
| `service-catalog.md` | All services with ports, configs, dependencies |
| `troubleshoot.md` | Past issues and resolutions |
| `changelog.md` | Version history of all changes |

---

## 🛠️ Key Lessons Learned

1. **VLAN-aware bridges need subinterfaces** — Proxmox management IP must be on `vmbr0.XX`, not the base bridge
2. **SMB over NFS for LXC backups** — NFS can't handle LXC user namespace UID mapping
3. **DNS-01 over HTTP-01 for SSL** — Cloudflare proxy interferes with HTTP-01 challenges
4. **Cloudflare Access blocks mobile apps** — Use Cloudflare Tunnel instead for app-compatible services
5. **Check interface names before editing** — `ip link show` before touching `/etc/network/interfaces`
6. **Switch ports need PVID AND membership** — Setting only one isn't enough for access ports

---

## 📫 Connect

- **LinkedIn**: [linkedin.com/in/muzakkir-kholil](https://www.linkedin.com/in/muzakkir-kholil/)
- **GitHub**: [github.com/muzakkir97](https://github.com/muzakkir97)
- **Domain**: [najhin-gaming.com](https://najhin-gaming.com)

---

*Last updated: March 8, 2026 — Phase 7 Complete*
