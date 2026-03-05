# Target Architecture

> **Last Updated:** March 5, 2026

---

## Design Principles

1. **Defense in Depth** — Multiple security layers
2. **Segmentation** — Isolate different traffic types
3. **One Service Per Container** — Clearer boundaries, independent management
4. **Documentation First** — Everything documented before and after
5. **Learn by Doing** — Prefer complex approaches for learning value

---

## VLAN Design

| VLAN | Purpose | Security Level |
|------|---------|----------------|
| 10 - Management | Infrastructure devices | Highest — admin only |
| 20 - Main Network | Daily-use clients | Medium — trusted users |
| 30 - Services | All hosted services | Medium — controlled access |
| 40 - DMZ | Public-facing services | Low trust — internet exposed |
| 50 - Malware Lab | Security research | Air-gapped — no connectivity |

## Traffic Flow Rules (Target)

| From → To | Allowed | Method |
|-----------|---------|--------|
| VLAN 20 → Internet | ✅ Yes | NAT via pfSense |
| VLAN 20 → VLAN 30 | ✅ Yes | Specific services only |
| VLAN 20 → VLAN 10 | ❌ No | Admin access via Tailscale |
| VLAN 30 → Internet | ✅ Yes | Updates, DDNS |
| VLAN 30 → VLAN 10 | ✅ Limited | Prometheus → Proxmox |
| VLAN 50 → Anywhere | ❌ No | Completely isolated |

---

## Architecture Decision Records

### ADR-001: LXC Containers over VMs
- **Context:** Single server with 32GB RAM, need 15+ services
- **Decision:** Use LXC (256-512MB each) instead of VMs (2-4GB each)
- **Trade-off:** Less isolation. Malware Lab uses full VMs for stronger isolation.

### ADR-002: One Container Per Service
- **Context:** Could stack multiple services per container
- **Decision:** Each service gets its own LXC container
- **Rationale:** Mirrors enterprise microservices; independent backup/update/firewall

### ADR-003: Router-on-a-Stick
- **Context:** Could use multiple physical NICs per VLAN
- **Decision:** Single-NIC pfSense with 802.1Q subinterfaces
- **Trade-off:** All inter-VLAN traffic on single link

### ADR-004: Gaming Services on VLAN 30
- **Context:** Originally had separate Gaming VLAN
- **Decision:** Merge into Services VLAN to reduce complexity

### ADR-005: Cloudflare for External Access
- **Decision:** DDoS protection, SSL termination, Zero Trust auth (free tier)

### ADR-006: Tailscale for Admin Access
- **Decision:** No exposed SSH ports; WireGuard-based VPN through NAT

---

## Access Control Matrix

| Resource | Internal (V20) | Tailscale | CF Access | Public |
|----------|---------------|-----------|-----------|--------|
| Proxmox | ❌ | ✅ | ❌ | ❌ |
| pfSense | ❌ | ✅ | ❌ | ❌ |
| Pi-hole | ❌ | ✅ | ❌ | ❌ |
| Grafana | ✅ | ✅ | ✅ (OTP) | ❌ |
| Pterodactyl | ✅ | ✅ | ❌ | ✅ |
| Game Servers | ✅ | ✅ | ❌ | ✅ |

---

*Last updated: March 5, 2026*
