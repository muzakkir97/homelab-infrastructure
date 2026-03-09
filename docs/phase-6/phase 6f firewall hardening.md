# Phase 6F: Firewall Hardening & Inter-VLAN Segmentation

> **Date:** March 9, 2026  
> **Status:** ✅ Complete  
> **Author:** Muzakkir Kholil  
> **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)

---

## Overview

This phase implements proper firewall rules to enforce network segmentation between VLANs. The previous configuration used "allow-all" rules which defeated the purpose of VLAN separation. After hardening, each VLAN has explicit rules controlling what traffic is permitted.

---

## Problem Statement

### Before (Vulnerable)
- All VLANs had "Allow all (temporary)" rules
- Any device on any VLAN could reach any other VLAN
- Compromised client device could access management infrastructure
- VLAN segmentation existed at Layer 2 but not enforced at Layer 3

### After (Hardened)
- Explicit allow rules for required traffic only
- RFC1918 block rules prevent inter-VLAN lateral movement
- Management network (VLAN 10) isolated from client devices
- Malware Lab (VLAN 50) completely air-gapped
- Admin access via Tailscale VPN only

---

## Network Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         INTERNET                                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                       pfSense Firewall                          │
│                    (192.168.x.1 per VLAN)                       │
└─────────────────────────────────────────────────────────────────┘
         │           │           │           │           │
         ▼           ▼           ▼           ▼           ▼
    ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐
    │VLAN 10 │  │VLAN 20 │  │VLAN 30 │  │VLAN 40 │  │VLAN 50 │
    │  MGMT  │  │  MAIN  │  │SERVICES│  │  DMZ   │  │MALWARE │
    └────────┘  └────────┘  └────────┘  └────────┘  └────────┘
    │Proxmox │  │Gaming PC│  │Pi-hole │  │(Future)│  │(Airgap)│
    │  NAS   │  │ Laptop  │  │All CTs │  │        │  │        │
    └────────┘  └────────┘  └────────┘  └────────┘  └────────┘
```

---

## Implementation

### Step 1: Create Firewall Aliases

Aliases simplify rule management by grouping IPs and ports.

#### IP Aliases

| Alias | Type | Values | Description |
|-------|------|--------|-------------|
| RFC1918 | Network | 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 | All private IP ranges |
| DNS_SERVERS | Host | 192.168.x.10 | Pi-hole DNS |
| PROXMOX | Host | 192.168.x.5 | Proxmox server |
| Monitoring_Servers | Host | 192.168.x.202-206 | Prometheus, Grafana, Loki, Alertmanager, Uptime Kuma |

#### Port Aliases

| Alias | Type | Values | Description |
|-------|------|--------|-------------|
| SERVICE_PORTS | Port | 80, 443, 3000, 3001, 8443 | Web service ports |
| GAME_PORTS | Port | 7777, 25565, 25570 | Game server ports |
| MONITORING_PORTS | Port | 8006, 9100 | Proxmox WebUI, Node Exporter |

**pfSense Path:** Firewall → Aliases → IP / Ports

---

### Step 2: Configure VLAN Rules

#### Rule Design Pattern

Each VLAN follows this pattern:
1. **Allow DNS** — Permit DNS queries to Pi-hole
2. **Allow specific services** — Only what's needed
3. **Block RFC1918** — Prevent access to all private networks
4. **Allow internet** — Permit outbound internet access

**Critical:** The RFC1918 block rule must come AFTER specific allow rules but BEFORE the internet allow rule.

---

#### VLAN10_MGMT (Management Network)

**Purpose:** Infrastructure devices (Proxmox, NAS)

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Pass | TCP/UDP | VLAN10_MGMT subnets | DNS_SERVERS | 53 | Allow DNS to Pi-hole |
| 2 | Pass | TCP | VLAN10_MGMT subnets | VLAN30_SERVICES subnets | SERVICE_PORTS | Allow access to VLAN30 services |
| 3 | Block | * | VLAN10_MGMT subnets | RFC1918 | * | Block other private networks |
| 4 | Pass | * | VLAN10_MGMT subnets | * | * | Allow internet access |

---

#### VLAN20_MAIN (Client Devices)

**Purpose:** Daily-use devices (Gaming PC, Work Laptop)

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Pass | * | VLAN20_MAIN subnets | VLAN20_MAIN address | * | Allow access to gateway |
| 2 | Pass | TCP/UDP | VLAN20_MAIN subnets | DNS_SERVERS | 53 | Allow DNS to Pi-hole |
| 3 | Pass | TCP | VLAN20_MAIN subnets | VLAN30_SERVICES subnets | SERVICE_PORTS | Allow access to web services |
| 4 | Pass | TCP/UDP | VLAN20_MAIN subnets | VLAN30_SERVICES subnets | GAME_PORTS | Allow access to game servers |
| 5 | Block | * | VLAN20_MAIN subnets | RFC1918 | * | Block all private networks |
| 6 | Pass | * | VLAN20_MAIN subnets | * | * | Allow internet access |

**Security Note:** VLAN20 cannot reach VLAN10 (Proxmox, NAS) directly. Admin access is via Tailscale VPN only.

---

#### VLAN30_SERVICES (Container Network)

**Purpose:** All homelab service containers

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Pass | TCP/UDP | VLAN30_SERVICES subnets | DNS_SERVERS | 53 | Allow DNS to Pi-hole |
| 2 | Pass | TCP | VLAN30_SERVICES subnets | PROXMOX | MONITORING_PORTS | Prometheus to Proxmox metrics |
| 3 | Pass | * | VLAN30_SERVICES subnets | VLAN30_SERVICES subnets | * | Allow intra-VLAN traffic |
| 4 | Block | * | VLAN30_SERVICES subnets | RFC1918 | * | Block other private networks |
| 5 | Pass | * | VLAN30_SERVICES subnets | * | * | Allow internet access |

---

#### VLAN40_DMZ (Demilitarized Zone)

**Purpose:** Future public-facing services

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Pass | TCP/UDP | VLAN40_DMZ subnets | DNS_SERVERS | 53 | Allow DNS to Pi-hole |
| 2 | Block | * | VLAN40_DMZ subnets | RFC1918 | * | Block all private networks |
| 3 | Pass | * | VLAN40_DMZ subnets | * | * | Allow internet access |

**Security Note:** DMZ has the most restrictive rules — no access to ANY internal network.

---

#### VLAN50_MALWARE (Security Lab)

**Purpose:** Isolated malware analysis environment

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| — | (none) | — | — | — | — | Implicit deny blocks everything |

**Security Note:** No rules = complete isolation. This VLAN is air-gapped by design.

---

### Step 3: Configure Tailscale for Admin Access

Since VLAN20 (clients) can no longer reach VLAN10 (management), Tailscale provides secure admin access.

#### Tailscale Setup

1. **pfSense:** Already configured as subnet router
2. **Gaming PC:** Install Tailscale client, login with same account
3. **Access:** Use local IPs through Tailscale tunnel

#### Tailscale Interface Rules

| # | Action | Protocol | Source | Destination | Port | Description |
|---|--------|----------|--------|-------------|------|-------------|
| 1 | Pass | * | * | * | * | Allow Tailscale full network access |
| 2 | Pass | TCP | * | This Firewall | 443 | Allow Tailscale to pfSense WebUI |

#### How It Works

```
Gaming PC (VLAN 20) ──local──→ 192.168.10.5 ═══ BLOCKED

Gaming PC (Tailscale) ──tunnel──→ pfSense ──→ 192.168.10.5 ═══ ALLOWED
```

---

## Verification Tests

### Test Results

| Test | Command | Expected | Result |
|------|---------|----------|--------|
| Internet access | `ping google.com` | Reply | ✅ Pass |
| DNS resolution | `nslookup google.com` | Resolves via gateway | ✅ Pass |
| VLAN30 services | `curl http://192.168.x.203:3000` | HTTP 200 | ✅ Pass |
| VLAN10 blocked (local) | `ping 192.168.x.5` (Tailscale OFF) | Timeout | ✅ Pass |
| VLAN10 via Tailscale | `https://192.168.x.5:8006` | Proxmox login | ✅ Pass |

---

## Security Improvements

| Aspect | Before | After |
|--------|--------|-------|
| Inter-VLAN access | Any → Any | Explicitly defined |
| Management access | Any VLAN | Tailscale only |
| Lateral movement | Possible | Blocked by RFC1918 rules |
| Compromised client impact | Full network access | Limited to VLAN30 services |
| Malware Lab isolation | Theoretical | Enforced (no rules) |

---

## Troubleshooting

### Common Issues

**Issue:** No internet after applying rules
**Cause:** Source set to "VLAN address" instead of "VLAN subnets"
**Fix:** Edit each rule, change Source to "VLANxx subnets"

**Issue:** Can't reach services after hardening
**Cause:** Missing allow rule or wrong port alias
**Fix:** Check rule order; ensure allow rules come before RFC1918 block

**Issue:** Tailscale not bypassing local firewall
**Cause:** Tailscale disconnected or not installed
**Fix:** Verify Tailscale status with `tailscale status`

---

## Files Changed

| Location | Change |
|----------|--------|
| pfSense → Firewall → Aliases | Created 7 aliases (4 IP, 3 Port) |
| pfSense → Firewall → Rules → VLAN10_MGMT | 4 rules (was 1 allow-all) |
| pfSense → Firewall → Rules → VLAN20_MAIN | 6 rules (was 1 allow-all) |
| pfSense → Firewall → Rules → VLAN30_SERVICES | 5 rules (was 1 allow-all) |
| pfSense → Firewall → Rules → VLAN40_DMZ | 3 rules (was 1 allow-all) |
| pfSense → Firewall → Rules → VLAN50_MALWARE | 0 rules (unchanged, correct) |
| pfSense → Firewall → Rules → LAN | Fixed DNS rule (wrong IP) |

---

## Lessons Learned

1. **"subnets" vs "address"** — Source must be "subnets" for rules to match device traffic. "address" only matches the gateway IP itself.

2. **Rule order matters** — pfSense processes top-to-bottom, first match wins. Allow specific traffic before blocking RFC1918.

3. **Tailscale bypasses local rules** — Traffic entering via Tailscale interface uses Tailscale rules, not the source VLAN's rules.

4. **Test with Tailscale OFF** — To verify local blocking works, disconnect Tailscale first.

5. **Gateway access rule** — Devices need to reach their VLAN gateway for routing. Add "allow to VLAN address" if issues occur.

---

## Next Steps

- [ ] Phase 7A: Backup Strategy (scheduled, rotation, recovery)
- [ ] Move switch management IP from 192.168.1.20 to 192.168.10.20
- [ ] Remove legacy LAN interface
- [ ] Consider logging rules for security monitoring

---

*Documentation created: March 9, 2026*
