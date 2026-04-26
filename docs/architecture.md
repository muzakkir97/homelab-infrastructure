# Architecture Documentation

## Network Topology

### High-Level Overview

```
Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
                                              ↓
                                    802.1Q Trunk (igc2)
                                              ↓
                                    TP-Link TL-SG108E
                                              ↓
                    ┌─────────┬─────────┬─────────┬─────────┐
                  VLAN10   VLAN20   VLAN30   VLAN40   VLAN50
                  Mgmt     Main    Services   DMZ    Malware
```

### Router-on-a-Stick Design

The infrastructure uses a router-on-a-stick topology with pfSense handling inter-VLAN routing through a single 802.1Q trunk connection to the managed switch. This design provides:

- Centralized routing and firewall control
- VLAN segmentation for security
- Scalable network architecture
- Single point of configuration

### Physical Hardware Layout

```
[Internet] ── [ISP Router] ── [pfSense AC8F] ── [TP-Link TL-SG108E]
                                                       │
                ┌──────────────┬─────────────┬─────────┴─────────┐
                │              │             │                   │
           [Kuromoon]    [Kinmoon NAS]  [Pi-hole]         [Gaming PC]
            Proxmox        Backup       DNS Server          Minimoon
         192.168.10.5   192.168.10.15  192.168.30.10   192.168.20.101
```

## VLAN Design

### VLAN Segmentation Table

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                |
|---------|-----------------|-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)     |

### Network Segmentation Strategy

- **Management Network (VLAN 10):** Critical infrastructure components requiring administrative access
- **Main Network (VLAN 20):** End-user devices and workstations
- **Services Network (VLAN 30):** All containerized services and internal applications
- **DMZ (VLAN 40):** Reserved for future public-facing services requiring internet exposure
- **Malware Lab (VLAN 50):** Completely isolated environment for security testing

## Firewall Architecture

### Inter-VLAN Security Policies

```
VLAN20 (Main) ──────────────────────→ VLAN30 (Services) ✅ ALLOW
VLAN20 (Main) ──X────────────────────→ VLAN10 (Mgmt)    ❌ DENY
VLAN30 (Services) ──────────────────→ VLAN10 (Mgmt)    ✅ ALLOW (limited)
VLAN10 (Mgmt) ──────────────────────→ All VLANs        ✅ ALLOW
VLAN50 (Malware) ──X────────────────→ All VLANs        ❌ DENY (air-gapped)
All VLANs ──────────────────────────→ Internet         ✅ ALLOW
```

### Key Firewall Rules

1. **Default Deny All:** All inter-VLAN traffic blocked by default
2. **Management Access Control:** VLAN 20 cannot access VLAN 10 (admin isolation)
3. **Service Access:** VLAN 20 can access VLAN 30 services
4. **Malware Isolation:** VLAN 50 completely air-gapped
5. **Internet Access:** All VLANs can reach internet through pfSense

### pfSense Configuration Notes

- **WAN Interface:** DHCP from ISP router
- **LAN Interface:** 802.1Q trunk on igc2 (Intel i226-V NIC)
- **VLAN Interfaces:** All VLANs configured as virtual interfaces
- **DNS Resolver:** Forwarding to Pi-hole (192.168.30.10)

## Storage Architecture

### ZFS Configuration (Proxmox Host)

```
Storage Pool Layout:
├── vmpool-fast (ZFS) - 899 GB NVMe SSD
│   ├── Fast container storage
│   └── High-performance workloads
├── local-lvm (LVM-Thin) - 137 GB NVMe SSD
│   ├── Container root filesystems
│   └── System storage
└── data-storage (ext4) - 7.2 TB HDD
    ├── Large container data
    └── Local backup target
```

### Storage Allocation Strategy

| Storage Pool    | Type      | Capacity | Use Case                    | Performance Profile |
|----------------|-----------|---------|-----------------------------|-------------------|
| vmpool-fast    | ZFS       | 899 GB  | High-performance containers | High IOPS, low latency |
| local-lvm      | LVM-Thin  | 137 GB  | Root filesystems           | Standard performance |
| data-storage   | ext4 HDD  | 7.2 TB  | Data storage, backups      | High capacity |
| kinmoon-smb    | NAS       | 3.6 TB  | Network backup target      | Network attached |

### Backup Storage Design

```
Backup Flow:
Small Containers (≤20GB) ──────→ kinmoon-smb (NAS via SMB/CIFS)
Large Containers (>20GB) ──────→ data-storage (Local HDD)

Retention Policy: 7 daily, 4 weekly, 2 monthly
Schedule: 02:00 (small), 02:30 (large)
```

## External Access Architecture

### Cloudflare Tunnel Configuration

```
Cloudflare Edge ──── Cloudflared Daemon ──── Nginx Proxy Manager ──── Services
      │                    (CT 201)              (CT 201)
      │
   SSL/TLS                                   ┌── grafana.domain.com
  Termination                                ├── n8n.domain.com
                                            ├── vault.domain.com
                                            ├── passwords.domain.com
                                            ├── cloud.domain.com
                                            ├── home.domain.com
                                            └── ollama.domain.com
```

### Cloudflare Access Integration

- **Authentication:** Email OTP to muzakkir.kholil06@gmail.com
- **Protected Applications:** 7 services behind Cloudflare Access
- **Bypass Rules:** Vaultwarden API paths (/api/, /identity/) for mobile app compatibility

### Tailscale VPN Access

```
Tailscale Network: 100.x.x.x/24
├── pfSense as Subnet Router
├── Direct access to VLAN subnets
└── Administrative access path (primary)
```

## DNS Architecture

### DNS Resolution Flow

```
Client Query ──→ pfSense DNS Resolver ──→ Pi-hole ──→ Upstream DNS
     │              (192.168.x.1)        (192.168.30.10)   (Cloudflare)
     │                     │                    │
     └── Internal DNS ─────┘                    │
                                               │
                                     ┌─── Ad Blocking
                                     └─── ~489K domains blocked
```

### DNS Configuration

- **Primary DNS:** Pi-hole (192.168.30.10)
- **Secondary DNS:** pfSense DNS Resolver
- **Upstream DNS:** Cloudflare (1.1.1.1, 1.0.0.1)
- **Ad Blocking:** ~489,000 domains blocked
- **Local Resolution:** Internal domain names resolved locally

## Hardware Architecture

### Server Specifications

#### Kuromoon (Proxmox Host)
```
CPU: AMD Ryzen 5 5600X (6C/12T, 3.7-4.6GHz)
RAM: 32GB DDR4
GPU: AMD RX 6700 XT 12GB (Passed through to VM 400)
Storage: 
  ├── 1TB NVMe SSD (System + Fast Storage)
  └── 8TB HDD (Data Storage)
Network: Integrated Gigabit Ethernet
Role: Hypervisor, Container Host
```

#### pfSense Firewall (AC8F Mini PC)
```
CPU: Intel N100 (4C/4T, up to 3.4GHz)
RAM: 8GB DDR4
Storage: 128GB eMMC
Network: 4x Intel i226-V 2.5GbE (only igc2/igc3 functional)
Role: Router, Firewall, VPN Gateway
```

#### Kinmoon (UGREEN NAS)
```
Model: UGREEN DXP2800
CPU: Intel Celeron N5105
RAM: 8GB
Storage: 2x 3.6TB WD Purple (RAID 1 equivalent)
Network: Gigabit Ethernet
Role: Network Attached Storage, Backup Target
```

#### Minimoon (Gaming PC)
```
CPU: AMD Ryzen 7 7800X3D (8C/16T)
GPU: AMD RX 9070 XT 16GB
RAM: 32GB DDR5
Storage: 2TB NVMe SSD
Role: Gaming Only (Strictly no homelab usage)
```

### Network Hardware

#### TP-Link TL-SG108E Managed Switch
```
Ports: 8x Gigabit Ethernet
VLAN Support: 802.1Q tagging
Management: Web-based configuration
Current IP: 192.168.1.20 (planned migration to 192.168.10.20)
```

#### TP-Link EAP610 WiFi Access Point
```
Standard: WiFi 6 (802.11ax)
Speed: Up to 1.2Gbps
Status: Purchased, pending installation
Power: PoE+ required
```

## Container Architecture (Proxmox)

### Container Distribution by Purpose

```
Infrastructure Services (VLAN 30):
├── CT 201: nginx-proxy-manager (Reverse Proxy)
├── CT 207: network-ddns (Dynamic DNS)
└── CT 208: dashboard-homepage (Internal Dashboard)

Monitoring Stack (VLAN 30):
├── CT 202: monitoring-prometheus (Metrics Collection)
├── CT 203: monitoring-grafana (Visualization)
├── CT 204: monitoring-loki (Log Aggregation)
├── CT 205: monitoring-alertmanager (Alert Management)
└── CT 206: monitoring-uptime (Uptime Monitoring)

Automation & Security (VLAN 30):
├── CT 211: automation-n8n (Workflow Automation)
├── CT 213: vault (Secrets Management)
└── CT 214: password-vaultwarden (Password Manager)

Data & Collaboration (VLAN 30):
└── CT 220: nextcloud-hub (File Sync, Collaboration)

Gaming Platform (VLAN 30):
├── CT 300: gaming-panel (Pterodactyl Panel)
└── CT 302: gaming-wings-1 (Game Servers)

AI & Machine Learning (VLAN 30):
└── VM 400: ollama-gpu (Local LLM with GPU Passthrough)
```

### Resource Allocation Summary

```
Total Containers: 14 LXC + 1 KVM VM
Total Memory Allocation: ~16GB
Total Storage Usage: ~400GB
All containers: autostart enabled
Network: All on VLAN 30 except infrastructure
```

### GPU Passthrough Architecture (VM 400)

```
Host GPU (Kuromoon) ──── VFIO Passthrough ──── VM 400 (ollama-gpu)
RX 6700 XT 12GB              │                  Ubuntu 22.04
                             │                  ROCm 6.1.3
IOMMU Groups:               │                  Ollama Server
├── Group 18: GPU (2d:00.0) ├─── Passed ────→ HSA_OVERRIDE_GFX_VERSION=10.3.0
└── Group 19: Audio (2d:00.1)                 qwen3:14b (9.3GB model)
```

This architecture provides enterprise-grade network segmentation, robust security, scalable storage, and comprehensive monitoring while maintaining clear separation between production services and personal use systems.