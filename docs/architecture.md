# Homelab Infrastructure Architecture

> **Last Updated:** December 19, 2024  
> **Project Owner:** Muzakkir Kholil  
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

## Network Topology

### Physical Architecture

```
Internet
    ↓
ISP Router (192.168.100.1)
    ↓
pfSense Firewall (WAN: DHCP, LAN: 192.168.10.1)
    ↓ (802.1Q Trunk - igc2)
TP-Link TL-SG108E Managed Switch (192.168.10.20)
    ↓
┌─────────┬─────────┬─────────┬─────────┬─────────┐
│ VLAN10  │ VLAN20  │ VLAN30  │ VLAN40  │ VLAN50  │
│  Mgmt   │  Main   │Services │  DMZ    │Malware  │
└─────────┴─────────┴─────────┴─────────┴─────────┘
```

### Router-on-a-Stick Implementation

The network uses a single trunk connection from pfSense to the managed switch, with all VLANs carried over 802.1Q tagging. This centralized routing approach allows pfSense to enforce inter-VLAN policies while maintaining network segmentation.

**Key Design Decisions:**
- Single physical connection reduces cable complexity
- All routing handled by pfSense for security control
- VLAN tagging enables logical network separation
- Managed switch provides VLAN-aware port assignment

## VLAN Design

### VLAN Allocation Table

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                |
|---------|-----------------|-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)     |

### VLAN Purposes & Security Zones

**VLAN 10 (Management):**
- Hypervisor and infrastructure components
- Administrative access only
- High security requirements
- Devices: Proxmox (Kuromoon), NAS (Kinmoon), pfSense management

**VLAN 20 (Main):**
- End-user devices and workstations
- Internet access allowed
- Gaming PC (Minimoon) - homelab-isolated
- Standard client security policies

**VLAN 30 (Services):**
- All LXC containers (14 total)
- Pi-hole DNS server
- Internal service communication
- External access via reverse proxy

**VLAN 40 (DMZ):**
- Reserved for future public services
- Planned for services requiring direct internet exposure
- Enhanced logging and monitoring

**VLAN 50 (Malware Analysis):**
- Completely isolated security lab
- No internet access (air-gapped)
- Malware analysis and testing environment

## Firewall Architecture

### Inter-VLAN Policies

```
VLAN20 (Main) → VLAN10 (Mgmt): BLOCKED
VLAN20 (Main) → VLAN30 (Services): ALLOWED (specific ports)
VLAN30 (Services) → VLAN10 (Mgmt): ALLOWED (backup, monitoring)
VLAN30 (Services) → Internet: ALLOWED
VLAN50 (Malware) → ALL: BLOCKED (air-gapped)
```

### Firewall Rule Categories

**1. Anti-Lockout Rules**
- pfSense management interface access
- Emergency access preservation

**2. RFC1918 Blocks**
- Prevent access to other private networks
- Block common private ranges (10.0.0.0/8, 172.16.0.0/12)

**3. Inter-VLAN Access Control**
- Main VLAN blocked from Management VLAN
- Services allowed specific management access
- DMZ isolation rules (when implemented)

**4. Service-Specific Rules**
- DNS (Pi-hole): Port 53 from all VLANs
- HTTP/HTTPS: Port 80/443 for web services
- Monitoring: Prometheus scraping ports
- Backup: SMB/NFS ports to NAS

### Known Firewall Issues & Resolutions

| Issue | Resolution |
|-------|------------|
| Rules not applying | Place custom rules BEFORE RFC1918 blocks |
| Source address problems | Use "subnets" not "address" for ranges |
| Internet loss after changes | Verify anti-lockout rules remain active |

## Storage Architecture

### Storage Hierarchy

```
Proxmox Node (Kuromoon)
├── local-lvm (NVMe) - 137 GB - Container root disks
├── vmpool-fast (ZFS) - 899 GB - Fast container storage  
└── data-storage (HDD) - 7.2 TB - Large container backups

External NAS (Kinmoon)
└── kinmoon-smb (CIFS) - 3.6 TB - Small container backups
```

### Storage Types & Use Cases

**local-lvm (NVMe - LVM-Thin):**
- Purpose: Container root filesystems
- Capacity: 137 GB
- Technology: LVM thin provisioning
- Performance: High IOPS for OS operations

**vmpool-fast (NVMe - ZFS):**
- Purpose: Container data storage
- Capacity: 899 GB  
- Technology: ZFS with compression/deduplication
- Performance: High-performance application data

**data-storage (HDD - Direct):**
- Purpose: Large container backups (Nextcloud, Gaming)
- Capacity: 7.2 TB
- Technology: Direct attached storage
- Performance: Bulk storage for backups

**kinmoon-smb (NAS - CIFS):**
- Purpose: Small container backups
- Capacity: 3.6 TB WD Purple
- Technology: SMB/CIFS over network
- Performance: Network-attached bulk storage

### Backup Strategy

**Backup Job Schedule:**
```
02:00 - Small Containers → kinmoon-smb (NAS)
02:30 - Large Containers → data-storage (Local HDD)
```

**Retention Policy:**
- 7 daily backups
- 4 weekly backups  
- 2 monthly backups

**Container Classification:**
- **Small:** Monitoring, networking, authentication services
- **Large:** Nextcloud (220), Gaming services (302)

## External Access Architecture

### Access Methods Overview

```
External User
    ↓
┌─────────────────┬─────────────────┬─────────────────┐
│  Tailscale VPN  │ Cloudflare      │ Cloudflare      │
│  (Primary)      │ Tunnel          │ Access + Tunnel │
│                 │ (Nextcloud)     │ (Protected Apps)│
└─────────────────┴─────────────────┴─────────────────┘
    ↓                    ↓                    ↓
pfSense Subnet      NPM Reverse         NPM Reverse
Router              Proxy               Proxy + Auth
    ↓                    ↓                    ↓
Internal Services   cloud.domain.com    {service}.domain.com
```

### Tailscale Implementation

**Configuration:**
- pfSense as subnet router
- Routes advertised: 192.168.10.0/24, 192.168.20.0/24, 192.168.30.0/24
- Primary method for administrative access
- Direct access to all VLANs

**Use Cases:**
- Administrative tasks on VLAN 10
- Direct service access without reverse proxy
- Secure management channel

### Cloudflare Tunnel

**Nextcloud Public Access:**
- Tunnel: cloud.najhin-gaming.com → 192.168.30.220:443
- Direct tunnel without additional authentication
- SSL termination at NPM level
- Public file sharing and sync capabilities

### Cloudflare Access Protected Services

**Protected Applications (7 total):**
1. grafana.najhin-gaming.com - Monitoring dashboards
2. n8n.najhin-gaming.com - Workflow automation  
3. vault.najhin-gaming.com - Secrets management
4. passwords.najhin-gaming.com - Password manager
5. home.najhin-gaming.com - Homepage dashboard
6. terraria.najhin-gaming.com - Game server
7. mc.najhin-gaming.com - Minecraft server

**Authentication Method:**
- Email OTP (One-Time Password)
- Domain: najhin-gaming.com
- Session duration: Cloudflare default

## DNS Architecture

### DNS Resolution Flow

```
Client Request
    ↓
pfSense DNS Resolver (Unbound)
    ↓ (Forwards to)
Pi-hole (192.168.30.10)
    ↓ (Upstream)
Cloudflare DNS (1.1.1.1)
```

### Pi-hole Configuration

**Installation:** Raspberry Pi 4, VLAN 30
**Blocklists:** ~489,000 domains blocked
**Interface:** Physical device, not containerized
**Upstream:** Cloudflare DNS for security and speed

### Internal DNS Records

**Automatic Records (NPM):**
- All *.najhin-gaming.com subdomains
- Automatic SSL certificate generation
- Internal resolution to container IPs

**Manual DNS (if needed):**
- Custom internal domains
- Development services
- Testing environments

## Hardware Specifications

### Primary Infrastructure

**Proxmox Hypervisor (Kuromoon):**
```
CPU: AMD Ryzen 5 5600X (6C/12T, 3.7-4.6GHz)
RAM: 32 GB DDR4
GPU: AMD RX 6700 XT 12GB (IOMMU Group 18, reserved for AI)
Storage: NVMe SSD + 7.2TB HDD
Network: Integrated Ethernet
IP: 192.168.10.5
Role: Primary virtualization platform
```

**pfSense Firewall:**
```
Platform: AC8F Mini PC
CPU: Intel N100 (4C/4T, up to 3.4GHz)
RAM: 8GB DDR4
Network: 4x 2.5GbE (only igc2/igc3 functional)
IP: 192.168.10.1 (LAN), DHCP (WAN)
Role: Router, firewall, VPN gateway
```

**Network Switch:**
```
Model: TP-Link TL-SG108E
Ports: 8x Gigabit Ethernet
Management: Web-based VLAN configuration
IP: 192.168.10.20 (will migrate to 192.168.10.20)
Role: VLAN switching and port assignment
```

**NAS Storage (Kinmoon):**
```
Model: UGREEN DXP2800
Storage: 3.6TB WD Purple HDD
Network: Gigabit Ethernet
IP: 192.168.10.15
SMB Target: kinmoon-smb
Role: Network backup destination
```

**DNS Server:**
```
Platform: Raspberry Pi 4
RAM: 4GB
Network: Gigabit Ethernet
IP: 192.168.30.10
Software: Pi-hole
Role: Network-wide ad blocking and DNS
```

### Secondary Hardware

**Gaming Workstation (Minimoon):**
```
CPU: AMD Ryzen 7 7800X3D
GPU: AMD RX 9070 XT 16GB (when available)
Role: Gaming only - isolated from homelab
Network: VLAN 20, no homelab access
```

**Wireless Infrastructure:**
```
Model: TP-Link EAP610 (purchased, pending setup)
Standard: Wi-Fi 6 (802.11ax)
Management: Omada Controller (planned)
Installation: Pending Phase implementation
```

### Hardware Notes

**GPU Allocation (Kuromoon):**
- RX 6700 XT in IOMMU Group 18
- Reserved for future Ollama/ROCm AI inference (Phase 11)
- Currently idle: 5W power draw, zero-RPM fan mode
- Thermal baseline: 46°C edge temperature

**Network Interface Issues:**
- Intel i226-V NICs have FreeBSD driver problems
- Only igc2/igc3 ports functional on pfSense
- igc0/igc1 unusable due to driver compatibility

**Power & Thermal:**
- Kuromoon idle: CPU 48.5°C, GPU 46°C
- All systems designed for 24/7 operation
- Temperature monitoring via Grafana dashboards

## Container Architecture

### LXC Container Distribution

**All containers run on Proxmox (Kuromoon) in VLAN 30 (192.168.30.0/24)**

```
VLAN 30 Services Network
├── 201 - nginx-proxy-manager (NPM) - Reverse proxy
├── 202 - monitoring-prometheus - Metrics collection
├── 203 - monitoring-grafana - Metrics visualization  
├── 204 - monitoring-loki - Log aggregation
├── 205 - monitoring-alertmanager - Alert routing
├── 206 - monitoring-uptime - Uptime monitoring
├── 207 - network-ddns - Dynamic DNS updates
├── 208 - dashboard-homepage - Service portal
├── 211 - automation-n8n - Workflow automation
├── 213 - vault - Secrets management (HashiCorp Vault)
├── 214 - password-vaultwarden - Password manager
├── 220 - nextcloud-hub - File storage and collaboration
├── 300 - gaming-panel - Pterodactyl game panel
└── 302 - gaming-wings-1 - Game servers (Docker)
```

### Service Dependencies

**Core Infrastructure Flow:**
1. **NPM (201)** - Entry point for all web services
2. **Pi-hole** - DNS resolution for all services
3. **Monitoring Stack** - Observability for all containers
4. **Vault (213)** - Secrets for automation and APIs

**Automation Flow:**
1. **n8n (211)** - Orchestrates workflows
2. **Vault (213)** - Provides API credentials
3. **Nextcloud (220)** - Document storage
4. **External APIs** - GitHub, Cloudflare, Claude

**Backup Dependencies:**
- **Small containers** → kinmoon-smb (NAS)
- **Large containers** → data-storage (Local HDD)
- **All containers** → Automated nightly backups

### Container Resource Allocation

**Resource Classes:**
- **Lightweight:** 1 CPU, 1GB RAM (DNS, DDNS, alerts)
- **Standard:** 2 CPU, 2GB RAM (monitoring, automation)  
- **Heavy:** 4+ CPU, 4GB+ RAM (Nextcloud, gaming)

**Storage Allocation:**
- **Root disks:** local-lvm (NVMe)
- **Data volumes:** vmpool-fast (ZFS) or data-storage (HDD)
- **Backup targets:** External NAS + local HDD

---

*This architecture document reflects the current state as of Phase 58 completion with 14 operational LXC containers and comprehensive network segmentation.*