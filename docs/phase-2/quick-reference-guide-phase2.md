# Homelab Quick Reference Guide - Phase 2

## 🔧 Essential Information

### Network Devices
| Device | IP Address | Access Method | Credentials |
|--------|-----------|---------------|-------------|
| Router | 192.168.100.1 | Web UI | Your ISP credentials |
| Switch | 192.168.100.2 | http://192.168.100.2 | admin / [your password] |
| Proxmox | 192.168.100.25 | https://192.168.100.25:8006 | root / [your password] |
| Gaming PC | 192.168.100.49 | Local | - |

### VLAN Configuration
| VLAN ID | Name | Subnet | Purpose | Current Status |
|---------|------|--------|---------|----------------|
| 1 | Management | 192.168.100.0/24 | Management network | ✅ Active |
| 10 | Main_Netwo | TBD | Client devices | ⏳ Ready (Phase 3) |
| 20 | Homelab_se | TBD | Self-hosted services | ⏳ Ready (Phase 3) |
| 30 | DMZ | TBD | Public-facing services | ⏳ Ready (Phase 3) |
| 40 | Malware_La | TBD | Malware analysis lab | ⏳ Ready (Phase 3) |

---

## 🔍 Quick Diagnostics

### Proxmox Commands

**Check VLAN-aware bridge status:**
```bash
# Should output: 1 (enabled)
cat /sys/class/net/vmbr0/bridge/vlan_filtering
```

**View bridge VLAN configuration:**
```bash
# Shows which VLANs are configured on the bridge
bridge vlan show
```

**Check network interface:**
```bash
# Shows IP address and status
ip addr show vmbr0
```

**Check network connectivity:**
```bash
# Test key devices
ping -c 3 192.168.100.1    # Router
ping -c 3 192.168.100.2    # Switch
ping -c 3 192.168.100.49   # Gaming PC
```

**Restart networking (if needed):**
```bash
# Use with caution - will disconnect briefly
systemctl restart networking

# Or reload configuration
ifreload -a
```

**View network configuration file:**
```bash
# Main network config
cat /etc/network/interfaces

# Edit if needed
nano /etc/network/interfaces
```

---

### Windows Commands (Gaming PC)

**Check IP configuration:**
```powershell
ipconfig
ipconfig /all  # Detailed info
```

**Release and renew DHCP:**
```powershell
ipconfig /release
ipconfig /renew
```

**Flush DNS cache:**
```powershell
ipconfig /flushdns
```

**View routing table:**
```powershell
route print
```

**Test connectivity:**
```powershell
ping 192.168.100.1   # Router
ping 192.168.100.2   # Switch
ping 192.168.100.25  # Proxmox
ping 8.8.8.8         # Internet
```

**Traceroute:**
```powershell
tracert 192.168.100.25
```

**View ARP table:**
```powershell
arp -a
```

**Reset network adapter:**
```powershell
# Disable adapter
netsh interface set interface "Ethernet 2" admin=disable

# Wait 5 seconds

# Enable adapter
netsh interface set interface "Ethernet 2" admin=enable
```

---

## 🔌 Switch Port Configuration Reference

### Current Port Assignments

| Port | Device | PVID | VLAN Membership | Link Type |
|------|--------|------|-----------------|-----------|
| 1 | Router | 1 | VLAN 1 (Untagged) | Access |
| 2 | Proxmox | 1 | VLAN 1 (Untagged)<br>VLAN 10,20,30,40 (Tagged) | Trunk |
| 3 | Work Laptop | 10 | VLAN 10 (Untagged) | Access |
| 4 | Gaming PC | 1 | VLAN 1 (Untagged) | Access |
| 5-8 | Available | 1 | VLAN 1 (Untagged) | Access |

### Switch Access

**Web Interface:**
```
URL: http://192.168.100.2
Username: admin
Password: [your password]
```

**Key Pages:**
- **VLAN Config:** VLAN → 802.1Q VLAN → VLAN Config
- **Port Config:** VLAN → 802.1Q VLAN → Port Config
- **PVID Settings:** VLAN → 802.1Q VLAN → Port PVID
- **Backup Config:** System Tools → Config Files

---

## 🚨 Emergency Procedures

### Lost Access to Switch

**Method 1: Access via VLAN 1 device**
```
1. Connect PC to any port (default VLAN 1)
2. Set PC IP: 192.168.100.x (x = 50-254)
3. Access: http://192.168.100.2
```

**Method 2: Factory reset**
```
WARNING: This erases all configuration!

1. Power off switch
2. Hold reset button
3. Power on switch while holding button
4. Keep holding for 10 seconds
5. Release - switch resets to defaults
6. Default IP: 192.168.0.1
7. Default credentials: admin / admin
8. Reconfigure from backup
```

---

### Lost Access to Proxmox

**Method 1: Check network connectivity**
```bash
# From another device on VLAN 1:
ping 192.168.100.25

# If ping fails, check:
1. Switch Port 2 configuration
2. Network cable connected
3. Proxmox server powered on
```

**Method 2: Physical console access**
```
1. Connect monitor and keyboard to Proxmox
2. Login at console (root / password)
3. Check network status:
   ip addr show
   systemctl status networking
4. Restart networking if needed:
   systemctl restart networking
```

**Method 3: Restore network config**
```bash
# If you have backup
cp /etc/network/interfaces.backup.YYYYMMDD /etc/network/interfaces
systemctl restart networking
```

---

### Network Configuration Rollback

**Switch:**
```
1. Login to switch web interface
2. System Tools → Config Files
3. Upload backed-up .bin file
4. Reboot switch
```

**Proxmox:**
```bash
# Restore from backup
cp /etc/network/interfaces.backup.YYYYMMDD /etc/network/interfaces
systemctl restart networking

# Or manually edit
nano /etc/network/interfaces
# Remove VLAN-aware settings if needed
systemctl restart networking
```

---

## 📝 Common Configuration Snippets

### Proxmox vmbr0 Configuration (VLAN-Aware)

**/etc/network/interfaces:**
```
auto vmbr0
iface vmbr0 inet static
        address 192.168.100.25/24
        gateway 192.168.100.1
        bridge-ports eno1
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094
```

### Proxmox VM Network Configuration

**For VM on VLAN 10:**
```
Hardware:
├─ Network Device (net0)
   ├─ Bridge: vmbr0
   ├─ VLAN Tag: 10
   ├─ Model: VirtIO (paravirtualized)
   └─ Firewall: Enabled (optional)
```

**For VM on VLAN 20:**
```
Hardware:
├─ Network Device (net0)
   ├─ Bridge: vmbr0
   ├─ VLAN Tag: 20
   └─ [rest same as above]
```

---

## 🔐 Security Best Practices

### Switch Security
```
✓ Change default admin password
✓ Enable HTTPS for web interface (if available)
✓ Disable unused ports
✓ Regular configuration backups
✓ Restrict management VLAN access
```

### Proxmox Security
```
✓ Use strong root password
✓ Enable firewall
✓ Keep system updated (apt update && apt upgrade)
✓ Use SSH key authentication (disable password)
✓ Regular backups
✓ Restrict web interface access to management VLAN
```

### Network Security
```
✓ Separate management from production VLANs
✓ Default deny firewall rules (Phase 3)
✓ Monitor logs for suspicious activity
✓ Document all configuration changes
✓ Test changes in isolated environment first
```

---

## 📊 Monitoring & Logging

### Proxmox Logs

**View system logs:**
```bash
# General system log
journalctl -xe

# Network-related logs
journalctl -u networking

# Last 100 lines
journalctl -n 100

# Follow log in real-time
journalctl -f
```

**Web UI Logs:**
```
Node → System → Syslog
```

---

### Switch Monitoring

**Web UI:**
```
Monitoring → Port Statistics
Monitoring → Port Status
System → Log
```

---

## 🔄 Regular Maintenance Tasks

### Weekly
```
□ Check Proxmox updates available
□ Review system logs for errors
□ Verify all critical services online
□ Test backup restoration (once/month)
```

### Monthly
```
□ Backup switch configuration
□ Backup Proxmox configuration
□ Review network topology (any changes?)
□ Update documentation
□ Check disk space on Proxmox
```

### Quarterly
```
□ Apply Proxmox updates (after testing)
□ Review and update firewall rules
□ Audit user access
□ Verify disaster recovery plan
```

---

## 🆘 Troubleshooting Decision Tree

### Cannot Access Device

```
Can you ping the device?
├─ YES → Web interface issue
│  ├─ Check browser (try different browser)
│  ├─ Clear browser cache
│  ├─ Check HTTPS/HTTP
│  └─ Verify credentials
│
└─ NO → Network connectivity issue
   ├─ Check cable connected
   ├─ Check link lights on switch port
   ├─ Verify device powered on
   ├─ Check VLAN configuration
   │  ├─ Same VLAN as source device?
   │  ├─ Correct PVID?
   │  └─ Proper trunk/access port?
   └─ Check IP configuration
      ├─ Correct subnet?
      ├─ Correct gateway?
      └─ No IP conflicts?
```

### VM Cannot Access Network

```
VM cannot reach internet
├─ Can VM ping its gateway (vmbr0 IP)?
│  ├─ YES → Issue outside VM
│  │  └─ Check Proxmox → Router connectivity
│  └─ NO → VM network config issue
│     ├─ Check VM VLAN tag correct
│     ├─ Check VM IP/gateway/DNS config
│     └─ Check VM firewall rules
```

---

## 📚 Useful Resources

### Official Documentation
- **Proxmox:** https://pve.proxmox.com/wiki/Network_Configuration
- **TP-Link Switch:** https://www.tp-link.com/support/download/
- **VLAN Tutorial:** https://en.wikipedia.org/wiki/IEEE_802.1Q

### Community Resources
- **Reddit r/homelab:** https://reddit.com/r/homelab
- **Reddit r/Proxmox:** https://reddit.com/r/Proxmox
- **ServeTheHome:** https://www.servethehome.com/

### Your Documentation
- **GitHub Repository:** github.com/muzakkir97/homelab-infrastructure
- **Network Topology:** `/docs/network-topology-phase2.md`
- **Phase 2 Guide:** `/docs/phase-2-vlan-segmentation-complete.md`

---

## 💾 Backup Locations

### Configuration Backups
```
Switch Config:
└─ /home/user/homelab-backups/phase2/TP-Link-SG108E-VLAN-Config-YYYY-MM-DD.bin

Proxmox Network Config:
└─ /etc/network/interfaces.backup.YYYYMMDD

Documentation:
└─ github.com/muzakkir97/homelab-infrastructure
```

### Backup Schedule
```
□ Switch: After any VLAN changes
□ Proxmox: Daily (automatic via cron)
□ Documentation: After each phase completion
```

---

## 📞 Support Contacts

### Hardware Support
- **TP-Link Support:** https://www.tp-link.com/support/
- **Local IT Store:** [Your local computer shop]

### Online Communities
- **r/homelab Discord:** [Discord invite link if member]
- **Proxmox Forums:** https://forum.proxmox.com/

---

## ✅ Phase 2 Quick Status Check

Run this checklist to verify Phase 2 is operational:

```bash
# On Proxmox, run:
echo "=== Proxmox Network Status ==="
cat /sys/class/net/vmbr0/bridge/vlan_filtering
bridge vlan show
ip addr show vmbr0
echo ""
echo "=== Connectivity Tests ==="
ping -c 3 192.168.100.1
ping -c 3 192.168.100.2
ping -c 3 192.168.100.49

# Expected output:
# VLAN filtering: 1 (enabled)
# Bridge shows VLANs 1,10,20,30,40
# IP: 192.168.100.25/24
# All pings: 0% packet loss
```

**If all green:** Phase 2 is healthy ✅  
**If any red:** See troubleshooting section ⚠️

---

*Last Updated: Phase 2 Completion - January 2025*
*Next Phase: pfSense Integration (Phase 3)*
