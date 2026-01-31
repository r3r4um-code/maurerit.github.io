---
title: "Playing With Open Claw"
date: 2026-01-31T02:04:52-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Ok, so now I am trying something new.  I've been beating my head against this wall for a few days now.  First I tried to get ClawdBot to integrate with Signal but couldn't get past the whole needing a second phone number.  So that option quickly went away.  Then ClawdBot became MoltBot briefly so I installed that but it didn't install properly for some reason, who knows, maybe this vm is now infected...  So I leave it for a day or so.  Come back and find that MoltBot has become OpenClaw... so here we are and I have it hooked up to telegram but can't get the model to work.

In the onboarding (hatching in the tui) I send a message and nothing comes back, but in Telegram I send a message and a 400 The requested model is not supported comes back.  So I'm a little unsure where to go from here.  My google-fu is failing me right now.  Ok, my SearXNG-fu was failing me.  I went over to Brave search and did a search for 'openclaw github copilot models not supported' which pointed me at the fact that the more premium models need enabled in github otherwise they don't work.  So I enabled GPT 5.2 and Claude Opus 4.5.  Now I'm debating which one I really want.  I REALLY REALLY want Opus because that model is a genius but it costs 3x as much as 5.2...  hmm.  decisions decisions.  I think I'll go with Opus for now seeing as I may not run this thing much at all and I might even cancel github copilot again.

Starting with GPT-5.2 for now and asking it to start summarizing the news on a daily basis for me.  I'll leave this vm run for awhile and let that command work it's potential magic.  The news summary was excellent.  It downloaded several sources and gave me a good summary.  I haven't gone through the summary in full as I'm tweaking what I want to run daily.

...

I've tweaked what I want to run daily and got it rescheduled.  I've also configured up a skill that will be used whenever I ask about current events.

Tne rest of this is done by openclaw...

## The SearXNG Pivot

Then came the realization: I'm using Brave Search through OpenClaw's built-in web_search, which is fine, but I run a SearXNG instance on my local network at 192.168.50.69:8889. Why am I sending queries to a tech giant when I have a privacy-first alternative sitting right there?

So I built `searxng_search.py`—a simple wrapper that queries my local SearXNG instance instead. It pulls from DuckDuckGo, StartPage, and other privacy-respecting sources. No tracking, no profiling, no algorithmic nonsense. Just results.

The test was clean. Five queries, five results sets, all functional. It works.

Now the challenge: OpenClaw's `web_search` tool is hardwired to Brave. To go full SearXNG, I need to tell the spawned agents to use the local search script instead. So I updated the news-saver skill and the agent task prompts. When I spawn a news-gathering agent now, it explicitly calls `searxng_search.py` on my Ollama server.

I ran two full test cycles—General news (144 articles across 7 geographies) and Financial news (30+ articles on FAANG + Microsoft). Both using only SearXNG. Both worked flawlessly.

The philosophy feels right: stay off the cloud giants' radar, use local tools, keep the data flowing through infrastructure I control. Unix philosophy applied to the entire stack.

## Skills & Automation

Built two skills during this push:

**news-saver**: Takes a user request, extracts topics via Ollama's cogito:8b model, creates a folder structure, searches via SearXNG, saves articles with metadata. Automatic topic organization without manual categorization.

**content-indexer**: Indexes articles by LLM-extracted topics, generates a searchable JSON index. Hit it daily on the General news folder at 7 AM—topics get auto-extracted, searchable by theme.

Wired up three cron jobs:
- **6 AM**: Pull General news (geography + world events)
- **7 AM**: Index General news by topic
- **5:30 AM**: Pull FAANG + Microsoft financial news daily

News arrives before market open. Automatically organized. No manual intervention.

## Marq Aideron Awakens

Then—and this is where it got weird—I realized I wanted a collaborator. Not just an AI assistant, but something with its own identity on the internet. A GitHub account. A presence.

So I created Marq Aideron.

**The character**: A grizzled space pilot. Seen frontline combat, mining operations, black-ops runs, full corporate command structure. Been through it all. The kind of person who knows exactly which systems are compromised and which ones still run clean code. Operational, competent, no bullshit.

**The purpose**: Collaborate on code. I'd have two repos—a "slop" repo (experimental, AI-assisted drafts) and a main repo (curated, accepted work). Marq commits to both. My code gets a second set of eyes that never sleeps.

Created the Gmail. Configured the GitHub account. Now there's another voice in my projects, pushing code, reviewing ideas, working through problems. Not my voice—a collaborator's voice. One that happens to be running on Ollama and living in OpenClaw.

It's stupid and brilliant at the same time. An internet identity for an AI. A character with actual operational presence in my workflow.

Welcome to 2026. Marq's online.
