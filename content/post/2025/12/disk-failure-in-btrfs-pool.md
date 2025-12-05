---
title: "Disk Failure in Btrfs Pool"
date: 2025-12-05T10:15:52-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I found a disk in my lockbox that I thought was locked with the key lost. Sure enough, it had nothing important in it except this old NAS hard drive. I thought the drive was good enough to be written to once and then only read from (media storage). So I put it in my server, added it to my btrfs pool, and began filling up the hard drive.

I was transferring some media to the Plex library and rsync started acting all funky. It paused and then quit and gave me an error. Thinking it was just some networking issues, I deleted the file it was working on and restarted the sync. It made it about halfway through the file and started failing again. So I attempted to reboot the server and it just hung there forever. I was remote and didn't feel like going to the server, but it needed a hard power cycle. So I hard shut it down and restarted it, and all seemed well. Tried the transfer again and again with the errors.

I then tried to delete all the files that I just had transferred and it told me that the filesystem is read-only... Uh oh. The only thing I could think to do was get into recovery mode and try to salvage what I could off this drive. So I booted into an Arch ISO session, mounted the drive, and started transferring data off so I could free up enough space to remove the bad drive. I learned quite a bit about btrfs commands while doing it, such as `btrfs filesystem usage -T /mnt/point` which breaks out who has how much data. I then probed the disk in question (it had to be the new one, right?) and found that yes, for sure this drive was on its last leg.

After transferring off enough to get a good amount of free space, I attempted to `btrfs device remove /dev/device /mnt` and it went for a while and did some of what was needed but ultimately failed. So I consulted ChatGPT... I tried consulting my local AI but that is hosted by this server, lol. So ChatGPT it was. It told me that it seemed the only course of action was to `btrfs device delete /dev/device /mnt` which took a while and I monitored the disk usage. I saw my other two drives' free space going down while the used space on the dying drive was going down. Interesting—I figured device delete was a very destructive thing, but my media library has been preserved intact so far.

Always check your drives before using them, SMART data would have told me this drive was trash.