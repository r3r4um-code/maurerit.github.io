---
title: "Network Time Protocol and Security"
date: 2025-05-10T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I was recently bit by an out-of-sync clock. I don't know all the details, but I do know that it's time-based... the one-time passwords that 2FA brings with it. I didn't realize I was so far out of sync that it would matter...

You see, I recently switched back to Linux again after years on a Mac and then mostly on Windows. My old PC had Linux on it for a while until I finally blew the partition away and expanded the Windows partition to consume that space again. Something I did to GRUB killed the booting, lol. I didn't feel like fixing it.

So, after the switch, I started logging into things as I usually do, and then it came to JetBrains... I couldn't pass the 2FA... I thought that was very odd, but I let it go. No need to worry about it; my card information with them is on file and I'm logged in everywhere it matters. I won't futz with it until it's a problem.

I can't remember how it became a problem, but it did, or I got a wild hair up my behind and decided to dig into it. I first opened a ticket requesting to have my 2FA removed. I would rather have access to my account than not because 2FA is somehow borked. I thought for sure it was on their end...

After getting my account back and setting up my credit card payments again, I opened a new ticket requesting any help with the 2FA they could offer. I could use 2FA with the same tool on various other sites, so why wasn't JetBrains working? Surely it couldn't be on my end? They responded with some steps to ensure that my machine was time synced. I let it go for a week or so; I didn't feel like futzing with it.

Then the problem started happening with EVE... Oh hell no, you can't mess with my EVE time. So I dug around my system and Google to see if I was syncing with a time server. Sure enough, I wasn't set up with syncing from the get-go. I thought Debian did that automatically. I found [this](https://phoenixnap.com/kb/debian-time-sync) article that spelled out a couple of ways to set up syncing with NTP. I chose the systemd approach this time.

Now, with that done, I tried to set up 2FA with JetBrains and it worked! I haven't tried EVE yet, but I had a workaround for that... I could still 2FA within the first few seconds of a new token being generated. But that threshold felt like it was getting smaller and smaller, so I had to do something.

Always assume it's your fault when dealing with a service that handles many more requests than just yours.