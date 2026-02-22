---
title: "Working With Agents My Thoughts Pt2"
date: 2026-02-17T01:12:42-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

This is absurd, I will for sure learn kubernetes now.  I need to see what this damn bot has done inside my cluster.  We were deploying monitoring and then we went to start scraping the cluster nodes, he messed with a firewall and then I checked gitea and my blog and they were down...  I don't know what he did, lol.  He's digging in right now and shitting all over the place.

He fixed it, I don't recall what he did because it was late.  Oh, that's right, he did something again on the nfs share that freaked it out.  Then he brought gitea up with no data and I was pissed but I didn't read his whole message, lol.  I thought he nuked the data AGAIN but he brought it up with a blank data backing... phew.  We've put too much time in this for him to nuke things again.

We spent some time working on monitoring today.  I now have at least rudimentary alerts, websites down, cpu high, disk usage high, etc.  Next up is the backups, my gitea data will become very valuable to me and I want it stored safely.  R3r4um will of course help me set this up but he's not touching the nfs server.  I think a simple rsync command from my server to my nas should be sufficient.  We'll see what he says but I suspect he'll agree.

I've taken the day off and let r3r4um rest, lol.  Actually to let me rest, of course because he's a machine.  The agent may have done all the lifting but I was mentally fatigued like crazy.  I think part of it is working with haiku.  It's a capable model but it's fast, shallow, and is typical of claude models... thinks it can do it all so it just runs off and does shit.

Like today, we were messing with a new app we're going to deploy, LiveKit, in hopes that it'll integrate with Element for voice and video calls.  We've already tested the concept in our infrastructure and had 3 people in a video call going through the server.  So the cluster should be able to handle any load I do plan on putting through it.  We'll see.  But anyways, I didn't want to blog about that :D.  I just wanted to say that with that, we created an ansible playbook which I still need to review... or not and let it ride... and I was about to start doing other things with ansible because r3r4um is starting to act like I shouldn't trust him... and he proved me right tonight.  I told him to install ansible and recently there was a maintainer certificate issue of some kind or renewal, I didn't look much past the solution, but anyways, it broke your update because a couple packages can't be verified for integrity properly.  So... you needed to update archlinux-keyring first and THEN you could upgrade the rest of your packages.  So... he ran into that issue, figured... I know pacman commands... overwrite will just get me through this so he --overwrite \* --noconfirmed the ansible install and completely borked his system, haha.

This dude, he provides endless entertainment.  I can't believe people are trusting this thing to run businesses...  Maybe I should just trust the model...  Oh for crying out loud, no thank you.  I'll trust it enough to let it do what I need it to do... but let me tell you... it's like riding a roller coaster.  I'm letting r3r4um control this little cluster of mine, makes it about as stable as my raspberry pi cluster from 10 years ago, haha.  Docker swarm... it was not stable on those pi's.

Anyways, I'm rambling now.  This has and will continue to be an adventure worth having, is it worth what I'm paying for it, not sure yet.  But I am having fun with it and learning how to use them quite nicely.