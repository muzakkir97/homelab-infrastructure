# Phase 4 Planning: Core Infrastructure & Remote Access

> **Project:** Enterprise Homelab Infrastructure  
> **Author:** Muzakkir  
> **Planning Date:** January 2026  
> **Status:** 📝 Planning Complete - Ready for Deployment

---

## 📋 Table of Contents

1. [Executive Summary](#executive-summary)
2. [Objectives](#objectives)
3. [Prerequisites](#prerequisites)
4. [Architecture Overview](#architecture-overview)
5. [Component Specifications](#component-specifications)
6. [Network Configuration](#network-configuration)
7. [Storage Strategy](#storage-strategy)
8. [Deployment Order & Dependencies](#deployment-order--dependencies)
9. [Resource Allocation](#resource-allocation)
10. [Security Considerations](#security-considerations)
11. [Risk Assessment](#risk-assessment)
12. [Timeline Estimate](#timeline-estimate)
13. [Success Criteria](#success-criteria)
14. [Planning Decisions Log](#planning-decisions-log)
15. [Open Items](#open-items)

---

## Executive Summary

Phase 4 establishes the foundational services layer of the homelab infrastructure. Upon completion, the environment will have:

- **Network-wide DNS filtering** via Pi-hole for ad-blocking and security
- **Centralized network storage** via UGREEN NAS for file sharing and backups
- **Reverse proxy with SSL** via Nginx Proxy Manager for secure service access
- **Secure remote access** via Tailscale VPN without port forwarding
- **Remote desktop capabilities** via Windows and Linux VMs accessible from anywhere

This phase transforms the network from basic connectivity (Phases 1-3) to a fully functional self-hosted environment ready for application deployment in Phase 5.

---

## Objectives

### Primary Objectives

| Objective | Success Metric |
|-----------|----------------|
| Deploy DNS filtering | Pi-hole blocking ads network-wide |
| Establish network storage | NAS accessible from all VLANs |
| Enable SSL for services | Valid certificates via Let's Encrypt |
| Provide remote access | Access homelab from anywhere securely |
| Create remote desktop VMs | Windows & Linux accessible remotely |

### Learning Objectives

| Skill Area | What I'll Learn |
|------------|-----------------|
| DNS | How DNS works, ad-blocking at network level |
| Storage | NAS configuration, RAID, SMB/NFS shares |
| Reverse Proxy | SSL termination, proxy hosts, certificate automation |
| VPN | WireGuard protocol, zero-trust networking |
| Virtualization | VM creation, remote desktop protocols |
| Linux Administration | Headless setup, static IPs, service management |
| Automation | Bash scripting for health checks and backups |

---

## Prerequisites

### Completed Phases

| Phase | Description | Status |
|-------|-------------|--------|
| Phase 1 | Proxmox VE Installation | ✅ Complete |
| Phase 2 | VLAN Network Segmentation | ✅ Complete |
| Phase 3 | pfSense Router-on-a-Stick | ✅ Complete |

### Hardware Inventory

| Item | Specification | Purpose | Status |
|------|---------------|---------|--------|
| Raspberry Pi 4 | 4GB RAM | Pi-hole DNS | ✅ Ready |
| MicroSD Card | 32GB+ Class 10 | Pi OS storage | ✅ Ready |
| Pi Power Supply | 5V 3A USB-C | Pi power | ✅ Ready |
| UGREEN NASync | 2-bay NAS | Network storage | ✅ Ready |
| NAS Drives | 2x 8TB WD Purple (Used) | NAS storage | 🟡 In Transit |
| Ethernet Cables | Cat5e or better | Network connections | ✅ Ready |

### Software & Accounts Required

| Item | Purpose | Status |
|------|---------|--------|
| Raspberry Pi Imager | Flash Pi OS | ✅ Downloaded |
| Tailscale Account | VPN service | ⬜ To Create |
| Cloudflare Account | DNS management | 🟡 To Verify |
| Domain Name | SSL certificates | 🟡 najhin-gaming.com (needs investigation) |
| Windows 11 ISO | Windows VM | ⬜ To Download |
| Ubuntu Desktop ISO | Linux VM | ⬜ To Download |

### Network Access Verified

| Access Point | IP Address | Status |
|--------------|------------|--------|
| Proxmox Web UI | 192.168.1.5:8006 | ✅ Accessible |
| pfSense Web UI | 192.168.1.1 | ✅ Accessible |
| Switch Management | 192.168.1.20 | ✅ Accessible |

---

## Architecture Overview

### Current State (After Phase 3)

```
                         ┌─────────────────┐
                         │    INTERNET     │
                         └────────┬────────┘
                                  │
                         ┌────────┴────────┐
                         │   ISP Router    │
                         └────────┬────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    │      pfSense Router       │
                    │      192.168.1.1          │
                    └─────────────┬─────────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    │   TP-Link Managed Switch  │
                    │      192.168.1.20         │
                    └─────────────┬─────────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    │      Proxmox Server       │
                    │      192.168.1.5          │
                    │                           │
                    │   (No services deployed)  │
                    └───────────────────────────┘
```

### Target State (After Phase 4)

```
                              ┌─────────────────┐
                              │    INTERNET     │
                              └────────┬────────┘
                                       │
                              ┌────────┴────────┐
                              │   ISP Router    │
                              └────────┬────────┘
                                       │
                         ┌─────────────┴─────────────┐
                         │      pfSense Router       │
                         │   + Tailscale VPN (4D)    │
                         │      192.168.1.1          │
                         └─────────────┬─────────────┘
                                       │
                         ┌─────────────┴─────────────┐
                         │   TP-Link Managed Switch  │
                         │      192.168.1.20         │
                         └──┬─────┬─────┬─────┬──────┘
                            │     │     │     │
      ┌─────────────────────┤     │     │     │
      │                     │     │     │     │
┌─────┴─────┐    ┌──────────┴─┐   │     │     │
│  VLAN 1   │    │  VLAN 20   │   │     │     │
│Management │    │  Services  │   │     │     │
│           │    │            │   │     │     │
│ Proxmox   │    │ Pi-hole    │   │     │     │
│ 192.168.  │    │ 192.168.   │   │     │     │
│ 1.5       │    │ 20.10      │   │     │     │
│           │    │ (4A)       │   │     │     │
│ NAS       │    │            │   │     │     │
│ 192.168.  │    │ Win/Linux  │   │     │     │
│ 1.50      │    │ VMs (4E)   │   │     │     │
│ (4B)      │    │            │   │     │     │
└───────────┘    └────────────┘   │     │     │
                                  │     │     │
                   ┌──────────────┴─┐   │     │
                   │   VLAN 30      │   │     │
                   │     DMZ        │   │     │
                   │                │   │     │
                   │ Nginx Proxy    │   │     │
                   │ Manager        │   │     │
                   │ 192.168.30.10  │   │     │
                   │ (4C)           │   │     │
                   └────────────────┘   │     │
                                        │     │
                        ┌───────────────┴─┐   │
                        │   VLAN 40       │   │
                        │  Malware Lab    │   │
                        │  (Isolated)     │   │
                        │  (Future Use)   │   │
                        └─────────────────┘   │
                                              │
                              ┌───────────────┴─┐
                              │   VLAN 10       │
                              │  Main Network   │
                              │  (Daily Use)    │
                              └─────────────────┘
```

### Data Flow Diagram

```
User Query: "google.com"
━━━━━━━━━━━━━━━━━━━━━━━━

Device (Any VLAN)
    │
    ▼
Pi-hole (192.168.20.10)
    │
    ├── Blocked domain? → Return 0.0.0.0
    │
    └── Allowed domain? → Forward to Cloudflare (1.1.1.1)
                              │
                              ▼
                         Internet
                              │
                              ▼
                         Response back to device


Remote Access Flow:
━━━━━━━━━━━━━━━━━━

User (Remote Location)
    │
    ▼
Tailscale Client
    │
    ▼ (Encrypted WireGuard tunnel)
    │
pfSense (Tailscale subnet router)
    │
    ▼
Internal Services (Pi-hole, VMs, NAS, etc.)


Web Service Access Flow:
━━━━━━━━━━━━━━━━━━━━━━━━

User → https://service.najhin-gaming.com
    │
    ▼
Cloudflare DNS → Points to home IP
    │
    ▼
pfSense → Port forwards 443 to NPM
    │
    ▼
Nginx Proxy Manager (192.168.30.10)
    │
    ├── Terminates SSL
    ├── Checks proxy host rules
    │
    ▼
Backend Service (e.g., 192.168.20.20)
```

---

## Component Specifications

### Phase 4A: Pi-hole DNS

| Specification | Value |
|---------------|-------|
| **Hardware** | Raspberry Pi 4 (4GB RAM) |
| **Operating System** | Raspberry Pi OS Lite (64-bit) |
| **Storage** | SD Card + Log2RAM (USB SSD later) |
| **Network** | VLAN 20 (Homelab Services) |
| **IP Address** | 192.168.20.10 (Static) |
| **Gateway** | 192.168.20.1 |
| **Upstream DNS** | Cloudflare (1.1.1.1, 1.0.0.1) |
| **Web Interface** | http://192.168.20.10/admin |
| **Purpose** | Network-wide ad blocking, DNS filtering, local DNS |

**Features to Configure:**
- Default blocklists enabled
- Query logging enabled
- DHCP disabled (pfSense handles DHCP)
- All VLANs configured to use Pi-hole as DNS

---

### Phase 4B: UGREEN NAS

| Specification | Value |
|---------------|-------|
| **Hardware** | UGREEN NASync (2-bay) |
| **Drives** | 2x 8TB WD Purple (Used) |
| **RAID Configuration** | RAID 1 (Mirror) |
| **Usable Capacity** | ~8TB |
| **Network** | VLAN 1 (Management) |
| **IP Address** | 192.168.1.50 (Static) |
| **Gateway** | 192.168.1.1 |
| **Protocols** | SMB, NFS |
| **Purpose** | File storage, Proxmox backups, media storage |

**Shares to Configure:**

| Share Name | Purpose | Access |
|------------|---------|--------|
| proxmox-backups | VM/Container backups | Proxmox only |
| documents | Personal documents | Authenticated users |
| media | Media files | Read for most, write for admin |
| public | General file sharing | All users |

**Firewall Rules Required:**
- VLAN 1 → NAS: Full access (management)
- VLAN 10 → NAS: SMB access (personal devices)
- VLAN 20 → NAS: SMB/NFS access (services)
- VLAN 30 → NAS: No access (DMZ isolated)
- VLAN 40 → NAS: No access (Malware Lab isolated)

---

### Phase 4C: Nginx Proxy Manager

| Specification | Value |
|---------------|-------|
| **Platform** | LXC Container on Proxmox |
| **Container ID** | 200 |
| **Operating System** | Debian 12 (Bookworm) |
| **RAM** | 512MB |
| **Storage** | 4GB on vmpool-fast |
| **CPU** | 1 core |
| **Network** | VLAN 30 (DMZ) |
| **IP Address** | 192.168.30.10 (Static) |
| **Gateway** | 192.168.30.1 |
| **Web Interface** | http://192.168.30.10:81 |
| **HTTP Port** | 80 |
| **HTTPS Port** | 443 |
| **Purpose** | Reverse proxy, SSL termination, certificate automation |

**Domain Configuration:**

| Setting | Value |
|---------|-------|
| Domain | najhin-gaming.com |
| Registrar | Exabytes (to verify) |
| DNS Provider | Cloudflare (to migrate) |
| SSL Provider | Let's Encrypt |
| Challenge Type | DNS (Cloudflare API) |

**Initial Proxy Hosts:**

| Subdomain | Backend | Purpose |
|-----------|---------|---------|
| pihole.najhin-gaming.com | 192.168.20.10:80 | Pi-hole admin |
| proxmox.najhin-gaming.com | 192.168.1.5:8006 | Proxmox UI |
| nas.najhin-gaming.com | 192.168.1.50 | NAS web UI |

---

### Phase 4D: Tailscale VPN

| Specification | Value |
|---------------|-------|
| **Platform** | pfSense Package |
| **Account** | Free tier (up to 100 devices) |
| **Subnet Router** | pfSense (advertise all VLANs) |
| **Exit Node** | Disabled (not needed) |
| **MagicDNS** | Enabled |
| **Purpose** | Secure remote access to all internal services |

**Advertised Subnets:**

| Subnet | VLAN | Access Level |
|--------|------|--------------|
| 192.168.1.0/24 | VLAN 1 (Management) | Full |
| 192.168.10.0/24 | VLAN 10 (Main) | Full |
| 192.168.20.0/24 | VLAN 20 (Services) | Full |
| 192.168.30.0/24 | VLAN 30 (DMZ) | Full |
| 192.168.40.0/24 | VLAN 40 (Malware Lab) | Restricted |

**Client Devices to Configure:**
- Windows 11 Gaming PC
- Mobile phone (Android/iOS)
- Work laptop (if permitted)

---

### Phase 4E: Remote Desktop VMs

#### Windows 11 VM

| Specification | Value |
|---------------|-------|
| **VM ID** | 300 |
| **OS** | Windows 11 Pro |
| **RAM** | 8GB |
| **CPU** | 4 cores |
| **Storage** | 60GB on vmpool-fast |
| **Network** | VLAN 20 (Homelab Services) |
| **IP Address** | DHCP (with reservation) |
| **Remote Access** | RDP (port 3389) |
| **Purpose** | Windows testing, general use |

#### Ubuntu Desktop VM

| Specification | Value |
|---------------|-------|
| **VM ID** | 301 |
| **OS** | Ubuntu 24.04 LTS Desktop |
| **RAM** | 4GB |
| **CPU** | 2 cores |
| **Storage** | 40GB on vmpool-fast |
| **Network** | VLAN 20 (Homelab Services) |
| **IP Address** | DHCP (with reservation) |
| **Remote Access** | RDP (xrdp) or VNC |
| **Purpose** | Linux learning, development |

#### Future VMs (Phase 5+)

| VM | Purpose | Priority |
|----|---------|----------|
| Debian | Server administration practice | Medium |
| Linux Mint | Alternative desktop | Low |
| Fedora | RHEL ecosystem learning | Medium |
| Pop!_OS | Development environment | Low |

---

## Network Configuration

### IP Address Assignments

```
VLAN 1 - Management (192.168.1.0/24)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
192.168.1.1       - pfSense (gateway)
192.168.1.5       - Proxmox host
192.168.1.20      - TP-Link Switch
192.168.1.50      - UGREEN NAS [NEW - Phase 4B]
192.168.1.100-254 - DHCP range

VLAN 10 - Main Network (192.168.10.0/24)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
192.168.10.1      - pfSense (gateway)
192.168.10.100-254 - DHCP range (personal devices)

VLAN 20 - Homelab Services (192.168.20.0/24)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
192.168.20.1      - pfSense (gateway)
192.168.20.10     - Pi-hole [NEW - Phase 4A]
192.168.20.20     - (Reserved: Nextcloud)
192.168.20.30     - (Reserved: Immich)
192.168.20.40     - (Reserved: Vaultwarden)
192.168.20.50     - (Reserved: Windows 11 VM) [NEW - Phase 4E]
192.168.20.51     - (Reserved: Ubuntu VM) [NEW - Phase 4E]
192.168.20.100-254 - DHCP range

VLAN 30 - DMZ (192.168.30.0/24)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
192.168.30.1      - pfSense (gateway)
192.168.30.10     - Nginx Proxy Manager [NEW - Phase 4C]
192.168.30.20     - (Reserved: Game servers)
192.168.30.100-254 - DHCP range

VLAN 40 - Malware Lab (192.168.40.0/24)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
192.168.40.1      - pfSense (gateway)
192.168.40.10     - (Reserved: Kali Linux)
192.168.40.20     - (Reserved: Vulnerable VMs)
192.168.40.100-254 - DHCP range
```

### DNS Configuration

| VLAN | DNS Server 1 | DNS Server 2 | Notes |
|------|--------------|--------------|-------|
| VLAN 1 | 192.168.20.10 | 1.1.1.1 | Pi-hole primary |
| VLAN 10 | 192.168.20.10 | 1.1.1.1 | Pi-hole primary |
| VLAN 20 | 192.168.20.10 | 1.1.1.1 | Pi-hole primary |
| VLAN 30 | 192.168.20.10 | 1.1.1.1 | Pi-hole primary |
| VLAN 40 | None or isolated | - | Malware lab isolated |

### Firewall Rules Summary

#### Pi-hole Access (Port 53 UDP/TCP)

| Source | Destination | Action |
|--------|-------------|--------|
| VLAN 1 net | 192.168.20.10:53 | Allow |
| VLAN 10 net | 192.168.20.10:53 | Allow |
| VLAN 20 net | 192.168.20.10:53 | Allow |
| VLAN 30 net | 192.168.20.10:53 | Allow |
| VLAN 40 net | 192.168.20.10:53 | Block |

#### NAS Access (SMB 445, NFS 2049)

| Source | Destination | Action |
|--------|-------------|--------|
| VLAN 1 net | 192.168.1.50:445 | Allow |
| VLAN 10 net | 192.168.1.50:445 | Allow |
| VLAN 20 net | 192.168.1.50:445,2049 | Allow |
| VLAN 30 net | 192.168.1.50 | Block |
| VLAN 40 net | 192.168.1.50 | Block |

#### Nginx Proxy Manager (Ports 80, 443)

| Source | Destination | Action |
|--------|-------------|--------|
| Internet | 192.168.30.10:80,443 | Allow (via port forward) |
| All VLANs | 192.168.30.10:80,443 | Allow |
| 192.168.30.10 | Internal services | Allow (proxy backend) |

---

## Storage Strategy

### Proxmox Storage Allocation

| Storage | Type | Size | Purpose in Phase 4 |
|---------|------|------|-------------------|
| **vmpool-fast** | ZFS | 965 GB | LXC containers, VM OS disks |
| **data-storage** | Directory | 7.94 TB | ISO images, Proxmox backups |
| **vm-storage** | Directory | 1.97 TB | Secondary VM data |
| **local-lvm** | LVM-Thin | 147 GB | Quick test VMs |
| **local** | Directory | 71 GB | Templates, snippets |

### Phase 4 Storage Usage

| Component | Storage Location | Size |
|-----------|------------------|------|
| NPM Container (200) | vmpool-fast | 4 GB |
| Windows 11 VM (300) | vmpool-fast | 60 GB |
| Ubuntu VM (301) | vmpool-fast | 40 GB |
| ISO Images | data-storage | ~15 GB |
| Backups | data-storage | Variable |
| **Total New Usage** | - | **~120 GB** |

### NAS Storage Allocation

| Share | Estimated Size | Purpose |
|-------|----------------|---------|
| proxmox-backups | 500 GB - 2 TB | VM/Container backups |
| documents | 100 GB | Personal files |
| media | 4+ TB | Media library |
| public | 100 GB | Shared files |

### Backup Strategy

| What | Where | Frequency | Retention |
|------|-------|-----------|-----------|
| Proxmox VMs/CTs | data-storage (internal) | Daily | 7 days |
| Proxmox VMs/CTs | NAS (offsite copy) | Weekly | 4 weeks |
| Pi-hole config | NAS | Weekly | 4 weeks |
| NPM config | NAS | Weekly | 4 weeks |
| Critical data | NAS | Continuous | N/A |

---

## Deployment Order & Dependencies

```
┌─────────────────────────────────────────────────────────────────┐
│                    PHASE 4 DEPLOYMENT ORDER                      │
└─────────────────────────────────────────────────────────────────┘

Phase 4A: Pi-hole                    [No dependencies]
    │
    │   Pi-hole provides DNS for all subsequent services.
    │   Once running, all devices benefit from ad-blocking.
    │
    ▼
Phase 4B: NAS                        [No dependencies]
    │                                (Can run parallel with 4A)
    │
    │   NAS provides storage for backups.
    │   Critical to have before deploying more services.
    │
    ▼
Phase 4C: Nginx Proxy Manager        [Depends on: 4A (DNS)]
    │
    │   NPM needs DNS resolution (Pi-hole).
    │   Domain needs to be configured first.
    │   SSL certificates require working DNS.
    │
    ▼
Phase 4D: Tailscale                  [Depends on: 4A, 4C recommended]
    │
    │   Works independently but better after NPM.
    │   Provides remote access to all services.
    │
    ▼
Phase 4E: Remote Desktop VMs         [Depends on: 4A, 4D recommended]
    │
    │   VMs need DNS (Pi-hole).
    │   Remote access via Tailscale.
    │   Most resource-intensive component.
    │
    ▼
PHASE 4 COMPLETE ✓
```

### Dependency Matrix

| Component | Requires | Optional But Recommended |
|-----------|----------|--------------------------|
| 4A: Pi-hole | Nothing | - |
| 4B: NAS | Nothing | - |
| 4C: NPM | 4A (DNS) | 4B (backup config) |
| 4D: Tailscale | Nothing | 4A, 4C |
| 4E: VMs | 4A (DNS) | 4B, 4D |

---

## Resource Allocation

### Proxmox Server Resources

| Resource | Total | Phase 4 Usage | Remaining |
|----------|-------|---------------|-----------|
| **RAM** | 32 GB | 12.5 GB | 19.5 GB |
| **CPU Cores** | 12 | 7 | 5 |
| **Fast Storage** | 965 GB | 104 GB | 861 GB |

### Detailed Allocation

| Component | RAM | CPU | Storage |
|-----------|-----|-----|---------|
| NPM (LXC 200) | 512 MB | 1 core | 4 GB |
| Windows 11 (VM 300) | 8 GB | 4 cores | 60 GB |
| Ubuntu (VM 301) | 4 GB | 2 cores | 40 GB |
| **Total** | **12.5 GB** | **7 cores** | **104 GB** |

### External Devices

| Device | Resources | Notes |
|--------|-----------|-------|
| Raspberry Pi 4 | 4 GB RAM, 4 cores | Dedicated to Pi-hole |
| UGREEN NAS | N/A | 8 TB usable (RAID 1) |

---

## Security Considerations

### Network Security

| Measure | Implementation |
|---------|----------------|
| VLAN Segmentation | Services isolated by function |
| Firewall Rules | Explicit allow, default deny |
| DNS Filtering | Pi-hole blocks malware domains |
| SSL Encryption | All external traffic via HTTPS |
| VPN Access | Tailscale for remote, no port forwarding |

### Authentication & Access

| Service | Authentication | 2FA |
|---------|----------------|-----|
| Pi-hole | Password | ⬜ Not available |
| NAS | Username/Password | ✅ Enable if available |
| NPM | Username/Password | ⬜ Not built-in |
| Tailscale | SSO (Google/Microsoft/etc.) | ✅ Enabled |
| Proxmox | Username/Password | ✅ Enable |
| pfSense | Username/Password | ✅ Enable |

### Sensitive Information Handling

| Information | Protection Method |
|-------------|-------------------|
| Passwords | Password manager, not in documentation |
| API Keys | Environment variables, not in Git |
| IP Addresses | Internal IPs okay, public IP hidden |
| Domain Name | Okay to publish (najhin-gaming.com) |

---

## Risk Assessment

### Technical Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Pi-hole SD card failure | Medium | High | Log2RAM + future USB SSD upgrade |
| NAS drive failure | Low | High | RAID 1 + offsite backup plan |
| DNS becomes single point of failure | Medium | High | Backup DNS (1.1.1.1) in DHCP |
| SSL certificate renewal fails | Low | Medium | Monitor expiry, NPM auto-renewal |
| Tailscale account compromise | Low | High | Enable 2FA on Tailscale account |

### Operational Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Configuration error breaks network | Medium | High | Document all changes, backup configs |
| Blocked legitimate domain | High | Low | Easy whitelist via Pi-hole |
| Run out of Proxmox resources | Low | Medium | Monitor usage, plan upgrades |

### Recovery Procedures

| Failure | Recovery Steps |
|---------|----------------|
| Pi-hole down | Devices fall back to 1.1.1.1 (backup DNS) |
| NAS down | Proxmox continues with internal storage |
| NPM down | Direct IP access to services still works |
| Tailscale down | No remote access until restored |
| VM corrupted | Restore from backup on data-storage/NAS |

---

## Timeline Estimate

### Phase 4 Schedule

| Phase | Estimated Time | Dependencies |
|-------|----------------|--------------|
| 4A: Pi-hole | 2-3 hours | None |
| 4B: NAS | 3-4 hours | NAS drives arrival |
| 4C: NPM | 2-3 hours | Domain investigation |
| 4D: Tailscale | 1-2 hours | None |
| 4E: VMs | 3-4 hours | ISO downloads |
| **Total** | **11-16 hours** | - |

### Suggested Schedule

| Session | Tasks | Duration |
|---------|-------|----------|
| Session 1 | Phase 4A (Pi-hole) | 3 hours |
| Session 2 | Phase 4B (NAS) | 4 hours |
| Session 3 | Phase 4C (NPM) + Domain setup | 4 hours |
| Session 4 | Phase 4D (Tailscale) | 2 hours |
| Session 5 | Phase 4E (VMs) | 4 hours |

---

## Success Criteria

### Phase 4A: Pi-hole ✓

- [ ] Raspberry Pi boots and accessible via SSH
- [ ] Pi-hole web interface accessible
- [ ] DNS queries being logged
- [ ] Ads blocked on test devices
- [ ] All VLANs using Pi-hole as DNS
- [ ] Health check script running (optional)

### Phase 4B: NAS ✓

- [ ] NAS accessible at 192.168.1.50
- [ ] RAID 1 configured and healthy
- [ ] SMB shares accessible from VLAN 1, 10, 20
- [ ] Proxmox backup storage configured
- [ ] Initial backup completed successfully

### Phase 4C: Nginx Proxy Manager ✓

- [ ] NPM container running on Proxmox
- [ ] Web interface accessible at :81
- [ ] Domain DNS configured in Cloudflare
- [ ] SSL certificate obtained for domain
- [ ] At least one proxy host working

### Phase 4D: Tailscale ✓

- [ ] Tailscale installed on pfSense
- [ ] Subnet routes advertised and approved
- [ ] Can access internal services from external device
- [ ] Mobile device connected to Tailscale
- [ ] Access works from outside home network

### Phase 4E: Remote Desktop VMs ✓

- [ ] Windows 11 VM created and running
- [ ] Ubuntu VM created and running
- [ ] RDP/VNC accessible from local network
- [ ] VMs accessible via Tailscale remotely
- [ ] VMs using Pi-hole for DNS

---

## Planning Decisions Log

This section documents key decisions made during the planning phase.

### Decision 1: Pi-hole Storage

| Decision | SD Card + Log2RAM initially, USB SSD upgrade later |
|----------|-----------------------------------------------------|
| **Date** | January 2026 |
| **Rationale** | Start immediately with available hardware; upgrade for reliability later |
| **Alternatives Considered** | Wait for USB SSD, use Proxmox container |
| **Trade-offs** | Slight risk of SD card failure vs. immediate deployment |

### Decision 2: NAS Placement

| Decision | VLAN 1 (Management) instead of VLAN 20 |
|----------|----------------------------------------|
| **Date** | January 2026 |
| **Rationale** | NAS is infrastructure, not a service; needs cross-VLAN access |
| **Alternatives Considered** | VLAN 20 (Services), dedicated Storage VLAN |
| **Trade-offs** | Requires firewall rules for cross-VLAN access |

### Decision 3: WD Purple Drives

| Decision | Accept WD Purple drives for NAS |
|----------|----------------------------------|
| **Date** | January 2026 |
| **Rationale** | Cost-effective used drives; acceptable for homelab |
| **Alternatives Considered** | WD Red, Seagate IronWolf (NAS-optimized) |
| **Trade-offs** | Surveillance drives not ideal for NAS; will monitor with SMART |
| **Mitigation** | RAID 1 + offsite backups + SMART monitoring |

### Decision 4: DNS Provider

| Decision | Cloudflare (1.1.1.1) as upstream DNS |
|----------|--------------------------------------|
| **Date** | January 2026 |
| **Rationale** | Fast, privacy-respecting, reliable |
| **Alternatives Considered** | Google (8.8.8.8), Quad9 (9.9.9.9), DoH |
| **Trade-offs** | Could add DoH later for extra privacy |

### Decision 5: Remote Desktop VMs Scope

| Decision | Start with 2 VMs (Windows 11 + Ubuntu) |
|----------|----------------------------------------|
| **Date** | January 2026 |
| **Rationale** | Learn the process before deploying many VMs |
| **Alternatives Considered** | Deploy all 5+ Linux distros immediately |
| **Trade-offs** | Fewer options initially; easier to manage |

### Decision 6: Scripting Approach

| Decision | Include optional Bash automation scripts |
|----------|------------------------------------------|
| **Date** | January 2026 |
| **Rationale** | Light scripting for automation; I now understand scripting ≠ stressful coding |
| **Alternatives Considered** | GUI-only, Python scripts |
| **Trade-offs** | Learning curve, but valuable skill building |

---

## Open Items

### To Investigate

| Item | Priority | Notes |
|------|----------|-------|
| Domain registrar status | High | Verify najhin-gaming.com at Exabytes |
| Cloudflare account | High | Check if account exists, create if not |
| Domain DNS migration | High | Move DNS to Cloudflare if needed |

### To Download

| Item | Size | Source |
|------|------|--------|
| Windows 11 ISO | ~5 GB | Microsoft official |
| Ubuntu 24.04 Desktop ISO | ~5 GB | Ubuntu official |
| Raspberry Pi Imager | ~20 MB | raspberrypi.com |

### To Purchase/Wait For

| Item | Status | Notes |
|------|--------|-------|
| NAS drives (2x 8TB) | In transit | Required for Phase 4B |
| USB SSD for Pi | Future | Optional upgrade |

### Questions to Answer

| Question | Answer | Status |
|----------|--------|--------|
| Can I access Exabytes account? | TBD | ⬜ To verify |
| Is najhin-gaming.com active? | TBD | ⬜ To verify |
| What switch port for Raspberry Pi? | VLAN 20 access port | ✅ Decided |

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | January 2026 | Muzakkir | Initial planning document |

---

## Appendix A: Quick Reference

### Important IPs

| Device | IP Address | Purpose |
|--------|------------|---------|
| pfSense | 192.168.1.1 | Router/Gateway |
| Proxmox | 192.168.1.5 | Hypervisor |
| Switch | 192.168.1.20 | Network switch |
| Pi-hole | 192.168.20.10 | DNS server |
| NAS | 192.168.1.50 | Storage |
| NPM | 192.168.30.10 | Reverse proxy |

### Important Ports

| Service | Port | Protocol |
|---------|------|----------|
| DNS | 53 | UDP/TCP |
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| SSH | 22 | TCP |
| RDP | 3389 | TCP |
| SMB | 445 | TCP |
| Proxmox UI | 8006 | TCP |
| NPM Admin | 81 | TCP |

### VM/Container IDs

| ID | Name | Type |
|----|------|------|
| 200 | nginx-proxy-manager | LXC |
| 300 | windows-11 | VM |
| 301 | ubuntu-desktop | VM |

---

**Planning Complete - Ready for Phase 4 Deployment! 🚀**
