# 🏗️ Homelab Infrastructure Project

> Building an enterprise-grade homelab for learning cybersecurity, infrastructure management, and self-hosted services

[![Status](https://img.shields.io/badge/Status-Active-success)]()
[![Proxmox](https://img.shields.io/badge/Proxmox-VE%208.x-orange)]()
[![Phase](https://img.shields.io/badge/Phase-1%20Complete-blue)]()

---

## 👤 About Me

**Name**: Muzakkir  
**Role**: Customer Service Engineer @ F-Secure  
**Location**: Petaling Jaya, Selangor, Malaysia  
**Domain**: [najhin-gaming.com](https://najhin-gaming.com)

Building this homelab to expand my skills in virtualization, networking, cybersecurity, and infrastructure automation.

---

## 📋 Project Overview

This project documents my journey building a production-grade homelab from scratch, focusing on:
- Enterprise virtualization (Proxmox VE)
- Redundant storage with ZFS
- Network segmentation and security
- Self-hosted services replacing cloud alternatives
- Cybersecurity lab for Red Team practice
- AI and automation experiments

---

## 🎯 Project Objectives

### Infrastructure Goals
- [x] Build enterprise-grade virtualization platform
- [x] Implement redundant storage with ZFS
- [ ] Deploy network segmentation with VLANs
- [ ] Set up dedicated firewall (pfSense)
- [ ] Implement network-wide ad blocking (Pi-hole)

### Service Goals
- [ ] Replace Google Drive with Nextcloud
- [ ] Replace iCloud Photos with Immich
- [ ] Host AI chatbot (local LLM)
- [ ] Deploy game servers (Minecraft)
- [ ] Set up development environment

### Security & Learning Goals
- [ ] Build isolated malware analysis lab
- [ ] Practice Red Team techniques
- [ ] Implement VPN for remote access
- [ ] Set up monitoring and alerting
- [ ] Document everything for portfolio

---

## 🖥️ Hardware Specifications

### Homelab Server
| Component | Specification |
|-----------|--------------|
| **CPU** | AMD Ryzen 5 5600X (6C/12T) |
| **RAM** | 32GB DDR4 |
| **GPU** | AMD RX 580 8GB (for AI workloads) |
| **Boot Drive** | 250GB SATA SSD (Proxmox host) |
| **VM Storage** | 2x 1TB NVMe SSD (ZFS mirror) |
| **Data Storage** | 8TB HDD (Nextcloud, Immich) |
| **Additional Storage** | 2TB HDD (extra VMs) |
| **Network** | 1Gbps onboard NIC |

### Gaming PC "WhiteSnow" (Separate)
- **CPU**: AMD Ryzen 7 7800X3D
- **Purpose**: Personal gaming (not part of homelab)

### Network Equipment
- **Router**: V60 Intel J4125 (planned - end of year)
  - 4x Intel i226-V 2.5Gbps NICs
  - pfSense firewall OS
- **Switch**: TP-Link TL-SG108E (8-port managed, VLAN capable)
- **DNS/Ad-blocking**: Raspberry Pi 4 8GB (planned - early 2025)
  - Pi-hole for network-wide ad blocking

### Internet Connection
- **ISP**: Time Internet
- **Speed**: 600 Mbps down / 200 Mbps up

---

## 💾 Storage Architecture

### Current Storage Configuration

| Storage ID | Type | Device | Size | Purpose |
|------------|------|--------|------|---------|
| `local` | Directory | 250GB SSD | 232GB | Proxmox system, ISOs |
| `vmpool-fast` | ZFS Mirror | 2x 1TB NVMe | 860GB | Primary VM storage (fast, redundant) |
| `vm-storage` | Directory | 2TB HDD | 1.8TB | Additional VM storage |
| `data-storage` | Directory | 8TB HDD | 7.3TB | User data (Nextcloud, Immich) |

**Total Usable Storage**: ~10TB

### Storage Strategy
- **Performance Tier**: ZFS mirror on NVMe (critical/production VMs)
- **Capacity Tier**: HDD storage (less critical VMs, data)
- **Backup Strategy**: Daily automated backups to data-storage
- **Future**: USB dock with 2x 4TB for off-site backups

### ZFS Benefits
- ✅ **Redundancy**: Mirror protects against drive failure
- ✅ **Performance**: Read from both drives simultaneously
- ✅ **Integrity**: Built-in checksums detect corruption
- ✅ **Snapshots**: Instant, space-efficient snapshots
- ✅ **Compression**: LZ4 compression saves space

---

## 🌐 Network Architecture

### Current Architecture (Phase 1)
```
[ISP Router] (192.168.1.1)
     |
[TP-Link TL-SG108E Switch]
     |
     ├── Homelab Server (Proxmox)
     ├── WhiteSnow (Gaming PC)
     └── Work Laptop
```

### Target Architecture (Phase 5+)
```
[ISP Modem]
     |
[V60 pfSense Router] (4x 2.5Gbps NICs)
  ├─ WAN: ISP Connection
  ├─ Port 2: LAN trunk → Switch
  ├─ Port 3: DMZ (exposed services)
  └─ Port 4: Management
     |
[TP-Link Switch with VLANs]
     |
     ├─ VLAN 1: Management
     ├─ VLAN 10: Main Network
     ├─ VLAN 20: Homelab Services
     ├─ VLAN 30: DMZ
     └─ VLAN 40: Malware Lab (isolated)
```

### Planned VLAN Design

| VLAN ID | Name | Subnet | Purpose |
|---------|------|--------|---------|
| 1 | Management | 192.168.1.0/24 | Infrastructure management |
| 10 | Main | 192.168.10.0/24 | Primary user network |
| 20 | Homelab | 192.168.20.0/24 | Self-hosted services |
| 30 | DMZ | 192.168.30.0/24 | Internet-exposed services |
| 40 | Malware Lab | 192.168.40.0/24 | Isolated security testing |

---

## 🚀 Services & Applications

### Infrastructure Services
- [x] **Proxmox VE 8.x** - Hypervisor platform
- [ ] **pfSense** - Firewall and router (V60 arriving end of year)
- [ ] **Pi-hole** - DNS and network-wide ad blocking
- [ ] **Nginx Proxy Manager** - Reverse proxy with SSL
- [ ] **WireGuard VPN** - Secure remote access
- [ ] **Grafana + Prometheus** - Monitoring and metrics

### Self-Hosted Applications
- [ ] **Nextcloud** - File storage and sync (Google Drive replacement)
- [ ] **Immich** - Photo management and backup (iCloud Photos replacement)
- [ ] **Vaultwarden** - Password manager
- [ ] **Uptime Kuma** - Service monitoring

### Development & AI
- [ ] **Ollama** - Local LLM hosting
  - Models: Llama 3.2, Mistral 7B
- [ ] **Open WebUI** - Web interface for AI chatbot
- [ ] **Code Server** - VS Code in browser

### Gaming & Entertainment
- [ ] **Pterodactyl Panel** - Game server management
- [ ] **Minecraft Server** - Java Edition (public-facing)
- [ ] **Plex/Jellyfin** - Media server

### Security Lab
- [ ] **Isolated Malware Analysis Environment** (VLAN 40)
  - Windows 10 VM (victim machine)
  - Kali Linux VM (attack machine)
  - REMnux VM (malware analysis)
  - Network monitoring and capture

---

## 📊 Current Status

### ✅ Phase 1: Foundation (COMPLETE)
**Duration**: December 20-21, 2024  
**Status**: ✅ Complete

- [x] Install Proxmox VE
- [x] Configure ZFS mirror storage on NVMe drives
- [x] Mount and configure HDD storage
- [x] Create first VM (Ubuntu Server 22.04)
- [x] Test ZFS snapshots
- [x] Configure automated backups

**Details**: [Phase 1 Completion Report](./docs/phase-1-completion.md)

### 🔄 Phase 2: Network Segmentation (Planned)
**Expected**: December 2024  
**Status**: 📅 Pending

- [ ] Design VLAN architecture
- [ ] Configure TP-Link switch VLANs
- [ ] Set up VLAN-aware bridges in Proxmox
- [ ] Test inter-VLAN routing

### 📅 Phase 3: Core Services (Planned)
**Expected**: Late December 2024

- [ ] Deploy Nextcloud
- [ ] Deploy Immich
- [ ] Configure Nginx Proxy Manager
- [ ] Set up SSL certificates

### 📅 Phase 4-9: Advanced Configuration
**Expected**: January - February 2025

See [Project Roadmap](./docs/roadmap.md) for complete timeline.

---

## 📚 Documentation

### Phase Guides
- [Phase 1: Proxmox Foundation](./docs/phase-1-completion.md) ✅
- [Phase 2: Network Segmentation](./docs/phase-2-vlans.md) 📅
- [Phase 3: Core Services](./docs/phase-3-services.md) 📅
- [Phase 4-9: Advanced Setup](./docs/roadmap.md) 📅

### Technical Documentation
- [Storage Configuration](./docs/storage-configuration.md)
- [Network Architecture](./docs/network-architecture.md)
- [Backup Strategy](./docs/backup-strategy.md)
- [Troubleshooting Guide](./docs/troubleshooting.md)

### Learning Resources
- [Lessons Learned](./docs/lessons-learned.md)
- [Useful Commands](./docs/commands-reference.md)
- [Video Tutorials Used](./docs/video-resources.md)

---

## 🎓 Skills & Technologies

### What I'm Learning
- ✅ **Virtualization**: Proxmox VE, KVM, QEMU
- ✅ **Storage**: ZFS, LVM, filesystem management
- 🔄 **Networking**: VLANs, routing, firewalling
- 📅 **Containerization**: Docker, LXC
- 📅 **Linux Administration**: Ubuntu, Debian
- 📅 **Security**: Firewall rules, network segmentation, malware analysis
- 📅 **Infrastructure as Code**: Automation, scripting
- 📅 **Monitoring**: Grafana, Prometheus

### Career Relevance
This homelab directly supports my role at F-Secure by providing hands-on experience with:
- Enterprise infrastructure management
- Security best practices and implementation
- Network architecture and segmentation
- Incident response and malware analysis
- Service reliability and backup strategies

---

## 📈 Project Timeline

| Phase | Dates | Status | Key Achievements |
|-------|-------|--------|------------------|
| **Phase 0** | Dec 19-20, 2024 | ✅ Complete | Planning, hardware inventory |
| **Phase 1** | Dec 20-21, 2024 | ✅ Complete | Proxmox installation, ZFS storage, first VM |
| **Phase 2** | Dec 22-26, 2024 | 📅 Planned | VLAN configuration |
| **Phase 3** | Dec 27-31, 2024 | 📅 Planned | Nextcloud, Immich deployment |
| **Phase 4** | Dec 31 - Jan 5, 2025 | 📅 Planned | Optimization, monitoring |
| **Phase 5** | Jan 6-12, 2025 | 📅 Planned | pfSense deployment (V60 arrival) |
| **Phase 6** | Jan 13-19, 2025 | 📅 Planned | Pi-hole deployment |
| **Phase 7** | Jan 20-31, 2025 | 📅 Planned | AI chatbot, game servers |
| **Phase 8** | Feb 1-14, 2025 | 📅 Planned | Security lab setup |
| **Phase 9** | Feb 15-28, 2025 | 📅 Planned | Final optimization |

**Total Project Duration**: ~10-12 weeks  
**Total Budget**: ~RM 1,080 (hardware upgrades)

---

## 💰 Budget Breakdown

| Item | Cost (RM) | Status | Purpose |
|------|-----------|--------|---------|
| Homelab PC (existing) | 0 | ✅ Owned | Main server |
| Storage drives (existing) | 0 | ✅ Owned | 2x NVMe, 3x HDD |
| TP-Link switch (existing) | 0 | ✅ Owned | Network equipment |
| V60 J4125 Router | 650-800 | 🛒 Dec 2024 | pfSense firewall |
| Raspberry Pi 4 8GB | 280-340 | 🛒 Jan 2025 | Pi-hole DNS |
| **Total Investment** | **930-1,140** | | |

---

## 🎯 Success Metrics

### Technical Metrics
- ✅ System uptime: 99%+ target
- ✅ Backup success rate: 100%
- ✅ Storage redundancy: RAID1 (ZFS mirror)
- 📊 VM deployment time: < 10 minutes
- 📊 Snapshot creation: < 5 seconds (ZFS)

### Learning Metrics
- ✅ Technologies mastered: 5+
- ✅ Documentation quality: Comprehensive
- ✅ Portfolio projects: Multiple working services
- 📈 GitHub contributions: Regular commits

---

## 🤝 Contributing

While this is a personal learning project, I welcome:
- Suggestions for improvements
- Security recommendations
- Best practices feedback
- Questions about implementation

Feel free to open an issue or reach out!

---

## 📞 Contact

- **GitHub**: [@muzakkir97](https://github.com/muzakkir97)
- **LinkedIn**: [Connect with me](https://www.linkedin.com/in/muzakkir-mohamad-yunos-218661144/)
- **Email**: Available on request
- **Domain**: [najhin-gaming.com](https://najhin-gaming.com)

---

## 🙏 Acknowledgments

### Learning Resources
- [Proxmox VE Documentation](https://pve.proxmox.com/wiki/)
- [TechnoTim YouTube Channel](https://www.youtube.com/@TechnoTim)
- [Learn Linux TV](https://www.youtube.com/@LearnLinuxTV)
- [r/homelab Community](https://reddit.com/r/homelab)
- [r/proxmox Community](https://reddit.com/r/Proxmox)

### Inspiration
- r/homelab community for inspiration and guidance
- Colleagues at F-Secure for encouragement
- Various YouTube creators for excellent tutorials

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Documentation and configurations are provided as-is for educational purposes.

---

## 📝 Project Notes

### Why This Project?
1. **Career Development**: Hands-on experience relevant to cybersecurity role
2. **Cost Savings**: Replace subscription services (Google Drive, iCloud)
3. **Privacy**: Full control over personal data
4. **Learning**: Deep understanding of enterprise infrastructure
5. **Portfolio**: Demonstrable technical skills for career growth

### Why Document Everything?
- Knowledge retention and reference
- Portfolio building for career advancement
- Helping others on similar journeys
- Accountability and progress tracking

---

**Last Updated**: December 21, 2024  
**Project Status**: 🟢 Active Development  
**Current Phase**: Phase 1 Complete ✅
