---
title: "Hyprland With Omarchy"
date: 2025-10-04T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I've recently come up with a good use case for using a tiling window manager.  A good workflow that requires just a few windows, one application and 3 terminals.  Should be a perfect fit for a tiler like hyprland on a 1080p monitor.

I've recently been seeing videos about a new 'distro' that is making waves.  I guess it's a distro, more like a shared ricing done on arch and hyprland.  Although, it has it's own ISO and installer so I guess we can call it a proper distro.  What I'm talking about is Omarchy, a passion project utilizing Arch, as the base, and with Hyprland and some other programs I haven't fully dove into just yet to make up the desktop.

From the get go you're introduced into something really special.  A TUI installer with many opinions that will get you into a custom themed, consistent compilation of programs to make a full desktop environment.  Everything is styled with absolute consistency.  GRUB... although it might not be GRUB now that I think about it... the login screen... the SCREENSAVER... yes, that's right.  It has a screen saver as if it's 2005 and we don't have monitors that go to sleep on their own.  And it's glorious, lol.  Seriously, this thing is an 8bit work of art... maybe 16bit but it's definitely got this kind of NES vibe about it.

The configuration is painstakingly put together.  Sliced up into reasonably sized and appropriately named chunks so it's easy to find your way into what you want to tweak.  Need to tweak some short cuts?  Hyprland calls these bindings and Omarchy has them in a bindings.conf file.  Easy peasy.  Hyprland even updates instantly when you tweak a binding.  I'm not sure if that's the default or if Omarchy did something to enable that but it's definitely a nice to have and kind of a requirement.

The install is fairly slim, clocking in at around 1,000 packages or so installed including things like Docker, neovim, git and other various developer tools.  Like I said, I haven't fully dove into what is offered as I had a completely different use case in mind so a lot of what is installed is useless to me with that machine... for now at least.  It's a laptop but it has decent specs so I may disconnect the battery and plug it in permanently and make it a server :D.  That's a discussion for another day though.

The task at hand involves some cli tools in the terminals, long running tasks too so it's one step in one terminal which then feeds the application I'm running.  Then from there we go to a second terminal where we move the resulting files onto the nas.  The third terminal is just because I like having a terminal for utility functions around.  The first two terminals have a lot of up arrows being hit :D so I don't like other commands to be in the mix there.

I of course am using some other desktops as well.  I always install discord and signal of which signal was already installed and bound to a binding.  So I bound discord and opened it and signal on the first desktop, the second desktop is the main function of the setup and then the third desktop is a browser and another terminal sometimes.  Other than that I've just been messing about with the available themes and the other menu options provided by the far left button on the bar at the top.  I might have been able to roll my own hyprland setup with like 600 packages or so I bet.  I should try that and then steal Omarchy's configs.