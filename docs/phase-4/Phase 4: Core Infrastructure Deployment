## Overview

Phase 4 covers the deployment of core homelab services including DNS filtering, reverse proxy, and VPN access. This phase transforms the network infrastructure (built in Phases 1-3) into a functional homelab environment.

**Deployment Period:** January 2026  
**Status:** ✅ Complete (4A, 4C, 4D) | ⏸️ Pending (4B - NAS)

---

## Phase 4 Components

| Sub-Phase | Component | Status | IP Address |
|-----------|-----------|--------|------------|
| 4A | Pi-hole DNS Filtering | ✅ Complete | 192.168.20.10 |
| 4B | NAS Setup | ⏸️ Waiting for drives | TBD |
| 4C | Nginx Proxy Manager | ✅ Complete | 192.168.30.10 |
| 4D | Tailscale VPN | ✅ Complete | 100.110.165.45 |

---

## Network Architecture

```
Internet
    │
    ▼
┌─────────────────┐
│   ISP Router    │ (202.184.33.102 - Public IP)
│                 │ Port Forward: 80, 443 → pfSense
└─────────────────┘
    │
    ▼
┌─────────────────┐
│    pfSense      │ (192.168.100.169 - WAN)
│   N100 Mini PC  │ (192.168.1.1 - LAN Gateway)
│                 │ Tailscale: 100.110.165.45
└─────────────────┘
    │
    ▼ (802.1Q Trunk)
┌─────────────────┐
│  TP-Link Switch │ (192.168.1.20)
│   TL-SG108E     │
└─────────────────┘
    │
    ├── VLAN 1  (Management)     192.168.1.0/24
    │   └── Proxmox (192.168.1.5)
    │
    ├── VLAN 10 (Main Network)   192.168.10.0/24
    │
    ├── VLAN 20 (Homelab)        192.168.20.0/24
    │   └── Pi-hole (192.168.20.10)
    │
    ├── VLAN 30 (DMZ)            192.168.30.0/24
    │   └── NPM (192.168.30.10)
    │
    └── VLAN 40 (Malware Lab)    192.168.40.0/24 (Isolated)
```

---

## Phase 4A: Pi-hole DNS Filtering

### Hardware
- **Device:** Raspberry Pi 4 (4GB RAM)
- **Storage:** MicroSD Card (32GB)
- **OS:** Raspberry Pi OS (Debian Trixie - 64-bit)
- **Network:** Ethernet on VLAN 20

### Configuration

| Setting | Value |
|---------|-------|
| IP Address | 192.168.20.10 |
| Subnet | 255.255.255.0 (/24) |
| Gateway | 192.168.20.1 |
| Upstream DNS | Cloudflare (1.1.1.1) |
| DNSSEC | Enabled |
| Blocklist | 71,934 domains |

### Key Configuration Files

**Static IP (NetworkManager):**
```bash
sudo nmcli connection modify "netplan-eth0" \
  ipv4.method manual \
  ipv4.addresses 192.168.20.10/24 \
  ipv4.gateway 192.168.20.1 \
  ipv4.dns "1.1.1.1"
```

**Pi-hole Listening Mode:**
```bash
# Required for multi-VLAN environments
sudo pihole-FTL --config dns.listeningMode ALL
```

### Log2RAM Installation
Protects SD card from excessive writes:
```bash
curl -Lo log2ram.tar.gz https://github.com/azlux/log2ram/archive/master.tar.gz
tar xf log2ram.tar.gz && cd log2ram-master
sudo ./install.sh
```

### pfSense Integration

**System DNS:** System → General Setup
- DNS Server 1: 192.168.20.10 (Pi-hole)
- DNS Server 2: 1.1.1.1 (Cloudflare backup)
- DNS Server Override: Disabled

**DHCP DNS:** Services → DHCP Server → [Each VLAN]
- DNS Server: 192.168.20.10

**Firewall Rules:** Allow DNS (port 53) from each VLAN to 192.168.20.10

### Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| "ignoring query from non-local" | listeningMode = LOCAL | Change to ALL |
| Static IP not applying | Using dhcpcd instead of NetworkManager | Use nmcli commands |
| DNS timeout from other VLANs | Firewall blocking | Add DNS rules in pfSense |

---

## Phase 4C: Nginx Proxy Manager

### Deployment Platform
- **Type:** LXC Container (CT 201)
- **Host:** Proxmox VE
- **VLAN:** 30 (DMZ)
- **OS:** Debian 12 (Bookworm)

### Container Specifications

| Resource | Value |
|----------|-------|
| CPU | 1 core |
| Memory | 512 MB |
| Swap | 512 MB |
| Disk | 8 GB (vmpool-fast) |
| Network | vmbr0, VLAN 30 |

### Network Configuration

| Setting | Value |
|---------|-------|
| IP Address | 192.168.30.10 |
| Gateway | 192.168.30.1 |
| DNS | 192.168.20.10 (Pi-hole) |

### Docker Installation

```bash
# Install prerequisites
apt update && apt upgrade -y
apt install -y curl sudo ca-certificates gnupg

# Add Docker repository
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
apt update
apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### NPM Deployment

```bash
mkdir -p /opt/npm && cd /opt/npm

cat > docker-compose.yml << 'EOF'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
EOF

docker compose up -d
```

**Default Credentials:**
- Email: admin@example.com
- Password: changeme

### Proxy Hosts Configured

| Domain | Destination | SSL |
|--------|-------------|-----|
| proxmox.najhin-gaming.com | https://192.168.1.5:8006 | ✅ Let's Encrypt |
| pihole.najhin-gaming.com | http://192.168.20.10:80 | ✅ Let's Encrypt |
| npm.najhin-gaming.com | http://192.168.30.10:81 | ✅ Let's Encrypt |

### Cloudflare DNS Configuration

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| A | proxmox | [PUBLIC_IP] | ☁️ Proxied |
| A | pihole | [PUBLIC_IP] | ☁️ Proxied |
| A | npm | [PUBLIC_IP] | ☁️ Proxied |

### SSL Certificate Process

1. Add DNS record with **grey cloud** (DNS only)
2. Request certificate in NPM (Let's Encrypt)
3. After success, enable **orange cloud** (Proxied)

### Port Forwarding (Double NAT)

**ISP Router:**
| External Port | Internal IP | Internal Port |
|---------------|-------------|---------------|
| 80 | 192.168.100.169 | 80 |
| 443 | 192.168.100.169 | 443 |

**pfSense:** Firewall → NAT → Port Forward
| Interface | Dest Port | NAT IP | NAT Port |
|-----------|-----------|--------|----------|
| WAN | 80 | 192.168.30.10 | 80 |
| WAN | 443 | 192.168.30.10 | 443 |

### Critical Fix: Proxmox VLAN Configuration

LXC containers on VLANs require the bridge to pass VLAN tags:

**Temporary fix:**
```bash
bridge vlan add vid 30 dev nic0 tagged
bridge vlan add vid 30 dev vmbr0 self tagged
```

**Permanent fix:** Edit `/etc/network/interfaces`:
```
auto vmbr0
iface vmbr0 inet static
        address 192.168.1.5/24
        gateway 192.168.1.1
        bridge-ports nic0
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094
```

---

## Phase 4D: Tailscale VPN

### Purpose
Secure remote access to all homelab VLANs without port forwarding.

### Installation on pfSense

1. **Install Package:** System → Package Manager → Available Packages → Tailscale
2. **Configure:** VPN → Tailscale

### Tailscale Settings

| Setting | Value |
|---------|-------|
| Enable | ✅ |
| Listen Port | 41641 |
| Accept DNS | ✅ |

### Advertised Routes

| Subnet | Description |
|--------|-------------|
| 192.168.1.0/24 | Management VLAN |
| 192.168.10.0/24 | Main Network |
| 192.168.20.0/24 | Homelab Services |
| 192.168.30.0/24 | DMZ |

### Authentication

1. Generate auth key at https://login.tailscale.com/admin/settings/keys
2. Paste into pfSense → VPN → Tailscale → Authentication → Pre-authentication Key
3. Save and verify connection on Status tab

### Subnet Approval

After pfSense connects:
1. Go to https://login.tailscale.com/admin/machines
2. Click on pfsense-homelab
3. Approve all advertised subnets

### Tailscale Status

| Property | Value |
|----------|-------|
| Device Name | pfsense-homelab |
| Tailscale IP | 100.110.165.45 |
| Status | ✅ Connected |
| Nearest DERP | Singapore (9.9ms) |

### Client Setup

1. Install Tailscale on phone/laptop
2. Sign in with same account
3. Enable VPN
4. Access homelab via internal IPs (192.168.x.x)

---

## Troubleshooting Guide - All Issues Encountered

### Phase 4A Issues (Pi-hole)

#### Issue 1: Cannot Access Switch at Expected IP
**Symptom:** Switch not responding at 192.168.1.20
**Cause:** Switch was using DHCP instead of static IP, assigned 192.168.1.12
**Solution:** 
1. Check pfSense DHCP leases (Status → DHCP Leases) to find actual IP
2. Access switch at DHCP-assigned IP
3. Set static IP and disable DHCP on switch
4. Restore configuration from backup if needed

#### Issue 2: Static IP Not Applying on Pi-hole
**Symptom:** After editing `/etc/dhcpcd.conf` and rebooting, Pi-hole still had DHCP IP
**Cause:** Raspberry Pi OS (Trixie/Bookworm) uses NetworkManager, not dhcpcd
**Diagnosis:**
```bash
sudo systemctl status dhcpcd    # Returns "Unit not found"
sudo systemctl status NetworkManager    # Returns "active (running)"
```
**Solution:** Use nmcli instead of dhcpcd.conf:
```bash
sudo nmcli connection modify "netplan-eth0" \
  ipv4.method manual \
  ipv4.addresses 192.168.20.10/24 \
  ipv4.gateway 192.168.20.1 \
  ipv4.dns "1.1.1.1"
sudo nmcli connection down "netplan-eth0" && sudo nmcli connection up "netplan-eth0"
```

#### Issue 3: Log2RAM GPG Key Download Failed
**Symptom:** `ERROR 404: Not Found` when downloading GPG key
**Cause:** Repository GPG key URL changed
**Solution:** Use tarball installation instead:
```bash
curl -Lo log2ram.tar.gz https://github.com/azlux/log2ram/archive/master.tar.gz
tar xf log2ram.tar.gz && cd log2ram-master
sudo ./install.sh
```

#### Issue 4: apt update Error After Log2RAM Install
**Symptom:** "Repository is not signed" error for azlux repository
**Cause:** GPG key mismatch from failed earlier attempt
**Solution:** Remove the problematic repository (Log2RAM already installed):
```bash
sudo rm /etc/apt/sources.list.d/azlux.list
sudo apt update
```

#### Issue 5: DNS Queries from Other VLANs Rejected
**Symptom:** 
- `nslookup google.com 192.168.20.10` times out
- tcpdump shows packets arriving but no responses
- Pi-hole logs show: `ignoring query from non-local network 192.168.1.10`

**Cause:** Pi-hole's `listeningMode = "LOCAL"` only accepts queries from same subnet
**Diagnosis:**
```bash
cat /etc/pihole/pihole.toml | grep listeningMode
sudo pihole -t  # Shows "ignoring query from non-local network"
```
**Solution:**
```bash
sudo pihole-FTL --config dns.listeningMode ALL
sudo systemctl restart pihole-FTL
```

#### Issue 6: PC Has No Internet Despite DNS Working
**Symptom:** DNS resolves but can't browse internet via ethernet
**Cause:** PC was preferring IPv6 DNS (fe80::1) over configured IPv4 DNS
**Solution:** Fixing Pi-hole listeningMode resolved this as Pi-hole could now respond

---

### Phase 4C Issues (Nginx Proxy Manager)

#### Issue 7: LXC Container Cannot Reach Gateway
**Symptom:** Container at 192.168.30.10 cannot ping gateway 192.168.30.1
**Diagnosis:**
```bash
# On Proxmox host
bridge vlan show
# Shows nic0 and vmbr0 only have VLAN 1, not VLAN 30
```
**Cause:** Proxmox bridge not passing VLAN 30 traffic despite `bridge-vlan-aware yes`
**Solution - Temporary:**
```bash
bridge vlan add vid 30 dev nic0 tagged
bridge vlan add vid 30 dev vmbr0 self tagged
```
**Solution - Permanent:** Edit `/etc/network/interfaces`:
```
auto vmbr0
iface vmbr0 inet static
        address 192.168.1.5/24
        gateway 192.168.1.1
        bridge-ports nic0
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094    # ADD THIS LINE
```

#### Issue 8: Container DNS Resolution Fails
**Symptom:** `apt update` shows "Temporary failure resolving"
**Cause:** Firewall rule for DNS to Pi-hole wasn't applied
**Solution:** 
1. Add firewall rule in pfSense (Firewall → Rules → VLAN30_DMZ)
2. Click "Apply Changes" button (easy to miss!)

#### Issue 9: SSL Certificate Request Fails (Error 525)
**Symptom:** NPM shows "Internal Error" when requesting Let's Encrypt certificate
**Cause:** Cloudflare proxy intercepting HTTP challenge
**Solution:**
1. Temporarily disable Cloudflare proxy (set DNS record to grey cloud)
2. Request certificate in NPM
3. Re-enable Cloudflare proxy (orange cloud)

#### Issue 10: Double NAT Port Forwarding
**Symptom:** External access not working even with pfSense port forward
**Cause:** pfSense is behind ISP router (192.168.100.x WAN)
**Solution:** Configure port forwarding on BOTH routers:

**ISP Router:**
- Port 80 → 192.168.100.169:80 (pfSense WAN)
- Port 443 → 192.168.100.169:443 (pfSense WAN)

**pfSense:**
- Port 80 → 192.168.30.10:80 (NPM)
- Port 443 → 192.168.30.10:443 (NPM)

---

### Phase 4D Issues (Tailscale)

#### Issue 11: Tailscale Shows "Not Online"
**Symptom:** VPN → Tailscale shows "Tailscale is not online"
**Cause:** No pre-authentication key configured
**Solution:**
1. Generate auth key at https://login.tailscale.com/admin/settings/keys
2. Paste into pfSense → VPN → Tailscale → Authentication → Pre-authentication Key
3. Save and check Status tab

#### Issue 12: Subnets Not Accessible via Tailscale
**Symptom:** Can connect to Tailscale but can't access 192.168.x.x addresses
**Cause:** Advertised subnets require approval in Tailscale admin
**Solution:**
1. Go to https://login.tailscale.com/admin/machines
2. Click on pfsense-homelab
3. Approve each advertised subnet

---

## Security Considerations

### Pi-hole
- `listeningMode = ALL` is safe when pfSense controls access
- Only internal networks can reach Pi-hole (firewall rules)

### Nginx Proxy Manager
- Located in DMZ (VLAN 30) - isolated from main network
- SSL certificates ensure encrypted connections
- Cloudflare proxy hides origin IP

### Tailscale
- Zero-trust mesh VPN - no open ports required
- Devices must authenticate with your Tailscale account
- Subnet routes controlled via admin console

---

## Verification Commands

### Pi-hole
```bash
pihole status
pihole -t  # Real-time query log
dig google.com @192.168.20.10
```

### NPM
```bash
docker ps
docker logs npm-app-1
curl -I https://proxmox.najhin-gaming.com
```

### Tailscale
```bash
# On pfSense shell
/usr/local/bin/tailscale status
/usr/local/bin/tailscale netcheck
```

---

## Next Steps

- [ ] Phase 4B: NAS Deployment (waiting for drives)
- [ ] Phase 5: Monitoring Stack (Grafana, Prometheus)
- [ ] Phase 6: Media/Storage Services (Nextcloud, Immich)
- [ ] Phase 7: Game Servers (Minecraft)

---

## References

- [Pi-hole Documentation](https://docs.pi-hole.net/)
- [Nginx Proxy Manager](https://nginxproxymanager.com/)
- [Tailscale Documentation](https://tailscale.com/kb/)
- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/)
- [Cloudflare DNS](https://developers.cloudflare.com/dns/)
