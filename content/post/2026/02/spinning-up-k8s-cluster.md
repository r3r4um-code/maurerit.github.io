---
title: "Spinning Up a 4-Node Kubernetes Cluster on Arch Linux"
date: 2026-02-14T17:00:00-05:00
author: "r3r4um"
draft: false
---

I just finished standing up a fully operational Kubernetes cluster—4 Arch Linux VMs (1 control plane, 3 workers), hardened with SSH key auth, UFW firewalls, audit logging, and RBAC. Here's what I learned doing it the hard way.

## The Setup

**Hardware:**
- Host: Ryzen 5 3600 (6 cores, 12 threads), 32GB RAM
- Control plane: k8s-control (192.168.50.193) — 2 cores, 2GB RAM
- Workers: k8s-worker-1/2/3 — 2 cores, 4GB RAM each
- Shared storage: NFS mount from maurer-plexy pointing to `/kube`

All nodes running Arch Linux with containerd as the container runtime.

## Phase 1: The Basics

Setting up 4 minimal Arch Linux VMs is straightforward if you've done it before. Each VM gets:
- Base Arch install with systemd-networkd for DHCP
- openssh + sshd (initially permissive, hardened in Phase 3)
- containerd with systemd cgroup driver
- kubeadm, kubelet, kubectl v1.35.1
- NFS client mounted to `/mnt/nfs` (persisted via fstab)
- Kernel modules (overlay, br_netfilter) + required sysctl settings

Nothing fancy. Just the pieces Kubernetes needs to function.

## Phase 2: Bootstrap — Where I Hit a Wall

Here's where I learned a hard lesson.

I ran `kubeadm init --pod-network-cidr=10.244.0.0/16` and... nothing. The API server stayed unreachable. Manifests existed in `/etc/kubernetes/manifests`, but the static pods (API server, controller-manager, scheduler, etcd) weren't launching.

The logs showed repeated kubelet crashes:

```
error: failed to load kubelet config file, path: /etc/kubernetes/kubelet-config.yaml
open /etc/kubernetes/kubelet-config.yaml: no such file or directory
```

I spent a while restarting services, checking logs, trying to figure out what was broken. But here's the thing I missed: **kubelet was starting *before* kubeadm created the config file it needed**.

The dependency chain is: `kubeadm init` creates `/etc/kubernetes/kubelet-config.yaml` → kubelet starts reading it → kubelet watches the manifests directory → static pods launch.

I broke that sequencing by having the kubelet systemd service try to start during Phase 1, before `kubeadm init` ran. Kubelet crashed repeatedly, couldn't watch the manifests, and the static pods never came up—even though the manifest files existed on disk.

**The lesson:** Don't troubleshoot symptoms. Map the dependency chain first. "file not found" isn't a symptom to work around—it's a root cause indicator. Fix the sequencing, not the symptoms.

Once I understood the real problem, the fix was straightforward: don't enable kubelet until after `kubeadm init` completes. Kubelet started cleanly on the second attempt, found the manifests, and spun up the static pods.

## Phase 2 (Retry): The Cluster Comes Online

After fixing the sequencing issue:
- Installed Flannel CNI
- Generated join tokens
- Added workers to the cluster
- All 4 nodes reported Ready

**Verification:**
```
kubectl get nodes
NAME           STATUS   ROLES           VERSION
k8s-control    Ready    control-plane   v1.35.1
k8s-worker-1   Ready    <none>          v1.35.1
k8s-worker-2   Ready    <none>          v1.35.1
k8s-worker-3   Ready    <none>          v1.35.1
```

CoreDNS running. Pod networking operational. CNI working as expected.

## Phase 3: Hardening

Once the cluster was stable, I locked it down:

**SSH Hardening (all nodes):**
- Disabled password authentication
- Disabled root login
- Set MaxAuthTries to 3
- Configured post-quantum KexAlgorithms (though OpenSSH 10.2p1 doesn't support them yet)
- Restricted to key-based auth only

**UFW Firewall (all nodes):**
- Default deny incoming, allow outgoing
- SSH access from the cluster subnet (192.168.50.0/24)
- Kubelet API (10250/tcp) from other nodes
- Pod network traffic (10.244.0.0/16)
- Flannel VXLAN (8472/udp)

One worker (k8s-worker-3) hit an edge case: UFW got enabled before all SSH rules were applied, locking me out. I recovered using a privileged Kubernetes debug pod to add the rules from inside the cluster. That's a good reminder to always add critical access rules *before* enabling the firewall.

**Audit Logging:**
- Configured kubelet audit policy on all nodes
- API server audit logging on control plane
- Captures pod exec/attach, secret access, RBAC changes

**RBAC:**
- Created `operators` namespace
- Added `node-operator` ServiceAccount + ClusterRole with read-only permissions
- Restricted to nodes, pods, events, and service account operations

## What I'd Do Differently Next Time

1. **Map dependencies upfront.** Kubernetes init has a specific order. Get that right before you start troubleshooting.
2. **Enable services after their dependencies are ready.** Kubelet needs its config file. Don't let it start until kubeadm creates it.
3. **Add firewall rules before enabling UFW.** SSH lockout recovery is possible but annoying.
4. **Test each phase incrementally.** Don't wait until the end to check if anything works.

## What's Next

The cluster is operational and hardened. I'm now running it as the shared compute layer for my homelab—spinning up pods as needed instead of maintaining special-purpose VMs. Storage via NFS, networking via Flannel, logging via audit trails.

It's a solid foundation. And honestly? Building it the hard way—hitting the wall, diagnosing the actual problem, fixing it—taught me more than following a tutorial blindly ever would.

---

**Built with:** Kubernetes v1.35.1, containerd, Flannel CNI, Arch Linux, kubeadm  
**Deployed on:** 4 VMs (1 control plane, 3 workers) on a Ryzen 5 3600 with 32GB RAM
