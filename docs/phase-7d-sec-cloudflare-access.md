# Phase 7D-Sec: Cloudflare Access for n8n

> **Status:** ✅ Complete  
> **Completed:** April 7, 2026  
> **Duration:** ~30 minutes

---

## 📋 Overview

Added Cloudflare Access protection to n8n, matching the existing security model used for Grafana. This ensures that the n8n workflow editor is protected by email OTP authentication.

---

## 🎯 Objectives

- [x] Configure Cloudflare Access application for n8n
- [x] Set up email OTP authentication
- [x] Test access flow
- [x] Verify webhook still works (bypass for API calls)

---

## 🔒 Security Model

### Before

| Service | Protection |
|---------|------------|
| Grafana | ✅ Cloudflare Access (email OTP) |
| n8n | ❌ No protection (public) |
| Homepage | ❌ No protection |
| Nextcloud | ✅ Cloudflare Tunnel |

### After

| Service | Protection |
|---------|------------|
| Grafana | ✅ Cloudflare Access (email OTP) |
| n8n | ✅ Cloudflare Access (email OTP) |
| Homepage | ❌ No protection |
| Nextcloud | ✅ Cloudflare Tunnel |

---

## 🏗️ Implementation

### Step 1: Create Access Application

1. Log in to Cloudflare Dashboard
2. Navigate to **Zero Trust** → **Access** → **Applications**
3. Click **Add an application**
4. Select **Self-hosted**

### Step 2: Configure Application

| Setting | Value |
|---------|-------|
| **Application name** | n8n |
| **Session duration** | 24 hours |
| **Application domain** | n8n.najhin-gaming.com |
| **Path** | (leave empty for entire domain) |

### Step 3: Configure Policy

| Setting | Value |
|---------|-------|
| **Policy name** | Email OTP |
| **Action** | Allow |
| **Include** | Emails ending in @gmail.com (or your domain) |
| **Authentication method** | One-time PIN |

### Step 4: Webhook Bypass (Important!)

n8n webhooks need to work without authentication for Telegram integration.

**Option A: Service Token (Recommended)**
- Create a Service Token in Cloudflare Access
- Configure Telegram webhook to include token in header

**Option B: Bypass Path**
- Add bypass rule for `/webhook/*` path
- Less secure but simpler

**Implemented:** The Telegram webhook continues to work because Cloudflare Access only protects browser requests to the editor UI, not API/webhook calls by default.

---

## ✅ Verification

| Test | Result |
|------|--------|
| Access n8n.najhin-gaming.com in browser | ✅ Redirects to Cloudflare login |
| Enter email, receive OTP | ✅ OTP received |
| Enter OTP, access n8n | ✅ Access granted |
| Telegram webhook works | ✅ Messages processed |
| Session persists for 24h | ✅ No re-auth needed |

---

## 🔄 Access Flow

```
┌─────────────────┐
│  User Browser   │
│  (You)          │
└────────┬────────┘
         │ https://n8n.najhin-gaming.com
         ▼
┌─────────────────┐
│  Cloudflare     │
│  Access         │
│                 │
│  ┌───────────┐  │
│  │ Logged in?│  │
│  └─────┬─────┘  │
│        │        │
│   No ──┴── Yes  │
│   │         │   │
│   ▼         │   │
│ ┌─────────┐ │   │
│ │Email OTP│ │   │
│ │ Login   │ │   │
│ └────┬────┘ │   │
│      │      │   │
│      └──────┘   │
│         │       │
└─────────┼───────┘
          │ Authenticated
          ▼
┌─────────────────┐
│  n8n Editor     │
│  (CT 211)       │
└─────────────────┘
```

---

## 📝 Notes

### Why Email OTP?

- No password to manage
- Works on any device
- Simple for single-user setup
- Same model as existing Grafana protection

### Session Duration

- Set to 24 hours
- Balances security vs. convenience
- Can adjust in Cloudflare Access settings

### Webhook Considerations

- Telegram webhooks still work (different auth path)
- API calls with proper headers bypass Access
- Only browser requests require OTP

---

## 🔗 Related

- **Prerequisite:** [Phase 7D: Gilgamesh Enhancements](phase-7d-gilgamesh-enhancements.md)
- **Similar:** Grafana Cloudflare Access (Phase 5)

---

*Completed: April 7, 2026*
