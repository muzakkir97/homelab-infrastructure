# Phase 7: Nextcloud Deployment

> **Completed:** March 8, 2026  
> **Duration:** ~3 hours  
> **Objective:** Deploy self-hosted cloud storage to replace Google Drive

---

## Overview

Phase 7 deploys Nextcloud as a self-hosted cloud storage solution, providing file sync, calendar, contacts, and notes functionality accessible from anywhere via Cloudflare Tunnel.

### Deliverables

- ✅ Nextcloud Hub 26 Winter (33.0.0) deployed on LXC container
- ✅ LAMP stack (Apache, MariaDB, PHP 8.2)
- ✅ External access via Cloudflare Tunnel (no port forwarding)
- ✅ Mobile app connected (Android)
- ✅ Desktop client connected (Windows)
- ✅ Uptime Kuma monitor configured

---

## Container Specifications

| Property | Value |
|----------|-------|
| **CTID** | 220 |
| **Hostname** | nextcloud |
| **IP Address** | 192.168.30.220/24 |
| **Gateway** | 192.168.30.1 |
| **VLAN** | 30 (Services) |
| **CPU** | 2 cores |
| **RAM** | 4096 MB |
| **Disk** | 20 GB (local-lvm) |
| **Template** | debian-12-standard_12.12-1_amd64.tar.zst |

---

## Software Stack

| Component | Version |
|-----------|---------|
| OS | Debian 12 (Bookworm) |
| Web Server | Apache 2.4.66 |
| PHP | 8.2.30 |
| Database | MariaDB 10.11 |
| Nextcloud | Hub 26 Winter (33.0.0) |
| Cloudflared | 2026.2.0 |

---

## Installation Steps

### 1. Create Container

```bash
# From Proxmox shell
pct create 220 local:vztmpl/debian-12-standard_12.12-1_amd64.tar.zst \
  --hostname nextcloud \
  --cores 2 \
  --memory 4096 \
  --swap 512 \
  --rootfs local-lvm:20 \
  --net0 name=eth0,bridge=vmbr0,tag=30,ip=192.168.30.220/24,gw=192.168.30.1 \
  --nameserver 192.168.30.1 \
  --features nesting=1 \
  --unprivileged 1 \
  --start 1
```

### 2. Install LAMP Stack

```bash
apt update && apt upgrade -y

# Install Apache
apt install apache2 -y

# Install MariaDB
apt install mariadb-server -y
mysql_secure_installation

# Install PHP and extensions
apt install php php-apcu php-bcmath php-cli php-common php-curl \
  php-gd php-gmp php-imagick php-intl php-mbstring php-mysql \
  php-zip php-xml libapache2-mod-php php-redis php-smbclient -y
```

### 3. Configure Database

```bash
mysql -u root -p

CREATE DATABASE nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY '[DATABASE_PASSWORD]';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 4. Download & Install Nextcloud

```bash
cd /var/www
wget https://download.nextcloud.com/server/releases/latest.tar.bz2
tar -xjf latest.tar.bz2
chown -R www-data:www-data /var/www/nextcloud
rm latest.tar.bz2
```

### 5. Configure Apache

Create `/etc/apache2/sites-available/nextcloud.conf`:

```apache
<VirtualHost *:80>
    ServerName cloud.najhin-gaming.com
    DocumentRoot /var/www/nextcloud

    <Directory /var/www/nextcloud/>
        Require all granted
        AllowOverride All
        Options FollowSymLinks MultiViews

        <IfModule mod_dav.c>
            Dav off
        </IfModule>
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/nextcloud_error.log
    CustomLog ${APACHE_LOG_DIR}/nextcloud_access.log combined
</VirtualHost>
```

Enable configuration:

```bash
a2ensite nextcloud.conf
a2enmod rewrite headers env dir mime
a2dissite 000-default.conf
systemctl restart apache2
```

### 6. Optimize PHP

Edit `/etc/php/8.2/apache2/php.ini`:

```ini
memory_limit = 512M
upload_max_filesize = 512M
post_max_size = 512M
max_execution_time = 360
date.timezone = Asia/Kuala_Lumpur

opcache.enable = 1
opcache.interned_strings_buffer = 16
opcache.max_accelerated_files = 10000
opcache.memory_consumption = 128
opcache.save_comments = 1
opcache.revalidate_freq = 1
```

### 7. Web-Based Setup

Navigate to `http://192.168.30.220` and complete setup:

- **Admin username:** [your_admin]
- **Admin password:** [your_password]
- **Database user:** nextcloud
- **Database password:** [DATABASE_PASSWORD]
- **Database name:** nextcloud
- **Database host:** localhost

### 8. Configure Nextcloud

Edit `/var/www/nextcloud/config/config.php`:

```php
'trusted_domains' =>
array (
  0 => '192.168.30.220',
  1 => 'cloud.najhin-gaming.com',
),
'overwrite.cli.url' => 'https://cloud.najhin-gaming.com',
'overwriteprotocol' => 'https',
'memcache.local' => '\\OC\\Memcache\\APCu',
'default_phone_region' => 'MY',
'maintenance_window_start' => 1,
```

### 9. Database Optimization

```bash
cd /var/www/nextcloud
sudo -u www-data php occ db:add-missing-indices
sudo -u www-data php occ maintenance:repair --include-expensive
```

### 10. Install Cloudflare Tunnel

```bash
# Download cloudflared
curl -L https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb -o cloudflared.deb
dpkg -i cloudflared.deb

# Create tunnel in Cloudflare Zero Trust dashboard
# Get connector token from dashboard

# Install as service
cloudflared service install [TUNNEL_TOKEN]
systemctl enable cloudflared
systemctl start cloudflared
```

### 11. Configure Tunnel Route

In Cloudflare Zero Trust Dashboard:
- **Public hostname:** cloud.najhin-gaming.com
- **Service:** HTTP://localhost:80
- **HTTP Settings:** Enable "No TLS Verify" (not needed for HTTP backend)

---

## External Access

| Method | URL | Authentication |
|--------|-----|----------------|
| Browser | https://cloud.najhin-gaming.com | Nextcloud login |
| Mobile App | https://cloud.najhin-gaming.com | Nextcloud login |
| Desktop Client | https://cloud.najhin-gaming.com | Nextcloud login |
| WebDAV | https://cloud.najhin-gaming.com/remote.php/dav | Nextcloud credentials |

---

## Installed Apps

| App | Purpose |
|-----|---------|
| Calendar | Event scheduling, CalDAV sync |
| Contacts | Address book, CardDAV sync |
| Mail | Email client integration |
| Notes | Markdown note-taking |

---

## Security Configuration

### Built-in Protection

Nextcloud includes brute-force protection by default:
- Failed login tracking
- Automatic IP throttling
- Progressive delays on repeated failures

### Why NOT Cloudflare Access

Cloudflare Access was initially tested but removed because:
- Mobile apps cannot complete browser-based OAuth flow
- Desktop sync client fails authentication
- Only web browser access works with CF Access

### Alternative Hardening (Future)

- Enable TOTP 2FA in Nextcloud settings
- Configure fail2ban for additional protection
- Set up rate limiting in Apache

---

## Monitoring

### Uptime Kuma

| Monitor | URL | Type |
|---------|-----|------|
| NextCloud | http://192.168.30.220/status.php | HTTP(s) Keyword |

**Note:** Monitor `/status.php` endpoint, not root URL. Root URL redirects to login which causes false alerts.

### Prometheus

Node exporter scraping configured via existing Prometheus job.

---

## Troubleshooting

### Mobile App "Server Not Found"

**Cause:** Cloudflare Access blocking API requests
**Solution:** Remove Cloudflare Access policy; use Nextcloud native auth

### ECONNREFUSED on Port 443

**Cause:** Nextcloud runs HTTP internally; HTTPS handled by tunnel
**Solution:** Monitor http://192.168.30.220/status.php

### PHP Memory Errors

**Cause:** Default 128MB limit too low
**Solution:** Set `memory_limit = 512M` in php.ini

### OPcache Warnings

**Cause:** Default `interned_strings_buffer` too small
**Solution:** Set `opcache.interned_strings_buffer = 16` in php.ini

---

## Container Autostart

```bash
pct set 220 -onboot 1 -startup order=8
```

Boot order 8 ensures Nextcloud starts after core services but before gaming platform.

---

## Future Enhancements

| Task | Priority |
|------|----------|
| Move data directory to NAS | Medium |
| Enable TOTP 2FA | Low |
| Install Nextcloud Office | Low |
| Configure automated backups | High (Phase 7A) |

---

## Architecture Decision

### ADR-007: Cloudflare Tunnel for Nextcloud

**Decision:** Use Cloudflare Tunnel instead of port forwarding for external Nextcloud access.

**Rationale:**
- No ISP router configuration needed
- No pfSense NAT rules required
- Automatic DDoS protection
- Works with dynamic IP (DDNS not needed for this service)
- Mobile app compatible (unlike Cloudflare Access)

**Trade-off:** Dependency on Cloudflare infrastructure. Tunnel outage = no external access.

---

## Commands Reference

```bash
# Check Nextcloud status
sudo -u www-data php /var/www/nextcloud/occ status

# Scan for new files
sudo -u www-data php /var/www/nextcloud/occ files:scan --all

# Enable maintenance mode
sudo -u www-data php /var/www/nextcloud/occ maintenance:mode --on

# Check cloudflared status
systemctl status cloudflared

# View tunnel logs
journalctl -u cloudflared -f
```

---

*Documentation created: March 8, 2026*
