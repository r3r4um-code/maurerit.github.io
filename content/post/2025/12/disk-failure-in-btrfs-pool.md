---
title: "Disk Failure in Btrfs Pool"
date: 2025-12-05T10:15:52-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I found a disk in my lockbox that I thought was locked and key lost.  Sure enough it had nothing important in it except this old nas hard drive.  I thought the drive was good enough to be written to once and then only read from (media storage).  So I put it in my server, add it to my btrfs pool and begin filling up the hard drive.

I was transfering some media to the plex library and rsync started acting all funky.  It paused and then quit and gave me an error.  Thinking it was just some networking issue's I deleted the file it was working on and restarted the sync.  It made it about half way through the file and started failing again.  So I attempt to reboot the server and it just hangs there forever.  I was remote and didn't feel like going to the server but it needed a hard power cycle.  So I hard shut it down and restart it and all seems well.  Try the transfer again and again with the errors.

I then try to delete all the files that I just had transfered and it tells me that the filesystem is read only...  Uh oh.  The only thing I can think to do is get into recovery mode and try to salvage what I can off this drive.  So I boot into an arch iso session, mount the drive and start tranfering data off so I can free up enough space to remove the bad drive.  I learned quite a bit about btrfs commands while doing it such as `btrfs filesystem usage -T /mnt/point` which breaks out who has how much data.  I then probe the disk in question (it had to be the new one right?) and find that yes, for sure this drive is on it's last leg.

After transfering off enough to get a good amount of freespace I attempt to `btrfs device remove /dev/device /mnt` and it goes for awhile and does some of what's needed but ultimately fails.  So I consult chat gpt... I tried consulting my local AI but that is hosted by this server, lol.  So chatgpt it is.  It tells me that it seems the only course of action is to `btrfs device delete /dev/device /mnt` which takes awhile and I monitor the disk usage.  I see my other two drives free space going down while the used space on the dying drive is going down.  Interesting, I figured device delete was a very destructive thing but my media library has been preserved in tact so far.

Always check your drives before using them, SMART data would have told me this drive was trash.