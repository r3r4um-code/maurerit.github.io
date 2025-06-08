---
title: "Bleeding Edge Just Cut Me a Bit"
date: 2025-06-11T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

I'll preface this post with: I haven't spent much time looking for alternate jdk packages in CachyOS.

Since I've been running Cachy I've only been compiling and running my new hobby through the ide... which, now that I think about it, had jdk 24 also... and everything has been going well.  Lombok has processed all the classes it needs to process, adding getters, setters, constructors, etc.  The app has been executing just fine and generally I've had no problems coding over there.  Until I went to compile on the command line... I was persented with ALL KINDS of 'cannot find symbol' errors and I'm like... wtf.  Why now?

So I dig in and get on the search engine.  I'd say google but i don't use google except for email... and logging in the various things... so I have no privacy, lol.  Anyways... my search leads me to the lombok issue's where someone is requesting that lombok support jdk 24 as it should be finalized and ready to go but others argue that there is still time for the java team to break things... then it could be back to the drawing board for lombok.  So they're waiting.

While this is fine, it could catch a more junior dev off guard and prevent them from really getting through it.  In the errors there was no hint that it was lombok that was the issue and if the junior is just capy pasta'ing things around and doesn't fully understand how lombok works... they may search for "cannot find symbol: getFoo" and get nothing or be buried in NoClassDefFoundError questions and be completely perplexed by why they would be having that issue.

I knew instantly what the issue was and while I should have just gone to lombok's github, I chose to search instead... that'll get me to the correct issue faster anyways.  I didn't spend much time reading through that post but if I had I would have seen at the end there is a link to a new PR completing the jdk24 upgrade...  Now I just have to override springs version until the spring-boot parent matures and gets ready for jdk24.  This will be fun watching how this plays our, I've always been slightly curious how Spring choose which jdk's to support and I don't know how fast they support the latest and greatest.

Spring Boot moves fast though, fast for java at least.  It's not react fast, or node fast but for what it does it moves seriously fast.  Let's see what happens next.