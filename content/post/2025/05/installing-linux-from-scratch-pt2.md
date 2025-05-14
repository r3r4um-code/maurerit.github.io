---
title: "Installing Linux From Scratch Pt2"
date: 2025-05-15T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

I've now made it to a point in the document where they're talking about udev and how it works along with setting up the init scripts. I think the kernel is coming soon. It'll be interesting to see if my computer tries to melt itself again while compiling the kernel. The glibc compile was quite daunting. Had all 16 of my cores pegged for a good 10 minutes or more. Temps as high as 90°C with the fans blaring full stop. Good stuff.

Unfortunately this book won't get me to a bootable system though (well... it will...) since it's set up for MBR boot. I plan on seeing if updating GRUB in Debian will recognize the new kernel as another Linux install and add it to the boot menu, which would make it bootable for now. Here's to hoping.

I won't be able to do much with the new install unless they talk about setting up certificates and whatnot. I do have wget2 installed, which felt oddly easy to compile compared to wget. At first I didn't notice that I had wget2 until I tried to run wget... and no binary was found. This sent me back to wget's page to download the latest version of 1.x, which led me to a few other dependencies. I compiled those and then went to compile wget, but it wanted a trust source... I THINK that was a location to the certificates on the system, but we have none so it's time to get some.

Maybe BLFS will teach me the ways.