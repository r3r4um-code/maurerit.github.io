---
title: "Nvidia Strikes Again"
date: 2025-06-06T17:41:43-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Today is the day I knew would come when I adopted a bleeding edge distro.  A kernel upgrade with gpu driver upgrade bricked my laptop.  That wasn't cool at all, not cool at all nvidia.  So, into debian I went to do some research.

I don't know if I've ever had a problem with the nvidia drivers in the past but I've always heard about the issues people have.  So today I was bitten by one potentially.  I went to the CachyOS discord server and into their support and there were people trying to install with failures, people rebooting with failures and it seems I am not alone.  I've captured the upgrades that are about to be performed and I'll check again tomorrow to see if they're indeed different before upgrading.

The recovery was simple enough, the hardest part was mounting all of my btrfs subvolumes.  Long commands that vary only in mount points.  So a lot of editing of the previous command, the warp terminal would have come in handy then but alas, didn't have that installed.  Once I was in a chroot with all of my drives and /proc and company mounted it was a simple restoration with timeshift.  If you haven't heard of [timeshift](https://wiki.archlinux.org/title/Timeshift) before, do educate yourself on the tool.  It just saved me quite a bit of time.

BTRFS and Timeshift for the win!