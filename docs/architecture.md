# Homelab Infrastructure Architecture

> **Last Updated:** April 27, 2026  
> **Purpose:** Complete network and system architecture documentation  
> **Owner:** Muzakkir Kholil  

## Hardware Specifications

### Core Infrastructure

| Device           | Hostname | Specifications                           | IP Address     | Role                              |
|------------------|----------|------------------------------------------|----------------|-----------------------------------|
| **Proxmox Server** | Kuromoon | AMD Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB | 192.168.10.5   | Primary hypervisor                |
| **pfSense Firewall** | —        | AC8F Mini PC, Intel N100, 8GB RAM       | 192.168.10.1   | Router, firewall, VPN gateway    |
| **Managed Switch** | —        | TP-Link TL-SG108E (8-port managed)      | 192.168.1.20   | Layer 2 switching, VLAN tagging  |
| **NAS Storage**  | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple         | 192.168.10.15  | Backup storage (SMB/NFS)          |
| **DNS Server**   | —        | Raspberry Pi 4, 4GB RAM                 | 192.168.30.10  | Pi-hole DNS filtering             |
| **Gaming PC**    | Minimoon | AMD Ryzen 7 7800X3D, RX 9070 XT 16GB    | 192.168.20.101 | **Gaming only — never homelab**  |
| **WiFi AP**      | —        | TP-Link EAP610 (WiFi 6)                 | TBD            | Wireless access (pending setup)  |

### GPU Passthrough Configuration (Kuromoon)

- **Primary GPU:** RX 6700 XT 12GB passed through to VM 400 (ollama-gpu)
- **IOMMU Groups:** Group 18 (GPU), Group 19 (audio) — both passed through
- **Driver Configuration:** amdgpu blacklisted, vfio-pci claimed
- **Performance:** Idle baseline CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan
- **ROCm Configuration:** HSA_OVERRIDE_GFX_VERSION=10.3.0 for gfx1031 compatibility

## Network Topology

### Router-on-a-Stick Design

```
Internet (1Gbps Fiber)
         ↓
ISP Router (192.168.100.1)
         ↓ WAN DHCP
pfSense Firewall (192.168.10.1)
         ↓ 802.1Q Trunk (igc2)
TP-Link TL-SG108E Switch
         ↓
┌────────┬────────┬────────┬────────┬────────┐
│ VLAN10 │ VLAN20 │ VLAN30 │ VLAN40 │ VLAN50 │
│  Mgmt  │  Main  │Services│  DMZ   │Malware │
└────────┴────────┴────────┴────────┴────────┘
```

### VLAN Segmentation

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                | Devices |
|---------|-----------------|-----------------|--------------|----------------------------------------|---------|
| **10**  | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure management              | 3       |
| **20**  | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices, workstations          | 2       |
| **30**  | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole      | 15      |
| **40**  | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          | 0       |
| **50**  | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)    | 0       |

**Device Distribution:**
- **VLAN 10:** Kuromoon (192.168.10.5), pfSense (192.168.10.1), Kinmoon NAS (192.168.10.15)
- **VLAN 20:** Minimoon Gaming PC (192.168.20.101), Switch management (192.168.1.20 — pending migration)
- **VLAN 30:** 14 LXC containers (201-302) + 1 KVM VM (400) + Pi-hole (192.168.30.10)
- **VLAN 40/50:** Reserved for future expansion

### Firewall Rules Overview

#### Inter-VLAN Policies
```
VLAN20 → VLAN10: BLOCKED (clients cannot reach management)
VLAN20 → VLAN30: ALLOWED (clients can reach services)
VLAN30 → VLAN10: ALLOWED (services need management access)
VLAN30 → VLAN20: BLOCKED (services cannot reach clients)
VLAN40 → ALL: RESTRICTED (DMZ isolation)
VLAN50 → ALL: BLOCKED (malware lab air-gap)
```

#### pfSense Rule Structure
1. **Anti-Lockout Rule** (WAN access to pfSense GUI)
2. **VLAN-specific rules** (ordered by VLAN ID)
3. **RFC1918 block rules** (deny private IP ranges to WAN)
4. **Default deny** (implicit)

## Storage Architecture

### Proxmox Storage Pools

| Storage ID    | Type      | Protocol | Capacity | Usage                           | Containers |
|---------------|-----------|----------|----------|---------------------------------|------------|
| **local-lvm** | LVM-Thin  | Direct   | 137 GB   | Container root disks            | All LXC    |
| **vmpool-fast** | ZFS      | Direct   | 899 GB   | High-performance VM storage     | VM 400     |
| **data-storage** | Directory | Direct  | 7.2 TB   | Large container data            | CT 220, 302|
| **kinmoon-smb** | SMB/CIFS | Network  | 3.6 TB   | Backup storage (NAS)            | Backups    |

### ZFS Configuration
- **Pool:** vmpool-fast on NVMe SSD
- **Compression:** lz4 (default)
- **Snapshots:** Enabled for VM backups
- **Thin provisioning:** Enabled

### Backup Strategy

| Job              | Schedule | Target Storage   | Containers                                  | Retention        |
|------------------|----------|------------------|---------------------------------------------|------------------|
| Small Containers | 02:00    | kinmoon-smb (NAS)| 201,202,203,204,205,206,207,214,300         | 7d/4w/2m         |
| Large Containers | 02:30    | data-storage     | 220,302                                     | 7d/4w/2m         |

**Backup Verification:** Restore testing scheduled via MERLIN agent (CT 207 recommended)

## Service Inventory

### Core Services (14 LXC + 1 VM)

| ID  | Type | Name                    | IP             | Subdomain                     | Role                    |
|-----|------|-------------------------|----------------|-------------------------------|-------------------------|
| 201 | LXC  | nginx-proxy-manager     | 192.168.30.201 | —                             | Reverse proxy, SSL     |
| 202 | LXC  | monitoring-prometheus   | 192.168.30.202 | —                             | Metrics collection      |
| 203 | LXC  | monitoring-grafana      | 192.168.30.203 | grafana.najhin-gaming.com     | Metrics visualization   |
| 204 | LXC  | monitoring-loki         | 192.168.30.204 | —                             | Log aggregation         |
| 205 | LXC  | monitoring-alertmanager | 192.168.30.205 | —                             | Alert routing           |
| 206 | LXC  | monitoring-uptime       | 192.168.30.206 | —                             | Service monitoring      |
| 207 | LXC  | network-ddns            | 192.168.30.207 | —                             | Dynamic DNS updates     |
| 208 | LXC  | dashboard-homepage      | 192.168.30.208 | home.najhin-gaming.com        | Infrastructure dashboard|
| 211 | LXC  | automation-n8n          | 192.168.30.211 | n8n.najhin-gaming.com         | Workflow automation     |
| 213 | LXC  | vault                   | 192.168.30.213 | vault.najhin-gaming.com       | Secrets management      |
| 214 | LXC  | password-vaultwarden    | 192.168.30.214 | passwords.najhin-gaming.com   | Password manager        |
| 220 | LXC  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com       | File storage, collaboration|
| 300 | LXC  | gaming-panel            | 192.168.30.210 | —                             | Game server management  |
| 302 | LXC  | gaming-wings-1          | 192.168.30.212 | terraria/mc.najhin-gaming.com | Game server instances   |
| 400 | VM   | ollama-gpu              | 192.168.30.221 | ollama.najhin-gaming.com      | Local AI inference      |

**All services:** VLAN 30, autostart enabled, 100% uptime target

## External Access Architecture

### Cloudflare Tunnel Configuration

```
Internet → Cloudflare Edge → Tunnel → nginx-proxy-manager (CT 201) → Backend Services
```

**Tunneled Services (7 total):**
- Grafana (grafana.najhin-gaming.com)
- n8n (n8n.najhin-gaming.com)  
- Vault (vault.najhin-gaming.com)
- Vaultwarden (passwords.najhin-gaming.com)
- Ollama (ollama.najhin-gaming.com)
- Homepage (home.najhin-gaming.com)
- Nextcloud (cloud.najhin-gaming.com)

### Cloudflare Access Security

**Protection:** All 7 services behind Cloudflare Access with Email OTP
**Authorized Email:** muzakkir.kholil06@gmail.com (single user)
**Bypass Paths:** 
- Vaultwarden API (/api/, /identity/ — mobile app compatibility)

### VPN Access (Primary)

**Tailscale Configuration:**
- **Subnet Router:** pfSense with subnet advertisement
- **Advertised Subnets:** 192.168.10.0/24, 192.168.20.0/24, 192.168.30.0/24
- **Primary Use:** Administrative access to management VLAN
- **Client Access:** All homelab services via private IPs

## DNS Architecture

### Pi-hole Configuration
- **Container:** Raspberry Pi 4 (192.168.30.10)
- **Blocklists:** ~489,000 domains blocked
- **Upstream DNS:** Cloudflare (1.1.1.1, 1.0.0.1)
- **DHCP:** Disabled (pfSense handles DHCP)

### DNS Resolution Flow
```
Client → pfSense DNS Resolver → Pi-hole (192.168.30.10) → Cloudflare DNS
```

**pfSense DNS Resolver:**
- **Forwarding Mode:** Query forwarding to Pi-hole
- **Domain Overrides:** *.najhin-gaming.com → Cloudflare
- **Local Resolution:** homelab.local domain (internal)

## Security Architecture

### Defense in Depth

| Layer           | Implementation                                                      |
|-----------------|---------------------------------------------------------------------|
| **Perimeter**   | ISP Router → pfSense firewall                                       |
| **Segmentation** | 5 VLANs with enforced inter-VLAN firewall rules                    |
| **DNS Security** | Pi-hole ad/tracker blocking (~489K domains)                        |
| **VPN Access**   | Tailscale (subnet router on pfSense)                               |
| **External Auth** | Cloudflare Access (Email OTP, single authorized user)              |
| **External Access** | Cloudflare Tunnel for all internet-facing services               |
| **Admin Access** | Tailscale only (VLAN 20 blocked from VLAN 10)                      |
| **Secrets**     | HashiCorp Vault (CT 213) with 8 KV secret paths                    |
| **Passwords**   | Vaultwarden (CT 214) personal password manager                     |

### Secrets Management (Vault)

**KV Engine Paths (8 total):**
- kv/gilgamesh — Telegram bot tokens, Claude API keys
- kv/cloudflare — DNS API tokens (pending fix — token truncated)
- kv/proxmox — API tokens for automation
- kv/alertmanager — Webhook URLs
- kv/github — API tokens for documentation pipeline
- kv/nextcloud — Admin credentials, WebDAV
- kv/n8n — Service credentials
- kv/pihole — Admin passwords

**Security Note:** Vault reseals on reboot — manual unseal required: `pct exec 213 -- vault operator unseal`

## Network Interfaces

### pfSense Interface Mapping
- **WAN (igc0):** ISP Router connection (DHCP)
- **LAN (igc2):** 802.1Q trunk to managed switch
- **igc1, igc3:** Available (Intel i226-V driver issues on igc0/igc1)

### Known Network Issues
- **Intel i226-V:** FreeBSD driver compatibility issues
- **Double NAT:** Port forwarding required on both ISP router and pfSense
- **Tailscale Ping:** Use `tailscale ping` instead of ICMP ping for testing

## Monitoring & Observability

### Prometheus Metrics Stack
- **Prometheus:** CT 202 (metrics collection)
- **Grafana:** CT 203 (visualization) 
- **Loki:** CT 204 (log aggregation)
- **Alertmanager:** CT 205 (alert routing)

### Alert Thresholds
- **High Memory:** 85% (raised from 80% due to Windrose baseline ~9GB)
- **High CPU:** 85% for 5 minutes
- **High Disk:** 90%
- **Service Down:** Immediate alert

### Alert Routing
- **Critical Alerts:** Telegram (via Alertmanager webhook)
- **Warning Alerts:** Discord #alerts channel
- **Planned:** Route all alerts through n8n for central processing

---

*This architecture supports 14 LXC containers + 1 KVM VM with 100% uptime, hybrid cloud access, and enterprise-grade security for career development and learning.*