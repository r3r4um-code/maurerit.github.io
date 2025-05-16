---
title: "Installing Linux From Scratch Pt3"
date: 2025-05-17T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I've made it to the kernel and have not been able to boot into the new OS. It all started with the first compile. While the compilation was successful and everything seems good, the only things I did differently than the manual were using tar 1.35 vs 1.34 and using Debian's GRUB to find and boot LFS. The kernel always panics stating:

```
/dev/root: Can't open blockdev
VFS: Cannot open root device "/dev/nvme0n1p4" or unknown-block(0,0): error -6
Please append a correct "root=" boot option; here are the available partitions:
```

And then it proceeds to list a few file system types.

I've tried several things. Using GPT UUIDs for the fstab entries, using /dev/nvme... notation for swap, boot EFI, and root. I tried manually tweaking the /boot/grub/grub.cfg to add some parameters to the LFS boot call. I've stripped it down to bare bones boot config, as outlined in the LFS book. Nothing has worked; all results in the same issue. I tried recompiling the kernel ensuring that I enabled NVMe support, as I thought I might have missed that option with compile one. Didn't work...

I'm sad and losing interest as I'm in WAY over my head. So I'm moving this to the virtual. I snapped a dd image of my partition so I can mount this in the VM that I just set up with Debian or in the same location I've been working. It'll be weird mounting /dev/vda vs /dev/vdaN. The lsblk tool doesn't show you multiple vda's like with my vdb and its partitions. But yet there is indeed a valid partition on that 'drive', just not a partition table, lol.

Time to play with Bazzite for a while; I need a palate refresher.