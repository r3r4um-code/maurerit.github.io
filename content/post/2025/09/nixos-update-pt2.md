---
title: "NixOS Part 2"
date: 2025-09-20T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---


I've been using NixOS for four days now. I'm not using it exclusively, as I frequently jump back into CachyOS for one main reason: my dock icons. On NixOS, for some weird reason, Discord, Signal, and Konsole stop displaying their icons and instead show a blank icon on desktops where the app isn't open. However, having them broken in NixOS is making me question my use of the dock at the bottom. I rarely use it, except on my "surfing" desktop. My main way of switching apps is by changing desktops. As I've mentioned before, I have nine desktops arranged in a 3x3 grid. This setup covers 90% of my use cases, and the other 10% happens in the middle workspace with my browser(s). So... do I really need a dock at all? That's something I'll be questioning moving forward.

This leads me to another quirk I encountered on NixOS once I started having Home Manager manage my dotfiles—at least the dotfiles I have it managing via source files in my `/etc/nixos` folder. My Plasma Shell configuration: if I want to update, say, my panels, I have to stop NixOS from tracking that file (causing a new generation), make my changes, copy the appropriate file back into the config folder, rebuild and switch (causing another generation), and then hope I did everything right. I think this is a bad way of handling these files. I'm definitely moving to GNU Stow to handle them. That way, I can do it on CachyOS as well and share the configs... maybe. I'm still concerned about either losing config options when downgrading KDE (Nix is on 6.3, Cachy on 6.4 and soon 6.5) or breaking one of them and causing a new config to be generated.

My next complaint, which is pretty critical, is that I can't install jEveAssets from the Nix repository because it doesn't exist. If I'm going to go the Nix route, I want everything installable through the config. This one is almost a deal breaker, but I can get around it fairly easily since jEveAssets is a Java app—I don't have to patch its binaries for Nix. Like I said, almost a deal breaker.

The next complaint IS a deal breaker. I game on my laptops; I don't always do "work," so being able to connect my gaming peripherals is a must. I can't, for the life of me, get my Xbox controller to pair. Nix sees it, attempts to pair with it, and then, right before the pairing is successful, the Xbox controller shuts off. This one is absolutely a deal breaker for me. I don't want to be limited in any way on my OS, except for the ways I'm willing to be limited—and really, anticheat games not working is something I can deal with.

Now for the pros—the things that would have kept me around if the OS had just been a bit closer to being the one for me. Configuring my shell was easy. It would be great to reinstall and, boom, have my shell fully configured the way I like it. This was nice. Fastfetch as part of my shell boot was also easy to set up, but configuring Fastfetch with my preferred settings was a bit tricky. It's nice, though, because the way you import it makes Nix aware of the data in a mergeable way—not that I need Fastfetch configs merged with anything. The one thing it wouldn't do easily (using Home Manager, at least) was make Fish my login shell. I had to do some magic with Bash to have it launch Fish on startup for me.

I kind of like the simplicity of "snapshots." Each generation is bootable, so if you build a failed generation, you can just boot into the previous one, see why things broke, and rebuild. Simple as that, so the need for Btrfs isn't there.

Now to decide what to do with my second hard drive, because Nix isn't for me.