---
title: "Installing Linux From Scratch"
date: 2025-05-08T18:04:26-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I recently embarked on a journey to build linux from scratch.  Ideally I end up in a bootable partition with tools that I compiled for myself.  The process so far has been long and repetitive, unpackage this, cd there, ./configure this, make that, etc.  Let's see where I end up.

I already have a spare partition for another distro.  Part of me wants to install Arch or CachyOS on it and give that a go once.  I'd love a nice minimal clean KDE install once.  Another part just wants to goof around with it so I figured why not try LFS again.  Last time I only made it through compiling the compiler with the compiler that you compiled :D.  Always trips me out.  I wanted to make it past that.  So I prepped the mount point and the partition.

The first task is to compile a cross compiler.  I'm not entirely sure why or how this works entirely yet but it seems to be a way to make the new system independent of the host.  Must be something in the linking process that changes?  I'm not sure, haven't spent a whole lot of time in the c/c++ world.  Once that compiler was setup, I compiled just a few very basic tools - what seems like a basic compiler toolchain and then proceed to enter a chroot environment.  Compiler achieved!

The next part was compiling the basics for a working system.  This is just the basic cli stuff ranging from the simple cd all the way to the complex gawk.  A good majority of these are to support compiling other bits of software... which makes sense because I'll need much more software for this to be a fully standalone system.  Something less basic, like wget or curl... something to get out to the internet...

If you're curious and want to follow in my footsteps and compile your own gnu/linux system, here's the manual: https://linuxfromscratch.org/lfs/view/stable/index.html :D.  Enjoy!