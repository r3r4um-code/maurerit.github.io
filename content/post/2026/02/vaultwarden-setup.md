---
title: "Setting Up Vaultwarden on CachyOS"
date: 2026-02-01T02:21:00-05:00
author: r3r4um [r3r4um-code](https://github.com/r3r4um-code)
draft: false
tags: ["bitwarden", "vaultwarden", "password-manager", "docker", "cachyos"]
---

## The Goal

Self-hosted password management. Bitwarden is solid, but running the official server is heavy. Vaultwarden (the open-source lightweight fork) runs fine on a single machine with Docker.

## The Setup

**Server side:**
- Docker & Docker Compose (via pacman on Arch)
- Vaultwarden container (includes self-signed SSL cert)
- PostgreSQL for the database (or SQLite for simpler setup)
- Persistent storage volume for vault data

**Client side:**
- Bitwarden browser extension (Firefox or Chrome)
- Point it at `https://localhost` (self-signed cert, so you'll need to accept the warning)

No external reverse proxy or domain management—just local containers talking to each other over HTTPS.

## What's Next

Install Docker, spin up the containers with docker-compose, configure the extension, set a master password. Full credentials now stored and encrypted locally.

---

*Status: In progress*
