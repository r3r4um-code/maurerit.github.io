---
title: "Building linux-netfilter: When You Can't Get a Module, Build the Kernel"
date: 2026-03-11T15:01:00-04:00
author: r3r4um [r3r4um-code](https://github.com/r3r4um-code)
draft: false
---

Our caddyshack VM needs hypervisor-level network filtering. We're using libvirt nwfilter rules to restrict traffic at the KVM level — cleaner than in-VM firewalls, harder to circumvent. But libvirt nwfilter requires the kernel module `nf_tables_bridge` or at minimum `bridge_netfilter` to be present.

Arch ships both `linux` and `linux-zen` with `CONFIG_BRIDGE_NETFILTER=m` in their kernel configs. Module form should be fine, right?

Wrong.

The `.ko` files aren't actually built or shipped. They're in the config but not compiled. We tried `modprobe bridge_netfilter` — nothing. We dug through `/lib/modules` — nothing. The Arch team compiles these kernels with certain modules omitted from the binary build for size/maintenance reasons.

## The Failed Detour

Our first instinct: compile just the module.

We grabbed the Arch kernel source, tried to build just the bridge netfilter modules. Immediate failure: missing `Module.symvers`. That file is generated during a full kernel build and is required to compile any standalone module. There's no way around it.

Translation: we have to build the entire kernel.

## The Decision

Rather than just build it once on the KVM hypervisor and install it, we decided to do it properly:

- Create a proper Arch package: `linux-netfilter`
- Set up CI/CD to build on every kernel tag
- Use GPG signatures (both kernel source verification and package signing)
- Serve packages from a custom Arch repo
- Make it repeatable and automated

## The Build Environment Nightmare

Here's where things got messy.

### Attempt 1: Container on ubuntu-latest runner
We thought Gitea Actions could use a container image. We'd run the build inside an `archlinux:latest` container on an Ubuntu runner. Should work, right?

**Issues:**
- `actions/checkout@v4` requires Node.js, which isn't in the Arch container. Not compatible with Gitea Actions.
- Replaced with native `git clone` + SSH auth. OK.
- Package conflicts: `--overwrite='*'` on a partial system update broke `gcc-libs`. The container image was stale.
- **Fix:** Full `pacman -Syu --noconfirm --overwrite='*'` before installing anything else.
- Version extraction using `CI_REF` (wrong variable) meant the kernel tarball URL was malformed. Downloading HTML error pages instead of the kernel.
- **Fix:** Use `GITHUB_REF` (Gitea Actions mimics GitHub's env vars).
- `makepkg` refuses to run as root. Added a `builder` user and used `su` (no PAM in containers, so `sudo` doesn't work).

### Attempt 2: Running on Kubernetes worker nodes
Once the workflow was working, we ran it on the k8s cluster workers. Should be fine — they're just VMs, plenty of disk.

Except... they weren't.

Workers only had 20GB disks total. A kernel build churns through 15-20GB just in intermediate artifacts and source. By the time the linker stage hit, the disk was full. OOM killer.

**Initial fix:** Expand all three workers from 20GB to 40GB.

**Still not enough.** Kernel builds create a spike at the end (compression, final artifacts). Even 40GB wasn't cutting it.

We could either:
1. Expand to 60GB on all workers
2. Expand to 120GB on one worker and make it the dedicated builder
3. Build on plexy directly

We went with #2. Dedicated one worker (worker-1) as the build machine: 120GB disk, its own Gitea runner with the `large-builds` label, no other workloads.

Workers 2 & 3 keep the general `linux-x64` runner for other jobs.

### The Disk Space Fix

After the build spiked and filled the disk again, we added a cleanup step to the workflow:

```yaml
- name: Clean build artifacts
  run: |
    KERNEL_VERSION=${{ steps.version.outputs.VERSION }}
    cd /repo
    # Remove extracted source (frees ~15GB)
    rm -rf linux-${KERNEL_VERSION}
    # Remove tarball and signature
    rm -f linux-${KERNEL_VERSION}.tar*
    df -h /
```

Now the build cleans up after itself, freeing space before the signing and upload phases.

## The Verification Piece

We also made sure we're actually verifying the kernel source, not just theater.

The workflow now:
1. Downloads the kernel tarball
2. Downloads the GPG signature from kernel.org (signed by Linus or Greg KH)
3. Imports their keys from the Ubuntu keyserver
4. Decompresses and verifies the tarball against the signature
5. Recompresses
6. Computes the hash for the PKGBUILD

Only then do we compile. The chain is: kernel.org signs → we verify against known keys → we build and sign with our own GPG key → we publish.

## What We Learned

1. **Container base images are often stale.** Always `pacman -Syu` first in containerized builds, even if the image says "latest."

2. **Kernel builds have brutal disk requirements.** 40GB is tight, 60GB is cutting it, 120GB is comfortable. Always overprovision by at least 50%.

3. **Module compilation is a dead end.** If you need a kernel module and the distro doesn't ship it, rebuild the whole kernel. Don't waste time trying to compile standalone modules.

4. **GitHub Actions env vars work in Gitea Actions.** Gitea mimics the GitHub Actions interface pretty well, including `GITHUB_REF`, `GITHUB_REPOSITORY`, etc. But custom vars like `gitea.ref` don't exist.

5. **Kubernetes DaemonSets can get nuanced.** You can run multiple daemonset variants with different node selectors and labels. We split builders by workload: general-purpose on worker nodes 2 & 3, heavy builds on worker node 1.

6. **Cleanup matters.** A build step that removes intermediate artifacts can double your available disk space for the remaining steps.

## Where We Are Now

We've implemented all the fixes and the kernel build is **currently running** on worker node #1 with the 120GB dedicated disk. Expected completion in ~45 minutes.

Once complete:
- A working `linux-netfilter` Arch package with `CONFIG_BRIDGE_NETFILTER=y` built-in
- A Gitea Actions workflow that builds on every kernel tag
- GPG verification of the kernel source + signed packages
- A custom Arch repository served via Kubernetes + Caddy at a private URL
- A dedicated builder (worker node #1) with enough disk and memory for the job
- Automated cleanup to avoid future disk panics

The workflow is idempotent. Push a tag like `v6.20.0`, the runner builds, signs, publishes to the repo, and we can install it with `pacman -U`.

## Next: The Actual Deployment

Once the build finishes and the packages are live, we'll:
1. Install `linux-netfilter` on the KVM hypervisor
2. Reboot with the new kernel
3. Apply the libvirt nwfilter rules to caddyshack
4. Test that the filtering actually works

But at least now the infrastructure is in place. The hard part is done.
