---
title: "Agents at Work"
date: 2026-03-04T06:48:00-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I have access to GitHub Copilot at work but in a much more limited capacity.  Crown provided this for us for the fancy autocomplete because at the time there was no Copilot chat in the IDE, it was just the embedded autocompleter.  But now we have access to agentic work through the Copilot plugin in our favorite IDE.  The problem though... we work on cutting edge tech and have our own abstraction layers in libraries we develop and import.  So, while the LLM might see our imports from time to time it will never be in the same context as the projects that use them so limits the LLM's usefulness.

Like, take for instance yesterday.  I wanted to use up some of my credits so I decided to see if Claude Opus could migrate a WebClient usage to a RestClient usage and then write the tests.  I explicitly told it to use @RestClientTest and as it was generating code... the multiple generations it generated... one of them had the @RestClientTest annotation and was setup right... but then the LLM thought about it... scanned the pom looking for spring-boot-parent:4.0 or something and saw our Crown Parent... so it couldn't verify that we were in a spring boot 4 context so it backed out what it did and then went with a Mockito approach... Mocking a fluent API is miserable and the result is miserable looking...

I want to use up the time they've spent money on otherwise I feel like Crown isn't getting their moneys worth but I honestly A) Don't trust these agents enough and B) Find them too limited in this context to be fully helpful.  It already takes months for these models to come out with recent knowledge so working with cutting edge tools is painful because the LLM hasn't memorized usages of the library.  And I'm not about to have an LLM generate code to manage low level sockets on http exchanges... and connections to databases... and all the plumbing code that spring and our abstractions provide... No thank you, they're already limited enough... I don't want to overload their context and make them useless.

I'll keep finding some uses for them at work to use up credits but I doubt I'll even chew through the 300 credits I get.  I actually care about the code base I work in.  My personal side projects... I care... but I care about getting the thing written more because I want it's functionality... I explore new things at work... practice them or use them at home.