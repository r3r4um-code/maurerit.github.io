---
title: "Stability Over Bleeding Edge"
date: 2025-05-21T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

Like I said yesterday, I've been trialing CachyOS and while I do enjoy it, I find myself beintg drawn back to Debian.  I've been talking to a buddy about the experience so far and a couple things I found funny was that a couple of my apps weren't the latest and greatest like they are in Debian.  One was installed from a git repo, which I'd have to update myself outside the yay -Syu update process.  The other wasn't the version that I wanted.

The first piece of softare that I'm talking about is 1password.  There was no package built for an Arch distro so they pointed me at a git repo.  After checking it out and being told to use makepkg... wait... what?  I don't recall that program being part of the essential build packages?  I later came to realize that this is an AUR package so yay for being update through yay :D.  One solved, now onto the next.

The second piece of software is VS Code.  I use it for it's AI capabilities.  Lately just been using it to proofread my posts so I sound less... I don't what to say... it's not less intelligent but that's the first thing that comes to mind.  According to claude I make several mistakes that it constantly cleans up.  Little things like calling the command line interface the cli instead of the CLI, EVE as eve... and X11 as x11... etc.  I want my AI proofreader.  No resolution for that yet but I have managed to at least install a version in my .local folder and created a .desktop file for it which seems to work.  But the same AI chat view I'm used to isn't there.  I must have an outdated version... I could have sworn selecting the model was much older than my CachyOS install.

I did forget about a third, which really doesn't work like I want it to work.  It's really weird.  It has a systray icon and SHOULD be closing into that but when I close it, the app shuts down completely and the systray icon goes away.  The library the author uses though doesn't support Arch with KDE.  Just Arch with Gnome.  They don't call out that it doesn't work on KDE but they have called out a lot of combinations that DO work.

The point of this post is to kind of point out that stability can rule supreme.  While most of my software is either in a package that pacman can access or in an AUR repository except the one.  That one however supports .deb's and auto updates.  Plus the hassle free installation of MS's VS Code, that's one that is nice to have.  There are pieces of software where I want the latest and greatest and IDE's are some of those pieces of software.

So my experience with CachyOS is that I find it a good environment to play around.  Explore Linux and the ecosystem a bit more but I don't see myself using it for real any time soon.  I keep being drawn back into Debian.