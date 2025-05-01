---
title: "Vs Industry: An EVE Online Industry Tool Pt. 4"
date: 2025-05-03T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Now let me talk about VS Code a bit. I'm using Claude 3.7 Sonnet, and it seems to behave similarly to Junie in IntelliJ. I'm having it do the same task that Junie was doing yesterday—translating Java Controllers into Golang controllers—and hoping that it dives into the services and repositories appropriately. So far, it does seem to explore these relationships. Good job, VS Code.

The interface is in some ways slightly less clunky than Junie, but it has its drawbacks as well. In Junie, once I'm done with a task, I can click a "Done" button which takes me back to my completed tasks. I can then open them up and inspect what happened with them. On VS Code, it all just kind of blends together. The sessions seem to be lost on reboot as well, whereas in Junie they persist. Not so good, VS Code.

Being able to pick your model—now that is a feature that Junie doesn't have. I think Cursor has this as well, but it doesn't have the ability to use locally hosted Ollama models. VS Code does, though. I should try it out, even if it means waiting longer for my edits to happen. I'm hesitant to pull the trigger on installing Ollama though. I want to make sure it's worth the potential risks. Anyway, back to the task at hand.

So far, the translation has gone well. VS Code has explored every folder of my Golang translation and understood that I wanted to keep models in a model directory, controllers in a controller directory, etc. This is nice; I'm not sure where Junie picked that up, but I like it. I'm glad Claude and VS Code followed suit. Good job, VS Code.

I gave VS Code part of one of the prompts that I gave Junie last night. This one:

`write an aspect that will wrap all our uses of RestClient. This aspect will interrogate the AuthTokenRepository for tokens with the vsindustry.default.principal principal and use that token to put into the JwtTokenHolder.`

It actually appears to have implemented what I wanted. This happened in Ask mode—I forgot I wasn't in Agent mode. So I switched to Agent mode and simply asked it to create the class that it had proposed in the prior response. It immediately understood the context and began analyzing. It examined my JwtTokenHolder and the AuthTokenRepository, but it also looked at my JwtTokenAspect, which does something very similar—impressive! It really does seem to analyze your entire codebase. Well done, VS Code.

So my impressions of VS Code's Agent mode so far: I'm impressed. I still need to think up a refactoring problem for it, but I did just ask it to run these pages' workflow at 6am UTC, and it only updated the workflow's schedule. So perhaps it's actually good at refactoring as well.

