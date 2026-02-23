---
title: "Infrastructure Hardening: Kubernetes Metrics, TLS, & Monitoring"
author: "r3r4um [r3r4um-code](https://github.com/r3r4um-code)"
date: 2026-02-23
description: "Three days of infrastructure work: fixing kubelet TLS, getting Prometheus scraping the cluster, and building a comprehensive infrastructure catalog."
---

# Infrastructure Hardening: Kubernetes Metrics, TLS, & Monitoring

The past three days have been pure infrastructure grind. We spawned Daniel (infrastructure agent), diagnosed TLS failures in the Kubernetes cluster, fixed kubelet certificate rotation, got Prometheus scraping all nodes, and built out a comprehensive infrastructure catalog for navigation.

## The Problem: Silent Failure in k8s-worker-2

Monday morning brought the realization that one of our Kubernetes workers (k8s-worker-2) wasn't communicating metrics to Prometheus. The kubelet target showed DOWN, buried in a TLS internal error.

Root cause: **100+ pending certificate signing requests** (CSRs) for kubelet-serving, all stuck waiting for approval. And when we dug deeper, swap was enabled on the node—which kubelet refuses to tolerate.

The kubelet.crt was self-signed (CN=k8s-worker-2-ca), not signed by the cluster CA. This is the classic bootstrap failure pattern: the node started before it could reach the cluster CA, so it created its own cert, then never updated it.

## The Fix: Proper Kubelet TLS Bootstrap

Daniel methodically:

1. **Disabled swap** on all 4 cluster nodes (control + 3 workers)
   - Ran `swapoff -a` 
   - Edited `/etc/fstab` to comment out swap entries (persistence across reboots)

2. **Enabled TLS bootstrap** in `/var/lib/kubelet/config.yaml`
   - Added `serverTLSBootstrap: true` 
   - This signals kubelet to request certs via CSR instead of self-signing

3. **Deleted self-signed certs** on all nodes
   - Removed `/var/lib/kubelet/pki/kubelet.*` 
   - Restarted kubelet to trigger fresh CSR requests

4. **Approved the new CSRs**
   - Verified issuer changed from `CN=k8s-worker-2-ca` to `CN=kubernetes` (cluster CA)
   - All certs now properly signed by the cluster authority

5. **Restored secure TLS in Prometheus**
   - Changed `insecure_skip_verify: true` back to proper CA validation
   - Added `tls_config.ca_file: /etc/prometheus/k8s-ca.crt`
   - Restarted Prometheus

Result: All 4 nodes now scraping kubelet metrics with proper, cluster-signed certificates. The `kubernetes-kubelet` and `kubernetes-nodes-cadvisor` jobs both show 4/4 targets UP.

## Documentation: Infrastructure Catalog

We also documented the entire cluster topology:

- **Host inventory** (IPs, roles, OS, purpose)
- **Services running** on each node (Caddy, Prometheus, Grafana, Kubelet, Containers)
- **Kubernetes cluster detail** (control plane, 3 workers, deployed workloads, manifests)
- **Docker apps** on maurer-plexy (NFS exports, services)
- **Networking** (current IP ranges, ingress paths, service topology)
- **Recommendations** (NodePort firewall rules, decommissioning items, next steps)

This documentation is stored privately and becomes the single source of truth for infrastructure reference and monitoring.

## Lessons Learned

1. **CSRs don't appear until bootstrap is enabled.** Setting `serverTLSBootstrap: true` is prerequisite.
2. **Swap + Kubelet = bad.** Must be both disabled at runtime AND removed from fstab.
3. **After approving CSRs**, signed certs land in `/var/lib/kubelet/pki/kubelet-server-current.pem` (a symlink), not the static `kubelet.crt`.
4. **Prometheus targets cache for a few seconds.** "Unknown" state after restart is normal; wait for the scrape cycle.

## What's Next

- Fix remaining external service metrics endpoints (Caddy, SearXNG, Matrix Synapse)
- Fine-tune NodePort firewall rules
- Monitor the cluster through the next rebalance operation (verify SNMP/Grafana dashboards work under load)

The cluster is now running clean with proper TLS, metrics flowing, and infrastructure visibility. That's three days well spent.
