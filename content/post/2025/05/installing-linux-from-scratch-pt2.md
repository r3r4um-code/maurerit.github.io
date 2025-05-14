---
title: "Installing Linux From Scratch Pt2"
date: 2025-05-15T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

I've now made it to a point in the document where they're talking about udev and how it works along with setting up the init scripts.  I think the kernel is coming soon.  It'll be interesting to see if my computer tries to melt itself again while compiling the kernel.  The glibc compile was quite daunting.  Had all 16 of my cores pegged for a good 10 minutes or more.  Temps as high as 90c with the fans blaring full stop.  Good stuff.

Unfortunately this book won't get me to a bootable system though (well... it will...) since it's setup for MBR boot.  I plan on seeing if updating grub in debian will recognize the new kernel as another linux install and add it to the boot menu though so that's how it will become bootable for now.  Here's to hoping.

I won't be able to do much with the new install unless they talk about setting up certs and what not.  I do have wget2 installed which felt oddly easy to compile compared to wget.  At first I didn't notice that I had wget2 until I tried to run wget... and no binary was found.  So that sent me back to wget's page to download the latest version of 1.x.  Which led me to a few other dependencies.  So I compiled those and then went to compile wget and it wanted a trust source...  I THINK that was a location to the certs on the system but we have none so time to get some.

Maybe BLFS will teach me the ways.