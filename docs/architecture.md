# Network and System Architecture

## Network Topology

### Physical Infrastructure
```
Internet
   ↓
ISP Router (192.168.100.1)
   ↓
pfSense Firewall (AC8F Mini PC, Intel N100)
   WAN: DHCP from ISP
   LAN: 802.1Q Trunk (igc2 only - igc0/igc1 have FreeBSD driver issues)
   ↓
TP-Link TL-SG108E Managed Switch (192.168.1.20 → moving to 192.168.10.20)
   ↓
┌─────────┬─────────┬─────────┬─────────┬─────────┐
│ VLAN 10 │ VLAN 20 │ VLAN 30 │ VLAN 40 │ VLAN 50 │
│  Mgmt   │  Main   │Services │  DMZ    │Malware  │
└─────────┴─────────┴─────────┴─────────┴─────────┘
```

### Router-on-a-Stick Design
- **Single trunk link** from pfSense to managed switch
- **VLAN tagging** at switch level
- **Inter-VLAN routing** handled by pfSense
- **Broadcast domain isolation** per VLAN

## VLAN Architecture

| VLAN ID | Name            | Subnet          | Gateway      | Purpose & Devices                        |
|---------|-----------------|-----------------|--------------|------------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure Management                |
|         |                 |                 |              | - Kuromoon (Proxmox): 192.168.10.5      |
|         |                 |                 |              | - Kinmoon (NAS): 192.168.10.15          |
|         |                 |                 |              | - Switch: 192.168.10.20 (planned)       |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client Devices                          |
|         |                 |                 |              | - Minimoon (Gaming PC): 192.168.20.101  |
|         |                 |                 |              | - WiFi clients (EAP610 - pending)       |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | Service Containers & Pi-hole            |
|         |                 |                 |              | - All 14 LXC containers                 |
|         |                 |                 |              | - Pi-hole (RPi4): 192.168.30.10         |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services           |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Air-gapped security lab                 |

## Firewall Rules Overview

### Inter-VLAN Policies
```
VLAN 20 (Main) → VLAN 10 (Mgmt):     BLOCKED
VLAN 20 (Main) → VLAN 30 (Services): ALLOW HTTP/HTTPS only
VLAN 30 (Services) → VLAN 10 (Mgmt): ALLOW (containers need Proxmox API)
VLAN 40 (DMZ) → All VLANs:           BLOCKED (egress only)
VLAN 50 (Malware) → All VLANs:       BLOCKED (completely isolated)

Special Rules:
- n8n CT 211 → Kuromoon SSH (TCP 22): ALLOW
- All VLANs → Internet: ALLOW
- All VLANs → Pi-hole DNS: ALLOW
```

### Critical Firewall Lessons
- **Place new rules BEFORE RFC1918 block rules**
- **Source must be "subnets" not "address" to avoid internet loss**
- **Test without Tailscale** to verify local firewall rules
- **Double NAT requires port forwarding on BOTH ISP router AND pfSense**

## Storage Architecture

### Proxmox Storage Pools

| Storage ID    | Type       | Technology | Size   | Purpose                    | Performance |
|---------------|------------|------------|--------|----------------------------|-------------|
| local-lvm     | Local      | LVM-Thin   | 137GB  | Container root disks       | Fast        |
| vmpool-fast   | Local      | ZFS        | 899GB  | Fast container storage     | Fastest     |
| data-storage  | Local      | HDD        | 7.2TB  | Large container backups    | Slow        |
| kinmoon-nfs   | Remote     | NFS        | 3.6TB  | Small container backups    | Network     |

### Backup Target Matrix

| Container Size | Backup Target  | Rationale                           |
|----------------|----------------|-------------------------------------|
| Small (<5GB)   | kinmoon-nfs    | Network efficiency, NAS reliability |
| Large (>20GB)  | data-storage   | Avoid network bottlenecks           |

### NAS Configuration (Kinmoon)
- **Hardware:** UGREEN DXP2800
- **Storage:** 3.6TB WD Purple HDD
- **Protocol:** NFS (switched from SMB after kernel crash issues)
- **Squash:** Map all users to admin
- **Performance:** Network-dependent, adequate for small backups

### Storage Lessons Learned
```
SMB/CIFS kernel crashes     → Switch to NFS
NFS permission denied       → Set squash to "Map all users to admin"
Large file I/O on NAS      → Use local storage for large containers
ZFS snapshot busy          → Kill processes or reboot before destroy
Container locked (backup)  → pct unlock <CTID>
LXC backup to NFS fails    → UID mapping issues (100000-165536)
```

## External Access Architecture

### Primary Access: Tailscale VPN
```
Client (Tailscale) → pfSense (subnet router) → Internal VLANs
```
- **Subnet router:** Configured on pfSense
- **Access scope:** Full homelab (all VLANs)
- **Security:** Tailscale ACLs + pfSense firewall
- **Performance:** Direct P2P when possible

### Secondary Access: Cloudflare Tunnel
```
Client → Cloudflare → Tunnel (CT 220) → Nextcloud only
```
- **Service:** Nextcloud at cloud.najhin-gaming.com
- **SSL:** Cloudflare managed certificates
- **Security:** No firewall ports opened
- **Limitation:** Single service only

### Application Access: Cloudflare Access
```
Client → Cloudflare Access (email OTP) → Cloudflare Tunnel → Services
```

| Service | Subdomain | Container | Protection |
|---------|-----------|-----------|------------|
| Grafana | grafana.najhin-gaming.com | CT 203 | Email OTP |
| n8n | n8n.najhin-gaming.com | CT 211 | Email OTP |
| Vault | vault.najhin-gaming.com | CT 213 | Email OTP |
| Vaultwarden | passwords.najhin-gaming.com | CT 214 | Email OTP |
| Homepage | home.najhin-gaming.com | CT 208 | Email OTP |
| Ollama | ollama.najhin-gaming.com | VM 400 | Email OTP |
| Gaming Panel | Panel access only | CT 300 | Email OTP |

## DNS Architecture

### Hierarchical DNS Resolution
```
Client Request → Pi-hole (192.168.30.10) → Cloudflare DNS (1.1.1.1)
                      ↓
               Block List Check (~489K domains)
                      ↓
               pfSense DNS Resolver (forwarding mode)
```

### DNS Components
- **Primary:** Pi-hole on Raspberry Pi 4
- **Blocking:** ~489,000 domains (ads, trackers, malware)
- **Upstream:** Cloudflare DNS (1.1.1.1, 1.0.0.1)
- **Backup:** pfSense DNS Resolver
- **DHCP:** Pi-hole advertises itself as DNS server

### Domain Management
- **External domain:** najhin-gaming.com (Cloudflare managed)
- **SSL certificates:** Cloudflare managed (full strict)
- **Subdomains:** Automated via network-ddns container (CT 207)

## Hardware Specifications

### Kuromoon (Proxmox Host)
```
CPU: AMD Ryzen 5 5600X (6C/12T, 3.7-4.6GHz)
RAM: 32GB DDR4
GPU: RX 6700 XT 12GB (IOMMU Group 18, ROCm-enabled)
Storage: NVMe SSD (system) + 7.2TB HDD (data)
Network: Integrated Gigabit Ethernet
Role: Hypervisor + GPU passthrough for Ollama
```

### pfSense Firewall (AC8F Mini PC)
```
CPU: Intel N100 (4C/4T, up to 3.4GHz)
RAM: 16GB DDR5
Network: 4x Intel i226-V (only igc2/igc3 functional)
Role: Router, firewall, VPN endpoint
Note: FreeBSD driver issues with i226-V NICs
```

### Kinmoon (NAS)
```
System: UGREEN DXP2800
Storage: 3.6TB WD Purple HDD
Network: Gigabit Ethernet
Protocols: NFS, SMB/CIFS
Role: Backup target, file storage
```

### Minimoon (Gaming PC - Homelab Isolated)
```
CPU: AMD Ryzen 7 7800X3D
GPU: RX 9070 XT 16GB
Role: Gaming only - never used for homelab
Network: VLAN 20 (blocked from VLAN 10)
```

### Monitoring Hardware
```
Pi-hole: Raspberry Pi 4 (DNS + ad blocking)
WiFi AP: TP-Link EAP610 (purchased, pending deployment)
Switch: TP-Link TL-SG108E (8-port managed, VLAN support)
```

## Container Resource Allocation

### Resource Distribution (14 LXC Containers)
```
Total Proxmox RAM: 32GB
Container allocation: ~8-12GB
Host overhead: ~4GB
Available for VMs: ~16GB (for Ollama VM 400)
```

### Container Performance Tiers
```
Tier 1 (Critical): nginx-proxy-manager, pi-hole
Tier 2 (Monitoring): prometheus, grafana, loki, alertmanager
Tier 3 (Applications): nextcloud, n8n, vault, vaultwarden
Tier 4 (Gaming): gaming-panel, gaming-wings-1
```

### Storage Performance Mapping
```
Fast (vmpool-fast ZFS): Database-heavy containers
Standard (local-lvm): Most containers
Bulk (data-storage HDD): Large file storage
Network (kinmoon-nfs): Backup targets
```

## Security Architecture Layers

### Layer 1: Perimeter Defense
- **ISP Router:** First firewall layer
- **pfSense:** Primary firewall with stateful inspection
- **VLAN segmentation:** Broadcast domain isolation

### Layer 2: Network Segmentation
- **Management isolation:** VLAN 10 blocked from client access
- **Service isolation:** VLAN 30 restricted protocols
- **Malware isolation:** VLAN 50 completely air-gapped

### Layer 3: Application Security
- **External authentication:** Cloudflare Access with email OTP
- **Internal secrets:** HashiCorp Vault (KV engine)
- **Password management:** Vaultwarden self-hosted

### Layer 4: Monitoring & Alerting
- **Infrastructure monitoring:** Prometheus + Grafana
- **Log aggregation:** Loki
- **Alert routing:** Alertmanager → Telegram + Discord
- **Uptime monitoring:** Custom container (CT 206)

### Layer 5: Backup & Recovery
- **Daily automated backups:** 7 daily, 4 weekly, 2 monthly
- **Multiple backup targets:** Local HDD + NAS
- **Backup validation:** Quarterly restore tests (planned)

## Performance Characteristics

### Network Performance
```
Inter-VLAN: ~940 Mbps (line rate)
VPN (Tailscale): ~200-400 Mbps (CPU dependent)
NAS (NFS): ~80-120 MB/s
Container-to-container: ~10 Gbps (virtual networking)
```

### Storage Performance
```
NVMe (vmpool-fast): ~3,500 MB/s read, ~3,000 MB/s write
HDD (data-storage): ~150 MB/s sequential
NAS (network): ~80-120 MB/s (Gigabit limited)
```

### GPU Performance (Ollama)
```
RX 6700 XT: 12GB VRAM
Model capacity: qwen2.5:14b (9GB), llama3.2:3b (2GB)
Inference speed: ~20-50 tokens/second (model dependent)
Power: 5W idle (zero-RPM), ~150W under load
```