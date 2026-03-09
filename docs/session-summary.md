# Session Summary: March 9, 2026 — Firewall Hardening

> **Session Duration:** ~2 hours  
> **Phase:** 6F (Firewall Hardening - continuation)  
> **Status:** ✅ Complete

---

## What We Did

### 1. Firewall Rules Audit
- Captured screenshots of all pfSense firewall rules (10 interfaces)
- Identified 5 critical issues: all VLANs had "allow-all (temporary)" rules
- Found LAN DNS rule pointing to wrong IP (192.168.20.10 instead of 192.168.30.10)

### 2. Created Firewall Aliases
**IP Aliases:**
- RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16)
- DNS_SERVERS (192.168.30.10)
- PROXMOX (192.168.10.5)
- Monitoring_Servers (192.168.30.202-206)

**Port Aliases:**
- SERVICE_PORTS (80, 443, 3000, 3001, 8443)
- GAME_PORTS (7777, 25565, 25570)
- MONITORING_PORTS (8006, 9100)

### 3. Implemented VLAN Rules
- **VLAN10_MGMT:** 4 rules (DNS, VLAN30 access, RFC1918 block, internet)
- **VLAN20_MAIN:** 6 rules (gateway, DNS, services, games, RFC1918 block, internet)
- **VLAN30_SERVICES:** 5 rules (DNS, Proxmox metrics, intra-VLAN, RFC1918 block, internet)
- **VLAN40_DMZ:** 3 rules (DNS, RFC1918 block, internet)
- **VLAN50_MALWARE:** 0 rules (air-gapped by design)

### 4. Installed Tailscale on Gaming PC
- Installed Tailscale Windows client
- Gaming PC Tailscale IP: 100.106.109.4
- Verified connectivity to pfSense subnet router
- Confirmed Proxmox access works via Tailscale tunnel

### 5. Fixed Issues During Implementation
- **Internet stopped working after VLAN20 rules:** Source was set to "address" instead of "subnets"
- **Fixed all VLANs:** Changed Source from "VLANxx address" to "VLANxx subnets"
- **Fixed LAN DNS rule:** Changed destination from 192.168.20.10 to DNS_SERVERS alias

### 6. Verification Testing
- ✅ Internet access works
- ✅ DNS resolution works
- ✅ VLAN30 services accessible
- ✅ VLAN10 blocked when Tailscale disconnected
- ✅ VLAN10 accessible when Tailscale connected

---

## What We Changed

### pfSense Configuration

| Component | Before | After |
|-----------|--------|-------|
| Aliases (IP) | 2 aliases | 4 aliases |
| Aliases (Ports) | 3 aliases | 3 aliases (fixed MONITORING_PORTS) |
| VLAN10_MGMT rules | 1 allow-all | 4 specific rules |
| VLAN20_MAIN rules | 1 allow-all | 6 specific rules |
| VLAN30_SERVICES rules | 1 allow-all | 5 specific rules |
| VLAN40_DMZ rules | 1 allow-all | 3 specific rules |
| VLAN50_MALWARE rules | 0 rules | 0 rules (correct) |
| LAN DNS rule | Wrong IP | Fixed to DNS_SERVERS |

### Gaming PC
- Installed Tailscale client (100.106.109.4)
- Verified access to all VLANs via Tailscale tunnel

---

## Errors Encountered & Resolutions

### 1. Internet Loss After VLAN20 Rules
**Symptom:** Gaming PC lost internet after applying VLAN20_MAIN rules  
**Cause:** Rules used "VLAN20_MAIN address" as Source (matches only gateway IP)  
**Fix:** Changed all rules to use "VLAN20_MAIN subnets" as Source

### 2. Tailscale ICMP Ping Failing
**Symptom:** `ping 100.110.165.45` timed out  
**Cause:** pfSense not responding to ICMP over Tailscale (normal behavior)  
**Fix:** Not needed — `tailscale ping` works, actual traffic works

### 3. Monitoring_Servers Alias Had Old IPs
**Symptom:** Alias contained VLAN 20 IPs from pre-migration  
**Fix:** Updated to current VLAN 30 IPs (192.168.30.202-206)

### 4. MONITORING_PORTS Had Wrong Ports
**Symptom:** Alias included port 53 (DNS)  
**Fix:** Changed to only 8006, 9100

---

## Key Learnings

1. **"subnets" vs "address"** — Critical distinction in pfSense. "address" = gateway IP only, "subnets" = all devices on network.

2. **Rule order matters** — pfSense processes top-to-bottom, first match wins.

3. **Tailscale bypasses local firewall** — Traffic via Tailscale uses Tailscale interface rules, not source VLAN rules.

4. **Test with Tailscale OFF** — To verify local blocking works correctly.

5. **Gateway access rule** — Devices need to reach their VLAN gateway for routing.

---

## What We Planned (Next Steps)

### Immediate Next Steps
1. **Phase 7A: Backup Strategy** — High priority
2. Move switch management IP (192.168.1.20 → 192.168.10.20)
3. Remove legacy LAN interface

### Lower Priority
- Add logging rules for security monitoring
- Homepage dashboard (Phase 6E)
- n8n automation (Phase 7B)

---

## Current Infrastructure State

### Firewall Status
- All VLANs properly segmented ✅
- RFC1918 blocking enforced ✅
- Tailscale admin access working ✅
- Malware Lab air-gapped ✅

### Pending Items
| Item | Priority | Status |
|------|----------|--------|
| Firewall rules hardening | High | ✅ COMPLETE |
| Backup strategy (Phase 7A) | High | Pending |
| Switch management IP migration | Medium | Pending |
| Legacy LAN removal | Medium | Pending |
| Nextcloud 2FA | Low | Pending |

---

*Session completed: March 9, 2026*
