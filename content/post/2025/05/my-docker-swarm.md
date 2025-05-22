---
title: "My Docker Swarm"
date: 2025-05-05T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Sometime in early 2017 I began the search for a new job.  I knew I wanted to play with some of the newness that was out there.  Ideally they included kubernetes or docker-swarm or something similar.  So I searched around and ran across people running docker swarm on their raspberry pi's.  This is perfect, I have like 5 of these things.  I can showcase some projects.

So I sat down to figure out the best way to power these things.  I had an old computer, some core 2 duo machine from my late Grandmother in law.  I could salvage it's power supply, we're not expecting much power draw.  If my memory serves me correct at peak load we really should only be pulling 60w, I'm sure this power supply is rated for 250w.  This will be the core.

Now I had to figure out exactly how these things work.  I can't remember the exact details now but I think you had to provide 5v or something on or tie something to ground to get the powersupply to stay on.  This is either some kind of power saver mode or safety feature, unsure.  This was something that a physical toggle switch could solve so I wire that up.  Then I built a board with some very crude solder 'traces' that tied some of the power lines of the power supply to usb cables that would power the raspberry pis.  They all powered on with the toggle of a switch, perfect.

My next task was to get internet access and docker swarm setup. I either had a spare switch or ordered a cheap one from Micro Center or Amazon and plugged the raspberry pi's network connections to those ports. Then I connected the switch directly to my router. Internet access achieved. Proceeding to Docker Swarm.

This setup is rather simple if I recall correctly. You initialize the master node with a single command, which provides the command needed for all worker nodes. Easy peasy, but now you have to figure out how to configure and use the thing. This is something I'd have to do more research on again, but if I recall correctly you can use a docker compose file or something very similar to deploy your containers. I don't recall it being hard.

While the initial setup was straightforward, the real challenge lies in configuring and optimizing the swarm for specific projects. This foundation, however, sets the stage for experimenting with containerized applications, automating tasks, and exploring the potential of Docker Swarm in a more practical, real-world context.  I didn't dive too deep down this rabbit hole.  I just wanted it running.

I remember playing around with OpenFaaS which was just being born at that time so was free to run without any restrictions.  It was cool to be able to write little bits of code that run in a short lived context
