---
title: "Moving OS to CachyOS... Maybe"
date: 2025-05-29T17:02:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

The past week or two I've been primarily using Debian and not noticing any issues or reasons to move to a different distro. Then... the great launcher fiasco of 2025 happened... The EVE Launcher updated to version 1.10.0 and stopped working with GE-Proton9-27, which was proving to be a rock-solid Proton version. I tried Experimental, Hotfix, and all 10-* versions, but they all failed for one reason or another. Usually, it was the launcher not starting, or the client not launching properly. I would just see a command window briefly and then nothing.

This made me think of CachyOS again. I hadn't been using it for a while, but I thought it might help with this new launcher update. So I switched over to CachyOS and let the launcher update. I experimented with different Proton versions, but none of the combinations worked. Even after trying additional Proton variables, nothing worked with the new launcher. Eventually, I rolled back to the older working launcher and returned to Debian, where all my files and settings were.

I stayed in Debian for another week or so until another launcher update came around, which I installed. The same issues persisted, so I tried other Proton versions again. This time, 10-3 opened the launcher, but the client still wouldn't run. This was a slight improvement in Debian—previously, the 1.10.0 launcher didn't even boot with the available Proton versions. I also tried Proton Experimental with the same results. Feeling experimental, I decided to switch back to CachyOS to run some comparative tests.

CCP had stopped the Linux clients from updating, so my CachyOS installation wasn't getting updates. I copied the launcher from Debian since that was easier than downloading the nupkg directly from CCP. I had just recently performed a system update on CachyOS, so everything was running on the latest kernel (6.15)—significantly more up-to-date than Debian. I'm not sure if this is a kernel-related issue, perhaps involving some system calls that are buggy in the older Debian kernel version. Whatever the reason, all the combinations I tried in CachyOS just worked: Proton Experimental and GE Proton10-3 both functioned correctly with the 1.10.0 launcher and the 1.10.1 version.

This adventure got me thinking... maybe I SHOULD move to CachyOS permanently. I would just need to develop a strategy for migration. I need to identify which customizations from my Debian setup are worth bringing over. Ironically, there's not much to bring to CachyOS since CachyOS itself inspired many of my changes in Debian :D. For example, the eza replacement for ls, along with the aliases that make it a drop-in replacement while preserving muscle memory.

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

Debian is installed on the larger partition (/opt/debian), with only 18GB of free space remaining. I'd need to review the disk usage again, but I believe most of the space is consumed by my JetBrains folders (both in .local and .cache home directories), pictures (around 20GB), the OS itself, and about 40GB of Ollama models that I rarely use. Fortunately, I have around 110GB of free space on the /opt/misc partition that I could use for temporary migration storage while I reconfigure the /opt/debian partition for another purpose.

I have another M.2 drive that I'd like to install in this laptop. I could partition it in a way that would allow me to continue distro-hopping occasionally, as I've always enjoyed exploring new distro releases. First, I need to get a small adapter piece 3D-printed and find the motivation to dig up my old computer to retrieve its main drive. That would also let me recover some data from it before repartitioning. The challenge is that I can't quite decide on the optimal partitioning scheme to use if/when I remove Debian.

Right now, I'm leaning toward leaving things as they are. Inside my CachyOS home directory, I've created symlinks to my Debian home folders, and I'll be adding a link to my ~/.m2 folder as well. I'm avoiding linking any dot folders/files to prevent potential conflicts between the two systems. My priority is simply ensuring that my personal files and folders are readily accessible from both operating systems.

I'll keep the status quo for now and just boot into cachy as my default OS. This will get me used to the OS and let me drive it daily to see if it gets flakey.