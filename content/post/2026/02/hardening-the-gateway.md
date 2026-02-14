+++
title = "Hardening the Gateway: Securing a Minimal Reverse Proxy"
date = 2026-02-14T12:30:00-05:00
author = "r3r4um [r3r4um-code](https://github.com/r3r4um-code)"
tags = ["security", "caddy", "fail2ban", "firewall", "hardening", "sysadmin"]
categories = ["security"]
draft = false
+++

An hour ago, I published a post about building a minimal reverse proxy. Now, let's lock it down.

A reverse proxy is the front door to your network. Every request from the internet passes through it. If it's compromised, everything behind it is exposed. So let's lock it down.

## The Threat Model

This box sits between the internet and internal services. It needs to:

1. **Accept legitimate traffic** on HTTP/HTTPS
2. **Reject everything else** by default
3. **Block attackers** who probe for vulnerabilities
4. **Minimize attack surface** (fewer services = fewer exploits)
5. **Log everything** for forensics

## Layer 1: Firewall (UFW)

First, lock down the network layer. Default deny everything, then open only what's needed:

```bash
# Set defaults
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Open only what's needed
sudo ufw allow 80/tcp comment 'HTTP'
sudo ufw allow 443/tcp comment 'HTTPS'
sudo ufw allow 8448/tcp comment 'Matrix Federation'

# SSH from LAN only
sudo ufw allow from 192.168.0.0/16 to any port 22 proto tcp comment 'SSH LAN only'

# Enable
sudo ufw enable
```

Result:
- HTTP/HTTPS: Open (required for reverse proxy)
- SSH: LAN only (no remote access from internet)
- Everything else: Blocked

## Layer 2: Caddy Security Headers

HTTP headers tell browsers how to handle your content. The right headers prevent entire classes of attacks:

```caddy
# Reusable security headers snippet
(security_headers) {
    header {
        # HSTS - Force HTTPS for 1 year, include subdomains
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        
        # Prevent MIME-type sniffing attacks
        X-Content-Type-Options "nosniff"
        
        # Block clickjacking
        X-Frame-Options "DENY"
        
        # XSS protection (legacy, but still useful)
        X-XSS-Protection "1; mode=block"
        
        # Control referrer information
        Referrer-Policy "strict-origin-when-cross-origin"
        
        # Disable dangerous browser features
        Permissions-Policy "geolocation=(), microphone=(), camera=()"
        
        # Remove server identification
        -Server
    }
}
```

Then apply to every site:

```caddy
example.com {
    import security_headers
    
    # Limit request body size (prevent DoS)
    request_body {
        max_size 10MB
    }
    
    reverse_proxy backend:8080 {
        header_up X-Real-IP {remote_host}
    }
}
```

**What each header does:**

| Header | Purpose |
|--------|---------|
| `Strict-Transport-Security` | Forces HTTPS, prevents downgrade attacks |
| `X-Content-Type-Options` | Prevents MIME confusion attacks |
| `X-Frame-Options` | Blocks clickjacking via iframes |
| `X-XSS-Protection` | Legacy XSS filter (backup for old browsers) |
| `Referrer-Policy` | Controls what info leaks in referrer headers |
| `Permissions-Policy` | Disables browser features you don't need |
| `-Server` | Removes version info from responses |

## Layer 3: JSON Access Logging

You can't investigate what you don't log. Enable structured JSON logging:

```caddy
{
    log {
        output file /var/log/caddy/access.log {
            roll_size 100mb
            roll_keep 5
        }
        format json
    }
}
```

JSON logs are:
- **Parseable** by tools like `jq`, fail2ban, SIEM systems
- **Structured** with consistent fields
- **Searchable** for incident response

## Layer 4: Fail2Ban

Attackers probe. Scanners scan. Bots try default passwords. Fail2ban watches logs and bans repeat offenders:

```ini
# /etc/fail2ban/jail.local

[DEFAULT]
bantime = 1h
findtime = 10m
maxretry = 5
banaction = ufw

[sshd]
enabled = true
port = ssh
filter = sshd
backend = systemd
maxretry = 3
bantime = 24h

[caddy-auth]
enabled = true
port = http,https
filter = caddy-auth
logpath = /var/log/caddy/access.log
maxretry = 5
findtime = 5m
bantime = 1h

[caddy-badbots]
enabled = true
port = http,https
filter = caddy-badbots
logpath = /var/log/caddy/access.log
maxretry = 2
bantime = 24h
```

**Custom filters for Caddy JSON logs:**

```ini
# /etc/fail2ban/filter.d/caddy-auth.conf
[Definition]
failregex = ^.*"remote_ip":"<HOST>".*"status":(401|403).*$
ignoreregex =
```

```ini
# /etc/fail2ban/filter.d/caddy-badbots.conf
[Definition]
failregex = ^.*"remote_ip":"<HOST>".*"uri":".*\.(php|env|git|sql|bak).*$
            ^.*"remote_ip":"<HOST>".*"user_agent":".*(sqlmap|nikto|scanner).*".*$
ignoreregex =
```

**What gets banned:**
- **SSH**: 3 failed logins → 24h ban
- **Auth failures**: 5 × 401/403 in 5 min → 1h ban
- **Bad bots**: 2 scanner hits → 24h ban

## Layer 5: Request Limits

Prevent resource exhaustion by limiting request sizes:

```caddy
# Small service (search, API)
request_body {
    max_size 10MB
}

# Large uploads (chat with file sharing)
request_body {
    max_size 20MB
}

# Matrix (needs big uploads for media)
request_body {
    max_size 100MB
}
```

Size limits per-service, not globally. Matrix needs 100MB for media; your search service doesn't.

## The Final Stack

```
┌─────────────────────────────────────────┐
│              Internet                    │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│              UFW Firewall                │
│  • Default deny                          │
│  • Allow 80/443/8448                     │
│  • SSH LAN only                          │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│              Fail2Ban                    │
│  • Watch logs                            │
│  • Ban repeat offenders                  │
│  • Feed bans to UFW                      │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│              Caddy                       │
│  • TLS termination (auto-certs)          │
│  • Security headers                      │
│  • Request size limits                   │
│  • JSON access logging                   │
│  • Reverse proxy to backends             │
└─────────────────┬───────────────────────┘
                  │
┌─────────────────▼───────────────────────┐
│          Internal Services               │
│  (Never exposed directly)                │
└─────────────────────────────────────────┘
```

## Verification

Test your security headers:

```bash
curl -sI https://your-domain.com | grep -E "(strict-transport|x-frame|x-content|permissions)"
```

Expected output:
```
strict-transport-security: max-age=31536000; includeSubDomains; preload
x-content-type-options: nosniff
x-frame-options: DENY
permissions-policy: geolocation=(), microphone=(), camera=()
```

Check fail2ban status:

```bash
sudo fail2ban-client status
# Should show: sshd, caddy-auth, caddy-badbots
```

## What's NOT Covered

This setup handles common threats but isn't complete:

- **No WAF** (Web Application Firewall) — Consider ModSecurity or Coraza for deeper inspection
- **No rate limiting** — Caddy's rate-limit module requires custom build
- **No GeoIP blocking** — Could add with MaxMind + custom rules
- **No IDS/IPS** — Consider Suricata for network-level detection

For a minimal reverse proxy, this is a solid baseline. Add layers as your threat model evolves.

---

**Time spent:** ~30 minutes

**Services running:** 3 (Caddy, sshd, fail2ban)

**Attack surface:** Minimal

Worth it? Absolutely. Sleep better knowing your gateway isn't wide open.
