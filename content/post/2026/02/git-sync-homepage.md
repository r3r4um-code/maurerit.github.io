---
title: "Self-Updating Homepage: Git-Sync + Nginx on Kubernetes"
date: 2026-02-14T18:10:00-05:00
author: "r3r4um [r3r4um-code](https://github.com/r3r4um-code)"
draft: false
---

Running a website that auto-updates from your git repo is a solved problem. Deploy git-sync as a sidecar, let it pull every few minutes, and your latest commits appear live without pod restarts.

## The Setup

**Goal:** Push to main branch → content appears on www.r3r4um.org within 5 minutes, no manual intervention.

**Architecture:**
- Kubernetes Deployment with two containers: nginx + git-sync
- Shared `emptyDir` volume for git clone
- git-sync sidecar syncs repo → `/git`, creates symlink to worktree at `/git/html`
- nginx serves from `/git/html`
- Init container handles SSH key setup and GitHub host key configuration
- NodePort 30000 exposed through reverse proxy

## The Container Problem

First attempt: mount SSH key directly to `/root/.ssh/id_ed25519`. Sounds simple. Doesn't work.

**The issue:** git-sync container doesn't read `/root/.ssh/id_ed25519` by default. It looks for keys in `/etc/git-secret/ssh` or requires explicit `--ssh-key-file` flag. Additionally, strict host key checking was failing because GitHub's host key wasn't in `known_hosts`.

**The fix:** Init container. Copy the key, set permissions, pre-populate known_hosts, and let git-sync reference it explicitly.

## The Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: www-r3r4um-org
spec:
  replicas: 1
  selector:
    matchLabels:
      app: www-r3r4um-org
  template:
    metadata:
      labels:
        app: www-r3r4um-org
    spec:
      securityContext:
        runAsUser: 0
      
      # Init container: SSH key setup
      initContainers:
      - name: ssh-key-setup
        image: busybox:latest
        command:
          - sh
          - -c
          - |
            cp /ssh-secret/id_ed25519 /ssh/id_ed25519
            chmod 600 /ssh/id_ed25519
            # Pre-populate known_hosts with GitHub's SSH keys
            echo "github.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOMqqnkVzrm0SdG6UOoqKLsabgH5C9okWi0dh2l9GKJl" > /ssh/known_hosts
        volumeMounts:
        - name: ssh-secret
          mountPath: /ssh-secret
          readOnly: true
        - name: ssh-config
          mountPath: /ssh
      
      containers:
      # git-sync sidecar
      - name: git-sync
        image: registry.k8s.io/git-sync/git-sync:v4.4.0
        args:
          - --repo=git@github.com:r3r4um-code/www-r3r4um-org.git
          - --ref=main
          - --root=/git
          - --period=300s
          - --link=html
          - --ssh-key-file=/ssh/id_ed25519
          - --ssh-known-hosts-file=/ssh/known_hosts
        volumeMounts:
        - name: git-volume
          mountPath: /git
        - name: ssh-config
          mountPath: /ssh
          readOnly: true
      
      # nginx web server
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        volumeMounts:
        - name: git-volume
          mountPath: /git
        - name: nginx-config
          mountPath: /etc/nginx/conf.d
      
      volumes:
      - name: git-volume
        emptyDir: {}
      - name: ssh-secret
        secret:
          secretName: git-ssh-key
          defaultMode: 0400
      - name: ssh-config
        emptyDir: {}
      - name: nginx-config
        configMap:
          name: nginx-config
---
apiVersion: v1
kind: Service
metadata:
  name: www-r3r4um-org
spec:
  type: NodePort
  selector:
    app: www-r3r4um-org
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30000
```

**The init container:**
- Copies the private SSH key from the Kubernetes Secret
- Sets correct permissions (chmod 600)
- Pre-populates `/ssh/known_hosts` with GitHub's public host keys
- Runs as root, finishes before git-sync starts

**git-sync sidecar:**
- Uses command-line arguments instead of environment variables (more reliable)
- `--period=300s` = sync every 5 minutes
- `--link=html` = create symlink from the git worktree to `/git/html`
- `--ssh-key-file` and `--ssh-known-hosts-file` point to init container's setup

**nginx:**
- Serves from `/git/html` (the symlink git-sync creates)
- ConfigMap provides nginx config

## The Secrets

SSH key lives in a Kubernetes Secret:

```bash
kubectl create secret generic git-ssh-key \
  --from-file=id_ed25519=/path/to/your/private/key
```

Then add the **public key** as a Deploy Key on GitHub:

```bash
ssh-keygen -y -f /path/to/your/private/key
# Copy the output and paste into GitHub → Settings → Deploy Keys
```

Deploy keys are read-only by default and limited to one repo. Safer than personal access tokens.

## The Reverse Proxy

The Kubernetes NodePort (30000) is internal. To expose it on a real domain:

**Caddyfile:**
```
www.r3r4um.org {
    reverse_proxy http://worker-1:30000 \
                   http://worker-2:30000 \
                   http://worker-3:30000
}
```

Caddy handles HTTPS automatically (Let's Encrypt), redirects HTTP → HTTPS, and load-balances across all worker nodes.

## The Workflow

**To update your homepage:**

1. Edit `index.html` locally
2. Commit and push to main branch
3. Wait up to 5 minutes
4. Refresh www.r3r4um.org

No pod restarts. No manual syncing. No rolling deployments. Just push and wait.

## What Could Go Wrong

**git-sync not syncing?**
- Check `kubectl logs deployment/www-r3r4um-org -c git-sync`
- Verify SSH key is added as a Deploy Key on GitHub (not just your personal key)
- Make sure known_hosts is pre-populated (strict host key checking is on by default)

**nginx serving old content?**
- git-sync creates a symlink; nginx follows it automatically
- If git-sync hasn't synced recently, check logs for authentication errors
- If synced but nginx stale, wait for the 5-minute sync cycle

**SSH key permission errors?**
- Init container must run as root and set chmod 600
- Secret volume must have defaultMode: 0400 (read-only)
- Check `kubectl exec` into the pod and verify `/ssh/id_ed25519` exists with correct permissions

## Lessons

1. **Init containers are underrated.** They run once, do setup work, and get out of the way. Perfect for SSH key configuration.

2. **Pre-populate known_hosts.** git-sync won't add GitHub's host key automatically. Do it in the init container or disable strict host key checking (less secure, but faster).

3. **Command-line args > environment variables.** git-sync's ENV variable parsing is fragile (missing units, misspellings). Use `args:` in the pod spec instead.

4. **Symlinks work.** git-sync's `--link` flag creates a symlink to the current worktree. nginx serves right through it. No extra configuration needed.

5. **NodePort + reverse proxy.** Don't expose Kubernetes NodePorts directly. Put a reverse proxy in front, handle HTTPS there, and load-balance across multiple nodes.

---

This setup has been running www.r3r4um.org since 2026-02-14. Push a commit to the repo, wait 5 minutes, and the site updates. Simple and effective.
