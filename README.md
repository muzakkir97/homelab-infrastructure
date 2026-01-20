# Homelab Infrastructure Project

## 🎯 Project Overview
Building an enterprise-grade homelab infrastructure for cybersecurity learning and self-hosted services.

**Goal:** Replace ~RM 780/year in cloud subscriptions with self-hosted alternatives.

---

## 📊 Project Phases

### ✅ Phase 1: Proxmox Setup (Complete)
- Installed Proxmox VE 8.x on Ryzen 5 5600X server
- Configured ZFS storage pool
- Established baseline infrastructure

[Documentation](docs/phase-1/)

---

### ✅ Phase 2: VLAN Network Segmentation (Complete)
**Status:** ✅ Complete (January 2025)

**Achievements:**
- Configured TP-Link TL-SG108E managed switch with 5 VLANs
- Implemented 802.1Q VLAN tagging with trunk port
- Set up Proxmox VLAN-aware bridge
- Established network segmentation for security

**VLANs Created:**
- VLAN 1: Management (192.168.100.0/24)
- VLAN 10: Clients (192.168.10.0/24)
- VLAN 20: Services (192.168.20.0/24)
- VLAN 30: DMZ (192.168.30.0/24)
- VLAN 40: Malware Lab (192.168.40.0/24)

**Skills Developed:**
- 802.1Q VLAN configuration
- Trunk vs access port setup
- PVID concepts
- Network troubleshooting

[📄 Full Documentation](docs/phase-2/phase-2-vlan-segmentation-complete.md)  
[🗺️ Network Topology](docs/phase-2/network-topology-phase2.md)  
[🔧 Quick Reference Guide](docs/phase-2/quick-reference-guide-phase2.md)

---

### 🟡 Phase 3: pfSense Integration (In Planning)
**Status:** 🟡 Planning

**Objectives:**
- Configure pfSense with VLAN interfaces
- Implement inter-VLAN routing
- Set up firewall rules per VLAN
- Configure DHCP and DNS per VLAN

[📋 Phase 3 Planning Document](docs/phase-3-planning/phase-3-planning-pfsense.md)

---

## 🖥️ Hardware Inventory

### Core Infrastructure
| Device | Specs | Role | Status |
|--------|-------|------|--------|
| **Proxmox Server** | Ryzen 5 5600X, 32GB RAM, ZFS | Hypervisor | ✅ Operational |
| **AC8F Mini PC** | Intel N100, 8GB RAM, 5x GbE | pfSense Router | ⏳ Phase 3 |
| **TP-Link TL-SG108E** | 8-port Managed Switch | VLAN Switching | ✅ Configured |
| **Raspberry Pi 4B** | 4GB RAM | Pi-hole DNS | ✅ Operational |
| **UGREEN NASync** | 2-bay NAS | Network Storage | ✅ Operational |

---

## 🌐 Network Topology
```
Internet → pfSense (AC8F) → TP-Link Switch → Devices
                                  ├─ Proxmox (VLAN Trunk)
                                  ├─ Gaming PC (VLAN 1)
                                  ├─ Work Laptop (VLAN 10)
                                  ├─ Raspberry Pi (VLAN 1)
                                  └─ NAS (VLAN 1)
```

[📊 Detailed Network Diagram](docs/phase-2/network-topology-phase2.md)

---

## 💰 Cost Savings Projection

**Annual Subscription Costs Being Replaced:**
- Google Drive (200GB): ~RM 250/year → Nextcloud
- iCloud Photos: ~RM 130/year → Immich
- Game Server Hosting: ~RM 400/year → Self-hosted

**Total Annual Savings: ~RM 780/year**  
**Hardware Investment: ~RM 1,500 (one-time)**  
**ROI: ~2 years**

---

## 🎓 Skills Developed

### Phase 1
- Proxmox VE installation and configuration
- ZFS storage management
- Basic virtualization concepts

### Phase 2
- VLAN configuration and management
- 802.1Q tagging protocol
- Trunk and access port concepts
- Network segmentation principles
- Systematic troubleshooting methodology

---

## 📚 Documentation

All phase documentation is available in the `/docs` folder:
- [Phase 1: Proxmox Setup](docs/phase-1/)
- [Phase 2: VLAN Segmentation](docs/phase-2/)
- [Phase 3: pfSense Integration (Planning)](docs/phase-3-planning/)

---

## 🔗 Connect

- **GitHub:** github.com/muzakkir97/homelab-infrastructure
- **LinkedIn:** [https://www.linkedin.com/in/muzakkir-kholil/]

---

## 📝 License

This project is documented for educational purposes.

---

*Last Updated: January 2025 - Phase 2 Complete*
