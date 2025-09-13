---
title: "Playing With Spec Kit Pt3"
date: 2025-09-14T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Random thoughts while interacting with the AI:

Yesterday I said that I didn't know how the AI was going to get around the whole default spring oauth flow... I should have put the urls together better that it was pointing at a custom auth controller.  We'll see how it handles the flow and if it's a better model than springs default flow.  But yet not because I see in the yaml that the oauth flow is the default spring flow... curiouser and curiouser.

I see a property I've never used before.  The `jwt.secret` property I've never used before and I'm curious how I get my own jwt secret key.  Maybe it's just as [simple as this](https://dev.to/tkirwa/generate-a-random-jwt-secret-key-39j4)?

Today was not a successful day.  I have had to reprompt now like 6 times for tasks T027-T031.  I've tried them all as a group, individually, paired... it doesn't matter... it will fail indeterministically...  Like for instance, I just did T031 twice... yes... that simple task took 2 tries.

I'm approaching backend controller implementations... I'm so excited to see this model either succeed or fail miserably.  I can't wait.

As expected it's spinning hard while implementing controllers.  I'm having it implement one controller at a time and it is taking many prompts to get through this thing.  Like... I'm having to reprompt a lot but this time I'm having it continue where it left off.  Seems to be... OK... but I think the whole buffer issue is getting in the way.

I'm happy to be using my CoPilot subscription more and I'm partially having fun.  I really don't understand how people are having AI Agents run off and edit things for them over night, I'm having a hard time when an AI is kept on strict guard rails.

Starting to get frustrating.  Even with all these guardrails it's still running off track.

After much hassle I got it to work, if you're following along the interactions were from commit hash 0c8b5bd4c0dd - 167ef6d2ac and the memory files are 094 - 104

That was all just to get the AuthController working.  The UserController submitted to the AI's will much easier.

I'm too exhausted to compose this into a proper blog post so all you get are these disconnected thoughts.  Have fun.