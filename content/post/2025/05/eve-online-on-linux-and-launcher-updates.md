---
title: "Eve Online on Linux and Launcher Updates"
date: 2025-05-09T01:30:49-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I've talked about EVE Online and what it's done for and to my career.  It's a game I spend probably far too much time in, but generating data that is both meaningful and fun is just so addictive.  It's part of why I talk about it so much.

The EVE Launcher, now that's a piece of software.  For what should be a simple piece of software used to launch another piece of software, this one sure has a lot of bells and whistles.  Who needs a launcher that requires hardware acceleration?  Is it an electron app or a .NET app?  If it's both... why?  Why does it need almost a gig of memory to launch other applications?  I thought java was bad...

Now for the troubles that I've had... Just today a launcher update was deployed. These always unnerve me because the launcher is the only thing that can get you into EVE. I get a bit nervous, think it through and decide... who cares? If it breaks, then it gives me something to do when idle. And sure enough... it broke.

Like I said previously, I don't spend much time in the C/C++ world, so my skills in the lower level are kind of garbage. I'd love to be able to help out with this troubleshooting because it seems like NO ONE knows what is going on. I spent some time trying to learn how to use the WINEDEBUG environment variable so MAYBE I can try and help. So far I've been able to get rid of all the fixme messages, but I'm not sure what to turn on to get crash reports or whatever might happen. I'll keep experimenting.

It's always fun dissecting someone else's software... /sarcasm