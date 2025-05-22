---
title: "Stability Over Bleeding Edge"
date: 2025-05-21T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: fase
---

Like I said yesterday, I've been trialing CachyOS and while I do enjoy it, I find myself being drawn back to Debian. I've been talking to a buddy about the experience so far, and one thing I found funny was that a couple of my apps weren't the latest and greatest like they are in Debian. One was installed from a Git repo, which I'd have to update myself outside the yay -Syu update process. The other wasn't the version that I wanted.

The first piece of software that I'm talking about is 1Password. There was no package built for an Arch distro, so they pointed me at a Git repo. After checking it out and being told to use makepkg... wait... what? I don't recall that program being part of the essential build packages. I later came to realize that this is an AUR package, so yay for being updated through yay :D. One solved, now onto the next.

The second piece of software is VS Code. I use it for its AI capabilities. Lately I've just been using it to proofread my posts so I sound less... I don't know what to say... it's not less intelligent, but that's the first thing that comes to mind. According to Claude, I make several mistakes that it constantly cleans up. Little things like calling the command line interface the cli instead of the CLI, EVE as eve, and X11 as x11, etc. I want my AI proofreader. No resolution for that yet, but I have managed to at least install a version in my .local folder and created a .desktop file for it, which seems to work. But the same AI chat view I'm used to isn't there. I must have an outdated version... I could have sworn selecting the model was much older than my CachyOS install.

I did forget about a third app, which really doesn't work like I want it to work. It's really weird. It has a system tray icon and SHOULD be closing into that, but when I close it, the app shuts down completely and the system tray icon goes away. The library the author uses doesn't support Arch with KDE—just Arch with GNOME. They don't call out that it doesn't work on KDE, but they have called out a lot of combinations that DO work.

The point of this post is to highlight that stability can rule supreme. While most of my software is either in a package that pacman can access or in an AUR repository, there's that one exception. That one, however, supports .deb packages and auto-updates. Plus the hassle-free installation of Microsoft's VS Code is nice to have. There are pieces of software where I want the latest and greatest, and IDEs are some of those pieces of software.

So my experience with CachyOS is that I find it a good environment to play around in. It helps me explore Linux and the ecosystem a bit more, but I don't see myself using it for real work any time soon. I keep being drawn back to Debian.