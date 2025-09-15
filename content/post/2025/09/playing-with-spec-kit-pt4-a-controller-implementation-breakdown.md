---
title: "Playing With Spec Kit Part 4 a Controller Implementation Breakdown"
date: 2025-09-15T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

The first controller I implemented was the AuthController.  I knew this step was going to be rough but damn did I underestimate the exhaustion.  The AI kept trying to mock services instead of the underlying mechanics that made the services behavior fail in the first place.  I don't know where it picked that up from but it did that several times.  Once the AI figured out that we needed to come up with better test tokens things went much smoother.  But getting there... but was getting there a journey.

There were several false positives, at least the AI thought it was done, I knew better, lol.  I can see that 3 prompts in at: 097_t040_verification_and_audit the AI thought things were passing.  I can't remember exactly what happened around that time besides the RestTemplate to RestClient refactoring.  I should have maybe had it refactor the RestClient calls into the AuthClient around this time because mocking the RestClient calls tripped the AI up quite a bit.  It would trip me up as well and I'm glad we finally moved into a position where more of the code can be tested and appropriate small area's are mocked.

During the implementation of the DrawController the AI started to get tripped up with writing a file.  I dont' know why it does this but occassionally it can't write a file or something and it get's corrupted.  So the AI spends time trying to here doc it, delete it on the command line and recreate it magically (which never happens) but this time it gave up.  But... while it was at it... it deleted it's work, haha:

![Oh No](../playing-with-spec-kit-pt4-oh-no.png "Oh No!")

This exercise has been exhausting, I'm not really feeling accomplished.  I'm feeling more frustrated than anything.  While I feel like there is forward progress, I don't know the code base, I didn't spend enough time defining what I wanted my code to look like so now it looks like something I don't want to look at.  I'll be playing with the checkstyle format and hopefully defining my format and then reformatting the code because damn is it ugly right now.  I can't do 2 spaces... too compact.  Four is just right.

At this point we're caught up to where I am now.  I didn't work on it much last night because I was mentally exhausted and I didn't much work on it today either as I was visiting family.  More work to come this week as I find time.
