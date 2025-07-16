---
title: "Weird Issues in Cachy"
date: 2025-07-16T11:07:02-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Recently I had an issue crop up in my CachyOS install where I would crash hard out of EVE Online. A Windows 98-looking dialog would pop up with the text `!status && vkAllocMemory` and then a line of code in a file (as if that codebase means anything to me, lol). I couldn't fire EVE back up after that—I would have to reboot. I didn't dive into whether there were stray processes holding something up, but things were REALLY slow if I restarted EVE. Like 12fps sitting in station. Not acceptable.

I tolerated this and troubleshot it for a couple of days. I downgraded to the LTS kernel and all was well for 2 days, and then out of nowhere the crash manifested itself again. I went back into Debian and played for a while—I wanted to see if the crash would happen there. I spent a couple of days in Debian and nothing. Not a hiccup (outside of the usual crashes, which aren't quite as hard).

I'm now in a fresh install of CachyOS... I keep typing "Crachy" because I find it funny. I've been in-game for a total of about 8 hours now, which is far more time than I would have had at the end of my last install's life. It was consistently crashing within half an hour. I've got at least the majority of my customizations in place and I'm ready to roll. I'll keep these pages updated.