# Homelab Infrastructure Project

## 🎯 Project Overview

Building an enterprise-grade homelab infrastructure for cybersecurity learning and self-hosted services.

**Goal:** Replace ~RM 780/year in cloud subscriptions with self-hosted alternatives while developing enterprise networking and security skills.

---

## 📊 Project Phases

### ✅ Phase 1: Proxmox Setup (Complete)
- Installed Proxmox VE 8.x on Ryzen 5 5600X server
- Configured ZFS storage pool with mirror redundancy
- Established baseline virtualization infrastructure

📄 [Phase 1 Documentation](docs/phase-1/)

---

### ✅ Phase 2: VLAN Network Segmentation (Complete)

**Status:** ✅ Complete (January 2025)

**Achievements:**
- Configured TP-Link TL-SG108E managed switch with 5 VLANs
- Implemented 802.1Q VLAN tagging with trunk ports
- Set up Proxmox VLAN-aware bridge
- Established network segmentation foundation

**VLANs Created:**
| VLAN ID | Name | Purpose |
|---------|------|---------|
| 1 | Management | Infrastructure management |
| 10 | Main Network | General client devices |
| 20 | Services | Homelab services (Pi-hole, NAS, VMs) |
| 30 | DMZ | Public-facing services |
| 40 | Malware Lab | Isolated security testing |

**Skills Developed:**
- 802.1Q VLAN configuration
- Trunk vs access port concepts
- PVID (Port VLAN ID) configuration
- Network troubleshooting methodology

📄 [Full Documentation](docs/phase-2/) | 🗺️ [Network Topology](docs/phase-2/network-topology.md) | 🔧 [Quick Reference Guide](docs/phase-2/quick-reference.md)

---

### ✅ Phase 3: pfSense Router-on-a-Stick (Complete)

**Status:** ✅ Complete (January 2026)

**Achievements:**
- Deployed pfSense 2.7.2-RELEASE on AC8F Intel N100 Mini PC
- Implemented **router-on-a-stick** architecture with 802.1Q VLAN trunking
- Configured 5 VLANs routed through single physical interface
- Established centralized firewall with inter-VLAN routing
- Created isolated malware lab with blocked internet access

**Technical Implementation:**

| Component | Configuration |
|-----------|---------------|
| WAN Interface | DHCP from ISP Router (2.5 Gbps) |
| LAN Interface | VLAN Trunk (1 Gbps) |
| VLAN 1 | 192.168.1.0/24 - Management |
| VLAN 10 | 192.168.10.0/24 - Main Network |
| VLAN 20 | 192.168.20.0/24 - Homelab Services |
| VLAN 30 | 192.168.30.0/24 - DMZ |
| VLAN 40 | 192.168.40.0/24 - Malware Lab (Isolated) |

**Firewall Rules:**
- VLANs 1, 10, 20, 30: Full internet access via NAT
- VLAN 40: **Blocked from internet** - isolated for safe malware analysis

**Challenges Overcome:**
- Intel i226-V driver compatibility issues with FreeBSD/pfSense
- Systematic port testing and documentation
- Switch trunk port configuration for multi-VLAN support

**Skills Developed:**
- Router-on-a-stick architecture
- pfSense installation and configuration
- Firewall rule creation and management
- Inter-VLAN routing concepts
- Hardware troubleshooting and adaptation

📄 [Installation Guide](docs/phase-3/phase-3-installation-guide.md) | 🔧 [Troubleshooting Log](docs/phase-3/phase-3-troubleshooting-log.md) | 🗺️ [Network Topology](docs/phase-3/phase-3-network-topology.md)

---

### 🟡 Phase 4: Self-Hosted Services (Planned)

**Status:** 🟡 Planning

**Objectives:**
- Deploy Pi-hole on VLAN 20 for network-wide DNS filtering
- Configure UGREEN NAS for centralized storage
- Set up Tailscale VPN on pfSense for secure remote access
- Implement Nginx Proxy Manager for reverse proxy with SSL

---

### 🟡 Phase 5: Game Servers (Planned)

**Status:** 🟡 Planning

**Objectives:**
- Minecraft server deployment
- Project Zomboid dedicated server
- Terraria server
- Centralized game server management

---

### 🟡 Phase 6: Security Lab (Planned)

**Status:** 🟡 Planning

**Objectives:**
- Malware analysis VMs on isolated VLAN 40
- Red Team practice environment
- Security tool deployment (Wazuh, ELK Stack)

---

## 🖥️ Hardware Inventory

### Core Infrastructure

| Device | Specs | Role | Status |
|--------|-------|------|--------|
| Proxmox Server | Ryzen 5 5600X, 32GB RAM, ZFS Mirror | Hypervisor | ✅ Operational |
| AC8F Mini PC | Intel N100, 8GB RAM, 5x 2.5GbE | pfSense Router | ✅ Operational |
| TP-Link TL-SG108E | 8-port Managed Switch | VLAN Switching | ✅ Configured |
| Raspberry Pi 4B | 4GB RAM | Pi-hole DNS | ⏳ Phase 4 |
| UGREEN NASync | 2-bay NAS | Network Storage | ⏳ Phase 4 |

### AC8F Port Status (Intel i226-V)

| Port | Interface | Status | Notes |
|------|-----------|--------|-------|
| Port 1010 | igc0 | ❌ | Driver compatibility issue |
| LAN4 | igc1 | ❌ | Driver compatibility issue |
| LAN3 | igc2 | ✅ | Used for LAN/VLAN Trunk |
| LAN2 | igc3 | ✅ | Used for WAN (2.5 Gbps) |

---

## 🌐 Network Architecture

```
                            INTERNET
                                │
                                ▼
                    ┌───────────────────────┐
                    │      ISP Router       │
                    │    (WiFi + DHCP)      │
                    └───────────┬───────────┘
                                │
              ┌─────────────────┴─────────────────┐
              │                                   │
              ▼                                   ▼
    ┌─────────────────┐                 ┌─────────────────┐
    │   WiFi Clients  │                 │  pfSense Router │
    │ (Shared Network)│                 │   (AC8F N100)   │
    └─────────────────┘                 │                 │
                                        │  WAN: DHCP      │
                                        │  LAN: Trunk     │
                                        └────────┬────────┘
                                                 │
                                    802.1Q VLAN Trunk
                                    (VLANs 1,10,20,30,40)
                                                 │
                                                 ▼
                                    ┌────────────────────┐
                                    │  TP-Link TL-SG108E │
                                    │   Managed Switch   │
                                    └────────┬───────────┘
                                             │
              ┌──────────────┬───────────────┼───────────────┬──────────────┐
              │              │               │               │              │
              ▼              ▼               ▼               ▼              ▼
        ┌──────────┐  ┌──────────┐    ┌──────────┐    ┌──────────┐   ┌──────────┐
        │ Proxmox  │  │Workstation│   │ Pi-hole  │    │   NAS    │   │  Future  │
        │ (Trunk)  │  │ (VLAN 1) │    │(VLAN 20) │    │(VLAN 20) │   │ Devices  │
        └──────────┘  └──────────┘    └──────────┘    └──────────┘   └──────────┘
```

---

## 💰 Cost Savings Projection

### Annual Subscription Costs Being Replaced:

| Service | Annual Cost | Self-Hosted Alternative |
|---------|-------------|------------------------|
| Google Drive (200GB) | ~RM 250/year | Nextcloud |
| iCloud Photos | ~RM 130/year | Immich |
| Game Server Hosting | ~RM 400/year | Self-hosted |
| **Total Annual Savings** | **~RM 780/year** | |

**Hardware Investment:** ~RM 2,500 (one-time)  
**ROI:** ~3 years

---

## 🎓 Skills Developed

### Phase 1 - Virtualization
- Proxmox VE installation and configuration
- ZFS storage management
- Virtual machine deployment

### Phase 2 - Network Segmentation
- VLAN configuration and management
- 802.1Q tagging protocol
- Trunk and access port concepts
- Network segmentation principles

### Phase 3 - Enterprise Routing
- **Router-on-a-stick architecture**
- pfSense firewall administration
- Inter-VLAN routing
- Firewall rule creation
- Hardware troubleshooting
- FreeBSD/pfSense driver issues

---

## 📚 Documentation Structure

```
docs/
├── phase-1/
│   └── proxmox-setup.md
├── phase-2/
│   ├── vlan-configuration.md
│   ├── network-topology.md
│   └── quick-reference.md
└── phase-3/
    ├── phase-3-summary.md
    ├── phase-3-installation-guide.md
    ├── phase-3-troubleshooting-log.md
    └── phase-3-network-topology.md
```

---

## 🔗 Connect

- **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)
- **LinkedIn:** [linkedin.com/in/muzakkir-kholil](https://www.linkedin.com/in/muzakkir-kholil/)

---

## 📝 License

This project is documented for educational purposes. Feel free to use as reference for your own homelab projects.

---

**Last Updated:** January 2026 - Phase 3 Complete ✅
