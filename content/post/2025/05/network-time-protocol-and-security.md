---
title: "Network Time Protocol and Security"
date: 2025-05-10T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I was recently bit by an out of sync clock.  I don't now all the details but I do know that it's time based... the one time passwords that 2fa brings with it.  I didn't realize I was so far out of sync that it would matter...

You see, I recently switched back to Linux again after years on a Mac and then mostly on Windows.  My old PC had Linux on it for awhile until I finally blew the partition away and expanded the windows partition to consume that space again.  Something I did to grub killed the booting, lol.  I didn't feel like fixing it.

So, after the switch I started logging into things as I usually do and then it came to Jetbrains... I couldn't pass the 2fa...  I thought that was very odd but I let it go.  No need to worry about it, my card information with them is on file and I'm logged in everywhere it matters.  I won't futz with it until it's a problem.

I can't remember how it became a problem but it did or I got a wild hair up my behind and decided to dig into it.  I first opened a ticket requesting to have my 2fa removed.  I would rather have access to my account than not because 2fa is somehow borked.  I thought for sure it was on their end...

After getting my account back and setting up my credit card payments again I open a new ticket requesting any help with the 2fa they can offer.  I could 2fa with the same tool on various other sites, why was Jetbrains not working.  Surely it's not on my end?  They responded with some steps to ensure that my machine was time synced.  I let it go for a week or so, I didn't feel like futzing with it.

Then the problem started happening with EVE...  Oh hell no, you can't mess with my EVE time.  So I dug around my system and google to see if I was syncing with a time server.  Sure enough I wasn't setup with syncing from the get go.  I thought Debian did that.  So I found [this](https://phoenixnap.com/kb/debian-time-sync) article that spelled out a couple of ways to setup syncing with ntp.  I chose the systemd approach this time.

Now, with that done, i tried to setup 2fa with Jetbrains and it worked.  I haven't tried EVE yet but I had a work around for that... I could still 2fa within the first few seconds of a new token being generated.  But that threshold felt like it was getting smaller and smaller so I had to do something.

Always assume it's your fault when dealing with a service that handles many more requests than just yours.