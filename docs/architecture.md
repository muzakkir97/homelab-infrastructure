# Architecture Documentation

## Network Topology

The homelab uses a **router-on-a-stick** design with VLAN segmentation through pfSense and a managed switch.

```
Internet
    ↓
ISP Router (192.168.100.1)
    ↓ WAN (DHCP)
pfSense Firewall (AC8F Mini PC)
    ↓ 802.1Q Trunk (igc2)
TP-Link TL-SG108E Managed Switch
    ↓
┌─────────┬─────────┬─────────┬─────────┐
│ VLAN 10 │ VLAN 20 │ VLAN 30 │ VLAN 40 │ VLAN 50
│  Mgmt   │  Main   │Services │  DMZ    │Malware
└─────────┴─────────┴─────────┴─────────┘
```

## VLAN Design

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                |
|---------|-----------------|-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)     |

## Firewall Rules Overview

### Inter-VLAN Policies
- **VLAN 20 → VLAN 10**: BLOCKED (clients cannot access management)
- **VLAN 30 → VLAN 10**: LIMITED (services can reach Proxmox API, NAS)
- **VLAN 10 → All**: ALLOWED (management access)
- **VLAN 50**: ISOLATED (no inter-VLAN communication)
- **Custom Rules**: TCP 22 from 192.168.30.211 → 192.168.10.5 (n8n SSH to Kuromoon)

### External Access
- **Tailscale VPN**: Primary admin access method
- **Cloudflare Tunnel**: Nextcloud public access
- **Cloudflare Access**: Protected services (Grafana, n8n, Vault, etc.)

## Hardware Specifications

| Device           | Hostname | Specifications                            | IP Address     | Role                                 |
|------------------|----------|-------------------------------------------|----------------|--------------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB  | 192.168.10.5   | Hypervisor                           |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                  | 192.168.10.1   | Router, Firewall                     |
| Managed Switch   | —        | TP-Link TL-SG108E                         | 192.168.1.20   | Layer 2, VLANs                       |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple           | 192.168.10.15  | Backups (SMB target: kinmoon-nfs)    |
| DNS Server       | —        | Raspberry Pi 4                            | 192.168.30.10  | Pi-hole (~489K domains blocked)      |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB          | 192.168.20.101 | Gaming only — never homelab          |
| WiFi AP          | —        | TP-Link EAP610                            | TBD            | Purchased, pending setup             |

### GPU Configuration (Kuromoon)
- **RX 6700 XT**: IOMMU Group 18, earmarked for Ollama/ROCm AI inference
- **Audio Device**: IOMMU Group 19
- **Idle Baseline**: CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan

## Storage Architecture

### Storage Pools

| Name         | Type       | Protocol | Capacity | Purpose                 | Location |
|--------------|------------|----------|----------|-------------------------|----------|
| kinmoon-nfs  | NAS        | SMB/CIFS | 3.6 TB   | Small container backups | Kinmoon  |
| data-storage | Local HDD  | Direct   | 7.2 TB   | Large container backups | Kuromoon |
| local-lvm    | Local NVMe | LVM-Thin | 137 GB   | Container root disks    | Kuromoon |
| vmpool-fast  | Local NVMe | ZFS      | 899 GB   | Fast container storage  | Kuromoon |

### Backup Strategy

| Job              | Schedule | Target Storage           | Containers                              | Retention                    |
|------------------|----------|--------------------------|-----------------------------------------|------------------------------|
| Small Containers | 02:00    | kinmoon-nfs (NAS)        | 201, 202, 203, 204, 205, 206, 207, 214, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | data-storage (Local HDD) | 220, 302                               | 7 daily, 4 weekly, 2 monthly |

## DNS Architecture

### DNS Resolution Flow
```
Client Request → pfSense DNS Resolver → Pi-hole (192.168.30.10)
                                           ↓
                                    Blocked Domains (~489K)
                                           ↓
                                    Cloudflare Upstream
```

### DNS Services
- **Primary**: Pi-hole on Raspberry Pi 4 (192.168.30.10)
- **Upstream**: Cloudflare (1.1.1.1, 1.0.0.1)
- **pfSense Role**: DNS forwarding to Pi-hole for all VLANs
- **Ad Blocking**: ~489,000 domains blocked

## External Access Architecture

### Access Methods

| Method            | Purpose                  | Authentication        | Services                    |
|-------------------|--------------------------|----------------------|------------------------------|
| Tailscale VPN     | Primary admin access     | Device trust         | All internal services        |
| Cloudflare Tunnel | Public Nextcloud         | Built-in auth        | cloud.najhin-gaming.com     |
| Cloudflare Access | Protected web services   | Email OTP            | 7 services (Grafana, n8n, etc.) |

### Cloudflare Access Protected Services
1. grafana.najhin-gaming.com
2. n8n.najhin-gaming.com
3. vault.najhin-gaming.com
4. passwords.najhin-gaming.com
5. home.najhin-gaming.com
6. terraria.najhin-gaming.com
7. mc.najhin-gaming.com

### Security Layers
- **Perimeter**: ISP Router → pfSense firewall
- **Segmentation**: VLAN isolation with enforced firewall rules
- **DNS Security**: Pi-hole ad/tracker blocking
- **VPN Access**: Tailscale mesh network
- **Application Auth**: Cloudflare Access with email OTP
- **Secrets Management**: HashiCorp Vault + Vaultwarden

## Container Architecture

### LXC Container Distribution

```
VLAN 30 Services (192.168.30.0/24)
├── Proxy & Networking
│   ├── CT 201: nginx-proxy-manager (192.168.30.201)
│   └── CT 207: network-ddns (192.168.30.207)
├── Monitoring Stack
│   ├── CT 202: monitoring-prometheus (192.168.30.202)
│   ├── CT 203: monitoring-grafana (192.168.30.203)
│   ├── CT 204: monitoring-loki (192.168.30.204)
│   ├── CT 205: monitoring-alertmanager (192.168.30.205)
│   └── CT 206: monitoring-uptime (192.168.30.206)
├── Automation & Security
│   ├── CT 211: automation-n8n (192.168.30.211)
│   ├── CT 213: vault (192.168.30.213)
│   └── CT 214: password-vaultwarden (192.168.30.214)
├── Productivity
│   ├── CT 208: dashboard-homepage (192.168.30.208)
│   └── CT 220: nextcloud-hub (192.168.30.220)
└── Gaming
    ├── CT 300: gaming-panel (192.168.30.210)
    └── CT 302: gaming-wings-1 (192.168.30.212)
```

**Total: 14 LXC containers, all on VLAN 30, all autostart enabled**

## Service Discovery & Load Balancing

### Nginx Proxy Manager (CT 201)
- **Role**: Reverse proxy for all web services
- **SSL**: Automated Let's Encrypt certificates
- **Routing**: Subdomain-based routing to backend containers

### Service Endpoints

| Service                | Internal IP        | External Domain                   | Protocol |
|------------------------|--------------------|-----------------------------------|----------|
| Grafana                | 192.168.30.203     | grafana.najhin-gaming.com         | HTTPS    |
| n8n                    | 192.168.30.211     | n8n.najhin-gaming.com             | HTTPS    |
| HashiCorp Vault        | 192.168.30.213     | vault.najhin-gaming.com           | HTTPS    |
| Vaultwarden            | 192.168.30.214     | passwords.najhin-gaming.com       | HTTPS    |
| Homepage               | 192.168.30.208     | home.najhin-gaming.com            | HTTPS    |
| Nextcloud              | 192.168.30.220     | cloud.najhin-gaming.com           | HTTPS    |
| Terraria               | 192.168.30.212     | terraria.najhin-gaming.com        | HTTPS    |
| Minecraft              | 192.168.30.212     | mc.najhin-gaming.com              | HTTPS    |

## Monitoring & Alerting Architecture

### Metrics Collection
```
Prometheus (CT 202) ← node_exporter (all containers)
        ↓
    Grafana (CT 203) ← Loki (CT 204) ← promtail (all containers)
        ↓
 Alertmanager (CT 205) → Telegram Bot + Discord Webhook
```

### Alert Channels
- **Critical Alerts**: Telegram (@JhinGilgamesh_bot)
- **Warning Alerts**: Discord webhook (#alerts channel)
- **Uptime Monitoring**: Uptime Kuma (CT 206)