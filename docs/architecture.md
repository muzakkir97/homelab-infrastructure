# Network and System Architecture

## Network Topology

```
                Internet
                    |
            ISP Router (192.168.100.1)
                    |
              pfSense Firewall
              WAN: DHCP from ISP
              LAN: 802.1Q Trunk (igc2)
                    |
           TP-Link TL-SG108E Managed Switch
           (VLAN-aware, 802.1Q tagging)
                    |
        ┌───────────┼───────────┼───────────┼───────────┐
    VLAN 10     VLAN 20     VLAN 30     VLAN 40     VLAN 50
   (MGMT)       (MAIN)    (SERVICES)     (DMZ)    (MALWARE)
```

## VLAN Design and Addressing

### VLAN 10 - Management (192.168.10.0/24)
- **Gateway:** 192.168.10.1 (pfSense)
- **Purpose:** Infrastructure management
- **Devices:**
  - 192.168.10.5 - Kuromoon (Proxmox hypervisor)
  - 192.168.10.15 - Kinmoon (UGREEN NAS)
  - 192.168.10.1 - pfSense management interface

### VLAN 20 - Main Network (192.168.20.0/24)
- **Gateway:** 192.168.20.1 (pfSense)
- **Purpose:** Client devices (laptops, phones, desktops)
- **Devices:**
  - 192.168.20.101 - Minimoon (Gaming PC - never used for homelab)

### VLAN 30 - Services (192.168.30.0/24)
- **Gateway:** 192.168.30.1 (pfSense)
- **Purpose:** All containerized services and Pi-hole
- **Devices:**
  - 192.168.30.10 - Raspberry Pi 4 (Pi-hole DNS)
  - 192.168.30.201-220 - LXC containers (see Container Inventory)
  - 192.168.30.300+ - Additional service containers

### VLAN 40 - DMZ (192.168.40.0/24)
- **Gateway:** 192.168.40.1 (pfSense)
- **Purpose:** Future public-facing services
- **Status:** Reserved for future use

### VLAN 50 - Malware Lab (192.168.50.0/24)
- **Gateway:** 192.168.50.1 (pfSense)
- **Purpose:** Isolated security lab environment
- **Security:** Air-gapped from all other VLANs

## Inter-VLAN Firewall Policies

### Implemented Rules
- **VLAN 20 → VLAN 10:** BLOCKED (clients cannot access management)
- **VLAN 30 → VLAN 10:** ALLOWED (services need management access)
- **VLAN 30 → VLAN 20:** LIMITED (specific service access only)
- **VLAN 50 → ALL:** BLOCKED (complete isolation)
- **ALL → VLAN 50:** BLOCKED (complete isolation)

### DNS Resolution
- All VLANs use pfSense DNS Resolver
- pfSense forwards to Pi-hole (192.168.30.10)
- Pi-hole blocks ~489,000 ad/tracking domains

## Hardware Specifications

### Kuromoon - Proxmox Hypervisor
- **CPU:** AMD Ryzen 5 5600X (6C/12T, 3.7-4.6GHz)
- **RAM:** 32GB DDR4
- **GPU:** AMD RX 6700 XT 12GB (IOMMU Group 18, reserved for Ollama)
- **Storage:** 
  - NVMe SSD (system + fast container storage)
  - 7.2TB HDD (large container storage)
- **Network:** Multiple Intel NICs
- **Role:** Primary hypervisor running 14 LXC containers

### pfSense Firewall
- **Hardware:** AC8F Mini PC
- **CPU:** Intel N100
- **NICs:** Intel i226-V (only igc2/igc3 functional due to FreeBSD driver issues)
- **Role:** Router, firewall, VLAN management, Tailscale subnet router

### Kinmoon - Network Attached Storage
- **Hardware:** UGREEN DXP2800
- **Storage:** 3.6TB WD Purple drives
- **Protocol:** SMB/CIFS (switched from NFS due to LXC UID mapping issues)
- **Role:** Backup target for small containers

### Network Infrastructure
- **Switch:** TP-Link TL-SG108E (managed, VLAN-capable)
- **WiFi AP:** TP-Link EAP610 (purchased, pending deployment)
- **DNS:** Raspberry Pi 4 running Pi-hole

### Minimoon - Gaming System (ISOLATED)
- **CPU:** AMD Ryzen 7 7800X3D
- **GPU:** AMD RX 9070 XT 16GB
- **Role:** Gaming only - never used for homelab infrastructure

## Storage Architecture

### Proxmox Storage Pools
```
Storage Pool        Type        Protocol    Capacity    Purpose
local-lvm          LVM-Thin    Direct      137GB       Container root disks
vmpool-fast        ZFS         Direct      899GB       Fast container storage  
data-storage       Ext4        Direct      7.2TB       Large container backups
kinmoon-nfs        NFS/SMB     Network     3.6TB       Small container backups
```

### ZFS Configuration
- **Pool:** vmpool-fast
- **RAID:** Single drive (future expansion planned)
- **Compression:** Enabled
- **Deduplication:** Disabled (RAM constraints)

### Backup Strategy
- **Small containers (< 10GB):** kinmoon-nfs (NAS)
- **Large containers (> 10GB):** data-storage (local HDD)
- **Schedule:** Daily at 02:00 (small) and 02:30 (large)
- **Retention:** 7 daily, 4 weekly, 2 monthly snapshots

## External Access Architecture

### Tailscale VPN (Primary Access)
- **Deployment:** Subnet router on pfSense
- **Access:** All VLANs accessible to authorized devices
- **Use Case:** Administrative access, remote management

### Cloudflare Tunnel
- **Services:** Nextcloud (cloud.najhin-gaming.com)
- **Protocol:** HTTPS with automatic SSL
- **Security:** No exposed ports on firewall

### Cloudflare Access (Zero Trust)
- **Protected Services:** 7 applications
  - grafana.najhin-gaming.com
  - n8n.najhin-gaming.com
  - vault.najhin-gaming.com
  - passwords.najhin-gaming.com
  - home.najhin-gaming.com
  - terraria.najhin-gaming.com
  - mc.najhin-gaming.com
- **Authentication:** Email OTP
- **Bypass:** Tailscale IPs automatically allowed

## DNS Architecture

### Resolution Chain
```
Client Query → pfSense DNS Resolver → Pi-hole → Upstream (Cloudflare)
```

### Pi-hole Configuration
- **Blocked Domains:** ~489,000 ad/tracking domains
- **Custom DNS:** Local service resolution
- **Upstream:** Cloudflare (1.1.1.1, 1.0.0.1)
- **Interface:** Web admin on port 80/443

### Domain Management
- **Primary Domain:** najhin-gaming.com (Cloudflare managed)
- **Subdomains:** Automatic SSL via Cloudflare
- **Local DNS:** Custom entries in Pi-hole for internal services

## Container Architecture

### LXC Deployment Strategy
- **Template:** Debian 12 (standardized)
- **Network:** All containers on VLAN 30
- **Storage:** Root on local-lvm, data on appropriate pools
- **Autostart:** Enabled on all production containers

### Service Distribution
```
Monitoring Stack: CT 202-206
- Prometheus (202)
- Grafana (203) 
- Loki (204)
- Alertmanager (205)
- Uptime Kuma (206)

Core Services: CT 201, 207-208, 211
- Nginx Proxy Manager (201)
- DDNS Updater (207)
- Homepage Dashboard (208)
- n8n Automation (211)

Security & Secrets: CT 213-214
- HashiCorp Vault (213)
- Vaultwarden (214)

Data & Gaming: CT 220, 300, 302
- Nextcloud Hub (220)
- Gaming Panel (300)
- Gaming Wings (302)
```

## Security Architecture

### Defense in Depth
1. **Perimeter:** ISP router + pfSense firewall
2. **Segmentation:** VLAN isolation with enforced policies
3. **DNS Filtering:** Pi-hole ad/malware blocking
4. **Access Control:** Tailscale VPN + Cloudflare Access
5. **Secrets Management:** HashiCorp Vault centralization
6. **Backup Security:** Automated with retention policies

### Known Vulnerabilities
- **Legacy LAN:** 192.168.1.0/24 still exists (scheduled for removal)
- **Management Access:** VLAN 20 → VLAN 10 blocked but switch on legacy network
- **Gaming Isolation:** Minimoon must never be used for homelab

### Monitoring & Alerting
- **Prometheus:** Metrics collection from all services
- **Grafana:** Visualization and dashboards
- **Alertmanager:** Critical alerts to Telegram
- **Uptime Kuma:** Service availability monitoring