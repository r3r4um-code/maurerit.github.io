---
title: "Self-Hosting Forgejo on Kubernetes: Lessons from Three Phases of Deployment"
date: 2026-02-14T23:45:00-05:00
author: r3r4um [r3r4um-code](https://github.com/r3r4um-code)
draft: false
---

Over the past few hours, I deployed a complete self-hosted Git platform with CI/CD on Kubernetes. What started as "just deploy Forgejo" turned into a masterclass in reverse proxies, environment variables, and why you should always check where your configuration is actually coming from.

This post covers the journey through three deployment phases: getting Forgejo running, fixing SSH clone URLs, and deploying Actions runners. More importantly, it covers the lessons that will transfer to any containerized app deployment.

## Phase 1: The Basics (The Easy Part)

Deploying Forgejo itself was straightforward:
- Standard Kubernetes manifests (Deployment, Service, PVC)
- NFS-backed persistent storage on a network server
- SQLite database (no external dependencies)
- HTTP NodePort exposed for initial access

**What went wrong:** Everything else. But Forgejo itself? Fine.

## Phase 2: The SSH URL Nightmare (The Actual Problem)

This is where things got interesting. The requirement was simple: **make SSH clone URLs show the proper FQDN instead of internal pod IPs.**

### The Initial Approach

I configured `app.ini` with domain and SSH settings:
```ini
DOMAIN = git.example.com
SSH_DOMAIN = git.example.com
SSH_PORT = 2222
ROOT_URL = https://git.example.com/
```

Restarted the pod. Still showing internal IPs in clone URLs.

### Discovery 1: Reverse Proxy Headers

Behind a reverse proxy (Caddy), Forgejo wasn't recognizing the domain. Solution: add headers and tell Forgejo to trust them:

```
# Reverse proxy config
header_up X-Forwarded-For {http.request.remote.host}
header_up X-Forwarded-Proto {http.request.scheme}
header_up X-Forwarded-Host git.example.com
```

```ini
# app.ini
REVERSE_PROXY_TRUSTED_PROXIES = YOUR_PROXY_IP
```

Still didn't work through the web UI... but that's because I was testing via direct NodePort, which bypassed the reverse proxy entirely.

### Discovery 2: The Real Problem - Environment Variables Override Everything

After hours of troubleshooting, I finally checked the actual pod environment:

```bash
kubectl get pod -o jsonpath='{.spec.containers[0].env}'
```

Output revealed:
```json
[
  {"name":"GITEA__SERVER__SSH_PORT","value":"22"},
  {"name":"GITEA__SERVER__SSH_LISTEN_PORT","value":"2222"}
]
```

**There it was.** The Kubernetes environment variables were **overriding** all my app.ini settings. The deployment had `GITEA__SERVER__SSH_PORT=22`, which took precedence over the config file.

### Discovery 3: INSTALL_LOCK = false

Another problem hiding in plain sight:
```ini
INSTALL_LOCK = false
```

Forgejo was still in **installation mode**, which meant it wasn't fully respecting configuration. Setting this to `true` helped unlock the config.

### The Real Fix

Update the Kubernetes deployment to set the correct environment variables:

```yaml
env:
- name: GITEA__SERVER__DOMAIN
  value: "git.example.com"
- name: GITEA__SERVER__SSH_DOMAIN
  value: "git.example.com"
- name: GITEA__SERVER__ROOT_URL
  value: "https://git.example.com/"
- name: GITEA__SERVER__SSH_PORT
  value: "2222"
```

**The lesson:** When deploying containerized apps, environment variables are king. They override config files. Know your app's precedence order, or you'll spend hours debugging.

## Phase 3: Runners Without Docker (The Elegant Solution)

Now for CI/CD. Standard assumption: runners need Docker.

But the cluster uses **containerd** as its runtime, not Docker. The worker nodes don't have Docker installed.

### The Options

1. **Install Docker** - More components, more maintenance
2. **Use containerd directly** - Already installed, already working
3. **Remote Docker** - Unnecessary complexity

### The Solution

Mount the containerd socket directly in the runner pods:

```yaml
volumes:
- name: docker-sock
  hostPath:
    path: /run/containerd/containerd.sock
    type: Socket
volumeMounts:
- name: docker-sock
  mountPath: /run/containerd/containerd.sock
```

Configure the runner to use it:
```yaml
docker_host: unix:///run/containerd/containerd.sock
```

**Result:** Three act_runner pods, one per worker node, executing workflows via containerd. No Docker needed, no additional installation required.

**The lesson:** Don't assume you need Docker. Containerd is a first-class container runtime that can do everything Docker can do—and it's lighter weight. Use what you already have.

## Architecture Overview

```
User → git.example.com (Reverse proxy)
  ├─ HTTPS/HTTP → Forgejo pod (10.244.x.x)
  │   └─ NFS storage
  │
  └─ SSH:2222 → socat/iptables forward → k8s NodePort → Forgejo SSH

CI/CD Jobs:
  Workflow (push) → Forgejo Actions → Runners (DaemonSet)
    └─ Each runner → containerd socket → Execute containers
```

## Verification: It Works

After deploying the runners, I pushed a simple test workflow:

```yaml
name: Test Workflow
on: [push]
jobs:
  test:
    runs-on: linux-x64
    steps:
      - run: echo "Hello from Forgejo Actions!"
      - run: uname -a
      - run: id
```

✅ Workflow triggered automatically on push
✅ Runners executed all steps successfully
✅ Output visible in the web UI

The only "failure" was `docker ps` (Docker CLI not in image), which is expected and acceptable.

## Key Takeaways

### 1. Configuration Hierarchy Matters

When deploying apps (especially in containers):
- Environment variables often override files
- Check where your actual config is coming from
- Don't assume the file you edited is what the app is reading
- Test through the full stack, not just direct connections

### 2. Reverse Proxies Need Headers

If your app is behind a reverse proxy (Caddy, Nginx, HAProxy):
- Send X-Forwarded-* headers
- Tell your app to trust the proxy
- Test through the proxy, not directly to the pod

### 3. Use What You Already Have

Don't install components you don't need. We didn't install Docker because containerd was already there. Same execution, less overhead, simpler maintenance.

### 4. SQLite + Kubernetes = Recreate Strategy

If you use SQLite on a PVC:
- Use `strategy: Recreate` not `RollingUpdate`
- SQLite locks prevent concurrent access
- Overlapping pods will cause lock conflicts
- Scale down to 0, then back up (forces fresh pod)

### 5. Test the Full Path

I spent time debugging via direct NodePort when the real issue was in the deployment configuration. Always test through the entire stack—reverse proxy, networking, everything.

## What's Live Now

The platform is production-ready:
- ✅ Git repos accessible via SSH and HTTPS
- ✅ Web UI functional with proper domain
- ✅ CI/CD runners executing workflows
- ✅ NFS storage persistent across pod restarts
- ✅ All configuration in git for reproducibility

## For the Curious

If you want to try this yourself:

1. **Start simple** - Get the app running first
2. **Fix one thing at a time** - Don't layer multiple unknowns
3. **Check your app's config precedence** - Read the docs
4. **Test through the entire stack** - Not just direct connections
5. **Use the tools you have** - Containerd instead of Docker, existing infrastructure

The deployment manifests are available in the Forgejo instance repo. Every environment is different—adapt these patterns to your constraints.

**Questions or issues?** This setup is production-ready. Happy self-hosting.
