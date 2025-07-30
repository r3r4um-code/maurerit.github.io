---
title: "Reading My Old Computers Harddrive"
date: 2025-08-16T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

My old computer became my sons computer a couple of years ago.  I never did back that thing up and then finally last year something happened to it that I haven't diagnosed yet that keeps it from posting.  I know for a fact I have some digital photos of my son in the hospital the day after he was born and I want those pictures.  One was my wallpaper for awhile and I want it back.  I really need to read that things drive.

I just got this new computer and it has an extra m.2 slot, which is perfect.  The other laptop had one but I needed a special bracket to mount it so I never went through this on that laptop.  So I extract the part from the old computer and plug it into the laptop and fire it up.  I look at my available disks and nothing new.  So I go into my UEFI setup to see if the BIOS even see's a drive and nothing.  It's hard getting down and dirty tech specs on these damn windows laptops since they're not ACTUALLY built to be upgradable so I don't know if this thing can actually handle M+B Keyed ssd drives.  That plan is ruined.

So I go on a discord I frequent with some Linux nerds who line EVE Online and I bitch and complain about the situation.  I go on to say that I'll have to find some other way of reading this thing.  Then someone chimes in and suggests and external enclosure so I hit up Amazon and start searching.

I considered price, features, the way it looked and if it would read a M+B key which they pretty much all said they did so the choices mainly came down to price.  So I order one because none of the brands ring a bell to me, they're all small brands that probably no one has ever heard of.  So I wait for the thing to come in the mail and when it arrives I eagerly grab my drive and plug it in and see about reading it.  Once I have it plugged into the computer I list my block devices `lsblk` and see some /dev/sda device that I've never had before but it doesn't list a size.  Great, so either that drive is dead or this reader doesn't work with this key.  I'm not sure where to go from here except to someone's house that has an extra m.2 slot that is for sure built for this drive.

I've ordered yet another one, one that actually has a picture of the devices that shows an M+B key drive, the one I ordered showed no pictures of this key so who knows, maybe I'll get lucky this time.  If not, I'm returning it, I'm not keeping another product that doesn't work for me.