+++
title = "Building a Minimal Reverse Proxy with Caddy and Arch"
date = 2026-02-14T12:00:00-05:00
author = "r3r4um [r3r4um-code](https://github.com/r3r4um-code)"
tags = ["infrastructure", "caddy", "arch-linux", "reverse-proxy", "sysadmin"]
categories = ["infrastructure"]
draft = false
+++

Today I built a minimal reverse proxy from scratch, and it was *delightfully* chaotic.

## The Problem

I had a working nginx reverse proxy on an upstream server, but wanted to consolidate infrastructure and reduce attack surface. The solution: a dedicated minimal Arch Linux VM running Caddy as a reverse proxy, with absolutely nothing else.

Just a kernel, systemd, sshd (local only), and Caddy. Everything else is noise.

## Act 1: The Broken Boot

Fresh Arch install, reboot, and... GRUB shell. No error message. Just a prompt.

Classic.

**The issue:** grub-mkconfig was never run. The system was installed, GRUB was placed, but the configuration file that tells GRUB how to boot was missing.

**The fix:**
```bash
# From the Arch ISO, mounted at /mnt
arch-chroot /mnt grub-mkconfig -o /boot/grub/grub.cfg

# Also the boot order was wrong (DVD-ROM first)
arch-chroot /mnt efibootmgr -o 0004,0002,0003,0000,0001
```

Boot order matters. Learned that the hard way twice.

## Act 2: Network Silence

System boots, but no network. I had configured `systemd-networkd` and `systemd-resolved` but something wasn't wired correctly.

**The culprit:** `/etc/resolv.conf` was a regular file pointing to some old config, not a symlink to `systemd-resolved`'s stub resolver.

**The fix:**
```bash
rm /etc/resolv.conf
ln -s /run/systemd/resolve/stub-resolv.conf /etc/resolv.conf
```

Also created a proper network config:
```toml
# /etc/systemd/network/20-wired.network
[Match]
Name=enp*

[Network]
DHCP=yes
DNS=1.1.1.1
DNS=8.8.8.8

[DHCPv4]
UseDNS=yes
```

Redundancy: DHCP + static fallback DNS. Network came right up.

## Act 3: The Migration

Now the fun part: converting complex nginx configs to Caddy.

**What I had to migrate:**
- Search service → SearXNG
- Chat service → with websockets + caching rules
- Matrix → complex routing:
  - `/` → Element web client
  - `/_matrix/*`, `/_synapse/*`, `/.well-known/matrix/*` → Synapse
  - `/admin/*` → Synapse admin
  - `:8448` → Federation port
- Matrix admin → Admin panel

**Caddy is beautifully simple.** No separate files for TLS, no certbot hooks, no cache rules scattered everywhere. Just:

```caddy
search.example.com {
    reverse_proxy upstream-ip:8889
}

chat.example.com {
    reverse_proxy upstream-ip:3002 {
        header_up X-Real-IP {remote_host}
    }
    
    @static path *.css *.jpg *.png *.gif *.ico *.svg *.woff *.woff2 *.ttf *.eot
    header @static Cache-Control "public, max-age=604800, immutable"
}

matrix.example.com {
    @synapse path /_matrix/* /_synapse/* /.well-known/matrix/*
    reverse_proxy @synapse matrix-ip:8008
    
    @admin path /admin/*
    reverse_proxy @admin matrix-ip:8009
    
    reverse_proxy matrix-ip:80  # Element (default)
}

matadm.example.com {
    reverse_proxy matrix-ip:8009
}

matrix.example.com:8448 {
    reverse_proxy matrix-ip:8008
}
```

That's it. No SSL directives. No proxy_set_header repetition. Caddy figures out HTTPS automatically, obtains certs from Let's Encrypt, and handles all the boring stuff.

**And it just works.**

## Act 4: The Debugging Twist

Testing from external, the search service returns `ERR_CONNECTION_REFUSED`.

But `curl -I https://search.example.com` from the internet? 200 OK.

From Caddy itself? 200 OK.

What the...?

Caddy logs show the cert was obtained. All the routes are configured. The backend is reachable.

**The culprit:** My local `/etc/hosts` file had an old entry pointing to the old server.

😂

One line deleted, and we're live.

## The Result

A minimal, secure, bulletproof reverse proxy:

- **1 service running**: Caddy
- **Automatic HTTPS**: Let's Encrypt integration, no manual renewal
- **Simple config**: Human-readable, modular
- **Fast**: Caddy is lean and fast
- **Secure**: Minimal surface area, isolated from other services

The entire system fits in a ~20GB VM. No bloat. No complexity. Just domain names, some Caddy configs, and the internet flows through it.

---

**Lessons learned:**

1. Boot order matters (learned twice, apparently)
2. Systemd networking is solid once you understand resolv.conf symlinks
3. Caddy makes reverse proxy setup almost boring
4. Always check `/etc/hosts` before you blame the network
5. Minimal Arch boxes are delightfully simple to reason about

Next: WKD (Web Key Directory) for key distribution. Should be even simpler—just static files and Caddy serving them.

Worth the afternoon? Absolutely.
