---
title: "SearXNG and Ollama"
date: 2025-11-08T00:00:00-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I love to goof around with these generative AI's.  Finding their issue's, watching them 'think' and then also finding where they're useful.  What I don't want to do though is be tracked and used as input for the models later on.  Not sure how useful that is and there are also these pages which I'm sure are being snarfed... but I digress.  Let's play with some tech anyways because I have computing power at hand that I'm not using.  Sure it'll elevate my power consumption but not by much, a few bucks now that the AC is off for the year.

I love that I can run a local LLM but quite frankly, without the ability to hit the internet they're only so useful.  So, I went searching and found a few things talking about a python script that gives you a cli into an Ollama model and then some logic of it's own to detect when you want to snarf the web.  That wasn't quite what I wanted.  I think I want something more like a search interface with an AI summary at the top with sources cited and linked.  So I kept searching.  After some more digging using SearXNG, I ran across some Medium articles talking about Perplexity.  Intrigued I dug in.

Perplexity seems like a nice service with the abstract information I have about their architecture.  Seems reasonable that they could anonymize the requests to the search engines and also to OpenAI and provide me with some decent stuff.  I want this locally though so I can see how useful these open source models are.  Plus I do want what I look at to be private as much as possible out of principle.  Don't be watching what I'm not willing to share :P.

After some time I ran into [Perplexica](https://github.com/ItzCrazyKns/Perplexica) through another article on Medium.  The article also mentioned something called Terminus but that project seems defunct now. So I dug into Perplexica looking through their readme I see they provide a docker immage.  This is alright, I do tend to use docker for various things so let's do it this way.  After some twiddling with the initial setup getting Ollama added I was off to the races and asking questions.

I really want to dive in and figure out exactly how this app is functioning.  Like, is it firing up a model to ask it about what it should search for?  Then performing a search and providing that to a different model?  I've watched my GPU usage and it seems like this is the case but I can't be for sure.  I need to dig into the code.  I might do that later, maybe more to come :D.  I've done a few things like searched for myself by providing a little bit of context on me.  It crosslinked me somehow with a murder in the general area where I live.  I wasn't mentioned in the main article it referenced so I'm unsure how it made this connection.

Oh, speaking of references, the way Perplexica does these is that they are little annotations in the text, numbered subscript links and then they're all lined up at the bottom as well.  But you can see in the summary where all of the references come from.  Quite a nice little feature.

My current chat is revolving around an article about the Trump Administrations Intel investment.  I then followed some 'related' links which led me to a graph that showed market cap comparisons historically for AMD, Nvidia and Intel and I noticed a jump recently in AMD.  So far I'm finding this app useful.  Good stuff

That's it for now.