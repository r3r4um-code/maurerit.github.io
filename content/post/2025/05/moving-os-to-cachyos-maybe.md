---
title: "Moving OS to CachyOS... Maybe"
date: 2025-05-29T17:02:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

The past week or two I've been primarily in Debian and not noticing any issue's or any reason to move to a different distro.  Then... the great launcher fiasco of 2025 happened...  The EVE Launcher updated to version 1.10.0 and stopped working with GE-Proton9-27 which was proving to be a rock solid proton version to be on.  I tried Experimental, Hotfix, 10-* all of which fail for one reason or the other.  Usually it's the launcher not launching, the other is that the client doesn't launch properly.  I just saw a command window briefly and then nothing.

This made me think of CachyOS again.  I haven't been over here for awhile now and I might need it to play around with this new launcher update.  So over to Cachy I went.  I let the launcher update and mess around with different proton versions only to have none of the combinations work.  I tried with some additional proton variables but nothing with the new launcher.  So I rolled back to the older working launcher.  I head back over to Debian because that's where everything is.

I stay in Debian for another week or so and then another launcher update comes around and I let it happen.  The same thing happens so I try the other Proton versions again and this time 10-3 opens the launcher but the client doesn't open.  This is a difference in Debian - the 1.10.0 launcher didn't even boot with the then available proton versions.  I try proton experimental and the same thing there.  So, since I'm in a playing mood, I head back over to cachy to perform some of the same tests.

CCP Stopped the linux clients from updating so Cachy wouldn't update, I stole the launcher from Debian as it was easier than downloading the nupkg from CCP.  I had just recently done a system update so everything was up to date running the latest kernel@6.15 so quite a bit more up-to-date than Debian is.  I have no clue if this is some kind of kernel issue, could be some kind of call being translated into a kernel call that is buggy in the Debian kernel version?  Who knows for sure but all the combinations I cared to try in Cachy just worked.  Proton Experiment, GE Proton10-3 both with the 1.10.0 launcher and the 1.10.1.

This adventure got me to thinking... maybe I SHOULD move over to cachy permanently.  I would just need to come up with a stragedy to do so.  I need to see what customizations I have over in debian that I care to bring over here.  I think there is much to bring to Cachy as Cachy is the one that spurred some of the changes over in Debian :D.  Like the eza replacement for ls along with the aliases to make it a drop in replacement and keep old muscle memory.

So, I have my harddrive partitioned as such:

```
nvme0n1     953.9G  0 disk 
├─nvme0n1p1   3.7G  0 part /boot/efi
├─nvme0n1p2  14.9G  0 part [SWAP]
├─nvme0n1p3 186.3G  0 part /opt/debian
├─nvme0n1p4  93.1G  0 part /var/tmp
│                          /var/log
│                          /var/cache
│                          /root
│                          /srv
│                          /home
│                          /
└─nvme0n1p6 655.8G  0 part /opt/misc
```

Debian is obviously on the larger drive there (/opt/debian) with the majority of the space being consumed, there is 18g free on that drive.  I'd have to pull up the breakdown again but I believe the majority of it is in my Jetbrains folders, both the .local and the .cache home directories.  Quite a bit of space consumed by pictures, maybe 20g by the OS and then like 40g consumed by ollama models which I never really used.  I have around 110g spare on the /opt/misc drive there that I can move things to for migration purposes while I rejigger /opt/debian into something else.

I have another m.2 drive I want to get installed in this laptop, I could partition that off in a way where I can still distro hop from time to time as I have always liked checking out new releases of distro's.  I just need to get a little piece printed first and get the motivation to dig up the old computer and rip out the main drive.  That'll allow me to recover some data from it as well before partitioning.  But I can't for the life of me figure out how I want to repartition if/when I remove Debian.

Right now I'm thinking I'll just leave things the way they are.  Inside my home directory are symlinks to my Debian home folders, I will be linking my ~/.m2 folder as well.  I won't be linking any dot folders/files as I don't want to introduce any problems on either side of the dual boot.  I just want my home files and folders to be readily accessible.

