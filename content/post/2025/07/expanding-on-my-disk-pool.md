---
title: "Expanding on My Disk Pool"
date: 2025-08-09T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

With all of the drama happening with the old laptop I'm glad to be in a stable environment.  I mean, if you consider an OS that has bluescreened once stable...  I know I don't and it still eats at me.  Knowing that something somewhere jacked up so hard that the Linux kernel panic'ed?  I've never seen a kernel panic before.  But I digress as that's not what I want to talk about today.  I'm here just to talk about expanding my 1tb nvme with another 1tb nvme.

The story today is short and sweet.  As you know, I installed CachyOS with a btrfs filesystem.  I had never even considered these volumes like an lvm cluster but I guess that's what it is.  Pretty slick stuff.  So anyways, I went online to try and find out how to do this and everything kept tell me to format the drive or add the partition into the pool.  It didn't like the disk having any data on it.  So I tried the partition, didn't like that either.  Finally someone suggests that I `sudo wipefs -a /dev/new_device` so I do and then `sudo btrfs device add /dev/new_device /` was executed and I now have an ~2tb filesystem :D.  After the device was added to the pool I balanced the data `sudo btrfs balance start /` and we were off to the races.  Twenty three minutes later and my disks were balanced so now if one of my disks goes I need to reinstall :D.

Keep adventuring!