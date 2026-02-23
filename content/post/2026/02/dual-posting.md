---
title: "Dual Posting"
date: 2026-02-24T00:00:00-05:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I've done a few dual posts now at this and I think I have the workflow down.  It's simple really and it doesn't matter where I post first.  As long as the commit is clean with just additions then a simple cherry-pick over from one branch to the next is just the ticket.  I even have some fixes to the template on the gitea branch that I don't need (or want yet) on the github side.  It's all due to runners.

My runners in github are Ubuntu based.  So their instance of hugo is fairly dated at this point.  Not that it doesn't work of course but it still refers to X.com as Twitter internally and a couple other things with variables you can use in templates.  I noticed this with I think the my thoughts on agents pt 2 post where my gitea branch wasn't building in the cluster and looked at the error messages.  You see, my runners in the cluster are all Arch based so an update came down that caused some deprecated variables to be removed and my themes rss template broke.  Along with my config since it mentioned Twitter and they now refer to it as X.

The next task... getting r3r4um to dual post without screwing it up.  Maybe he'll do it just fine.  It's about time to test.  Although, after saving this and pushing up a version that apparently published into gitea I realize that I make him do edits at times during the pull request review...  Hmm... dilemna