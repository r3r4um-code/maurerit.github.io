---
title: "Vertical Integration in Testing"
date: 2025-06-14T09:46:09-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I found a place to perform reactions near Amarr (my trade hub of choice). I don't know how long this structure has been here, and the corp is a 5-member corp who I have no idea about, so there is the potential for some shady operations since it's so close to a trade hub. I'm not concerned about that right now—what I am concerned about is testing and validating this strategy. Both from a code perspective and from a cost perspective in-game. Mainly for the code perspective.

You see, the app doesn't currently know how to do anything with the build hierarchy—it just knows inputs and outputs. So in a sense it DOES know about the hierarchy because it can indeed perform the correct operations for intermediate and composite reactions. I've run one batch of composites, and everything ran through the app just fine. From purchase to the intermediate output itself, and I am now sitting at the correct raw material counts.

The code wasn't that hard, and in fact I had JetBrains Junie do it for me. Still testing out that AI as it's both intriguing and scary at the same time. I fear that it will disrupt my industry—not put us all out of jobs, but start to make jobs without the use of AI few and far between. So I might as well embrace the future and learn how to prompt. I'm not a fast vibe coder; I'm very explicit in my prompts and could probably write the code in the same amount of time as the LLM, lol. But what I can't do while writing the code myself is run off and do something completely unrelated.

While messing with this, I'm also applying some small tweaks to the UI. Of course, having AI refactor these things sometimes takes a couple of prompts, but it gets the job done, lol.

To more and more features managed by AI!