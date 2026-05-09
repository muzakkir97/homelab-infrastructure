# 🏗️ Network & System Architecture

> **Last Updated:** May 9, 2026  
> **Owner:** Muzakkir Kholil  
> **Purpose:** Complete network topology, storage architecture, and system design documentation

---

## 🌐 Network Topology

### Router-on-a-Stick Design

```
Internet
    │
    ▼
ISP Router (192.168.100.1)
    │ DHCP WAN
    ▼
pfSense Firewall (AC8F Mini PC, Intel N100)
    │ WAN: DHCP from ISP
    │ LAN: igc2 (802.1Q Trunk)
    ▼
TP-Link TL-SG108E Managed Switch (192.168.1.20)
    │ 802.1Q VLAN Trunking
    ├─ Port 1: Kuromoon (Proxmox) - VLAN 10
    ├─ Port 2: Kinmoon (NAS) - VLAN 10  
    ├─ Port 3: Pi-hole (RPi4) - VLAN 30
    ├─ Port 4: Minimoon (Gaming PC) - VLAN 20
    ├─ Port 5: TP-Link EAP610 (WiFi AP) - All VLANs
    └─ Ports 6-8: Available
```

### VLAN Architecture

| VLAN ID | Name            | Network         | Gateway      | Devices                           | Purpose                    |
|---------|-----------------|-----------------|--------------|-----------------------------------|----------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Proxmox, pfSense, NAS            | Infrastructure Management  |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Gaming PC, Client Devices        | User Network              |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All LXC/VM Services, Pi-hole     | Application Services      |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future Public Services            | Demilitarized Zone        |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Security Lab Environment          | Isolated Security Testing |

### Inter-VLAN Firewall Rules

#### Default Policies
- **VLAN 10 (Management):** Full access to all VLANs (administrative control)
- **VLAN 20 (Main):** Blocked from VLAN 10 (no admin access), access to VLAN 30 services
- **VLAN 30 (Services):** Limited outbound internet, no inter-VLAN except responses
- **VLAN 40 (DMZ):** Isolated, controlled inbound/outbound rules
- **VLAN 50 (Malware):** Air-gapped, no internet or inter-VLAN access

#### Specific Rules
```
Rule 1: VLAN 20 → VLAN 10 (BLOCK)
Rule 2: VLAN 20 → VLAN 30 (ALLOW - HTTP/HTTPS/SSH)
Rule 3: VLAN 30 → Internet (ALLOW - Essential services)
Rule 4: VLAN 50 → ANY (BLOCK ALL)
Rule 5: VLAN 40 → Internet (CONTROLLED)
```

### DNS Architecture

#### Primary DNS (Pi-hole)
- **Location:** Raspberry Pi 4 (192.168.30.10)
- **Blocked Domains:** ~489,000 ad/tracker domains
- **Upstream:** pfSense DNS Resolver
- **Coverage:** All VLANs via pfSense DNS forwarding

#### Secondary DNS (pfSense)
- **DNS Resolver:** Unbound with Pi-hole forwarding
- **DHCP Integration:** Automatic hostname registration
- **External Forwarders:** Cloudflare (1.1.1.1, 1.0.0.1)

---

## 🖥️ Hardware Architecture

### Core Infrastructure

#### Kuromoon (Proxmox Hypervisor)
```
┌─────────────────────────────────────┐
│ ASUS TUF B550M-E (mATX)             │
├─────────────────────────────────────┤
│ CPU: AMD Ryzen 5 5600X (6C/12T)     │
│ RAM: 128GB DDR4-3200 (4x32GB)       │
│ GPU: RX 6700 XT 12GB (PCIe 0d:00.0) │
│ Storage: 1TB NVMe (ZFS) + 8TB HDD   │
│ NICs: Realtek + Intel i225-V         │
└─────────────────────────────────────┘
```

#### pfSense Router (AC8F Mini PC)
```
┌─────────────────────────────────────┐
│ Intel N100 Quad-Core                │
├─────────────────────────────────────┤
│ RAM: 16GB DDR4                      │
│ Storage: 512GB NVMe                 │
│ NICs: 4x Intel i226-V (2 working)   │
│ Ports: igc2/igc3 operational        │
└─────────────────────────────────────┘
```

#### Kinmoon NAS (UGREEN DXP2800)
```
┌─────────────────────────────────────┐
│ UGREEN DXP2800 (2-Bay NAS)          │
├─────────────────────────────────────┤
│ RAID: RAID 1 (Mirror)               │
│ Capacity: 5.4TB raw, 2.7TB usable   │
│ Filesystem: ext4                    │
│ Protocol: NFS + SMB                 │
│ Network: Gigabit Ethernet           │
└─────────────────────────────────────┘
```

### Network Hardware

| Device          | Model              | Management IP   | Configuration                    |
|-----------------|-------------------|-----------------|-----------------------------------|
| Managed Switch  | TP-Link TL-SG108E | 192.168.1.20    | 802.1Q VLAN, Port-based VLANs   |
| WiFi AP         | TP-Link EAP610    | TBD             | Multi-VLAN SSID (pending setup) |
| DNS Server      | Raspberry Pi 4    | 192.168.30.10   | Pi-hole + Unbound               |

---

## 💾 Storage Architecture

### ZFS Configuration (Kuromoon)
```
Pool: vmpool (899GB)
├── vmpool/data (LVM-Thin provisioning)
├── vmpool/vm-400-disk-0 (Ubuntu 22.04)
└── vmpool/snapshots (backup targets)

Pool: local-lvm (137GB NVMe)
├── Container root filesystems
└── Template storage
```

### Proxmox Storage Layout

| Storage ID    | Type      | Protocol | Capacity | Content                    | Location      |
|---------------|-----------|----------|----------|----------------------------|---------------|
| local         | Directory | Local    | 137GB    | ISO, Container templates   | NVMe SSD      |
| local-lvm     | LVM-Thin  | Local    | 137GB    | Container root disks       | NVMe SSD      |
| vmpool-fast   | ZFS       | Local    | 899GB    | VM disks, snapshots        | NVMe SSD      |
| data-storage  | Directory | Local    | 7.2TB    | Large container backups    | SATA HDD      |
| kinmoon-nfs   | NFS       | Network  | 2.6TB    | Small container backups    | NAS RAID 1    |

### Backup Architecture

#### 3-2-1 Backup Strategy
```
Production Data
    │
    ├─ Tier 1: Daily Snapshots (local ZFS)
    ├─ Tier 2: Small Containers → NAS (NFS, RAID 1)
    ├─ Tier 3: Large Containers → Local HDD
    └─ Tier 4: [Planned] Off-site (Backblaze B2)
```

#### Backup Schedule
- **Time:** 02:00 daily (small containers), 02:30 daily (large containers)
- **Retention:** 7 daily, 4 weekly, 2 monthly
- **Verification:** Monthly restore tests (MERLIN agent monitoring)

---

## 🔐 External Access Architecture

### Cloudflare Tunnel Configuration
```
Internet → Cloudflare Edge → Cloudflare Tunnel → NPM (CT 201) → Services
```

#### Protected Services (Cloudflare Access)
| Service      | Subdomain                   | Authentication      | Bypass Paths           |
|--------------|----------------------------|---------------------|------------------------|
| Grafana      | grafana.najhin-gaming.com  | Email OTP          | None                   |
| n8n          | n8n.najhin-gaming.com      | Email OTP          | None                   |
| Vault        | vault.najhin-gaming.com    | Email OTP          | None                   |
| Vaultwarden  | passwords.najhin-gaming.com| Email OTP          | /api/, /identity/      |
| Nextcloud    | cloud.najhin-gaming.com    | Email OTP          | /remote.php/dav/       |
| Ollama       | ollama.najhin-gaming.com   | Email OTP          | None                   |
| Homepage     | home.najhin-gaming.com     | Email OTP          | None                   |

### VPN Access (Tailscale)
```
Mobile/Laptop → Tailscale Mesh → pfSense (Subnet Router) → Internal Networks
```
- **Subnet Router:** pfSense (advertises 192.168.0.0/16)
- **ACLs:** Device-based access control
- **Primary Use:** Administrative access to VLAN 10

---

## 🏗️ Container Architecture

### Service Distribution (VLAN 30)

#### Monitoring Stack (CT 202-206)
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Prometheus      │────│ Grafana         │────│ Alertmanager    │
│ CT 202          │    │ CT 203          │    │ CT 205          │
│ :9090           │    │ :3000           │    │ :9093           │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Loki            │    │ Uptime Kuma     │    │ Telegram/Discord│
│ CT 204          │    │ CT 206          │    │ Webhooks        │
│ :3100           │    │ :3001           │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

#### Application Services
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ NPM             │────│ n8n             │────│ Vault           │
│ CT 201          │    │ CT 211          │    │ CT 213          │
│ :80, :443       │    │ :5678           │    │ :8200           │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Homepage        │    │ Vaultwarden     │    │ Nextcloud       │
│ CT 208          │    │ CT 214          │    │ CT 220          │
│ :3000           │    │ :80             │    │ :80             │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

#### Gaming & AI Services
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Gaming Panel    │────│ Gaming Wings    │    │ ollama-gpu      │
│ CT 300          │    │ CT 302          │    │ VM 400          │
│ :80, :443       │    │ Docker Swarm    │    │ :11434          │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Terraria        │    │ Minecraft       │    │ Open WebUI      │
│ :7777           │    │ :25565          │    │ :8080 (Docker)  │
│                 │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Resource Allocation

| Container Type | CPU Cores | RAM Allocation | Storage        | Autostart |
|----------------|-----------|----------------|----------------|-----------|
| Monitoring     | 1-2       | 1-2GB each     | vmpool-fast    | ✅        |
| Applications   | 2-4       | 2-8GB each     | vmpool-fast    | ✅        |
| Gaming         | 4-8       | 8-16GB each    | data-storage   | ✅        |
| AI/GPU (VM)    | 8         | 16GB           | vmpool-fast    | ✅        |

---

## 🎯 Design Principles

### High Availability
- **Redundancy:** RAID 1 for backups, multiple backup targets
- **Monitoring:** 360° observability with Prometheus/Grafana/Loki
- **Alerting:** Multi-channel (Telegram, Discord) with severity levels

### Security by Design
- **Zero Trust:** Every service behind authentication
- **Network Segmentation:** VLANs with strict firewall rules
- **Secrets Management:** HashiCorp Vault for all credentials
- **Regular Updates:** Automated security patching via EMIYA agent (planned)

### Scalability
- **Container-First:** Easy horizontal scaling via Proxmox
- **Storage Tiers:** Fast NVMe for active, HDD for archives
- **Resource Monitoring:** Automatic scaling alerts

### Maintainability
- **Infrastructure as Code:** All configs in Git
- **Documentation:** Auto-generated via Da Vinci agent
- **Monitoring:** Proactive issue detection via MERLIN agent

---

*This architecture supports enterprise-grade homelab operations with professional development practices and serves as a career transition portfolio for Cloud Engineering/DevOps roles.*