---
title: "Bleeding Edge Just Cut Me a Bit"
date: 2025-06-11T00:00:00-04:00
author: "Matthew Maurer [maurerit](https://github.com/maurerit)"
draft: false
---

I'll preface this post with: I haven't spent much time looking for alternate JDK packages in CachyOS.

Since I've been running CachyOS, I've only been compiling and running my new hobby project through the IDE—which, now that I think about it, had JDK 24 also—and everything has been going well. Lombok has processed all the classes it needs to process, adding getters, setters, constructors, etc. The app has been executing just fine, and generally, I've had no problems coding there. Until I went to compile on the command line... I was presented with ALL KINDS of "cannot find symbol" errors, and I thought, "What the heck. Why now?"

So I dug in and consulted my search engine. I'd say Google, but I don't use Google except for email and logging into various services... so I have no privacy, lol. Anyway, my search led me to the Lombok issues where someone was requesting that Lombok support JDK 24 as it should be finalized and ready to go. Others argued that there is still time for the Java team to break things, which could send Lombok back to the drawing board. So they're waiting.

While this is fine, it could catch a more junior developer off guard and prevent them from really getting through it. In the errors, there was no hint that Lombok was the issue. If a junior developer is just copy-pasting things around and doesn't fully understand how Lombok works, they may search for "cannot find symbol: getFoo" and get nothing or be buried in NoClassDefFoundError questions, completely perplexed by why they would be having this issue.

I knew instantly what the issue was, and while I should have just gone to Lombok's GitHub repository, I chose to search instead—that would get me to the correct issue faster anyway. I didn't spend much time reading through that post, but if I had, I would have seen at the end there's a link to a new PR completing the JDK 24 upgrade. Now I just have to override Spring's version until the Spring Boot parent matures and gets ready for JDK 24.

Part of the fix is also to add a new configuration to the maven-compiler-plugin.  Do like [this](https://github.com/projectlombok/lombok/issues/3772#issuecomment-2763364839) comment points out

This will be fun watching how this plays out; I've always been slightly curious how Spring chooses which JDKs to support and how quickly they adopt the latest and greatest. Spring Boot moves fast though—fast for Java, at least. It's not React fast or Node.js fast, but for what it does, it moves seriously fast. Let's see what happens next.