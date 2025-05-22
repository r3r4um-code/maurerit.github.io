---
title: "Nodejs and Libraries That Restrict"
date: 2025-05-22T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

I just recently grabbed my frontend code on my work laptop and tried to npm install.  I can't remember exactly what version of node I had on there, it's been about 2 years since I upgraded so it was old and crusty.  I expected to get an error from a react component which I did.  So I grab the version they say they want and then another library complains.  This led me to just make sure I had the latest LTS installed.  This resolved the issue.

This got me thinking - I've been a java dev for roughly 18 years at this point and I've never had to deal with this kind of a moving target.  We just pinned to versions though so we may or may not have allowed security holes in our apps.  These were internal apps though so they wouldn't be exposed to the raw web and therefore not AS exposed to all the threats.  All in all though, todays way of doing things is more than likely more secure.

There is a difference in ecosystems though.  Take my little project as an example.  In the frontend, I've already dealt with 46 PR's.  A good portion of which were dependabot updates.  Some of which required intervention such as upgrading node.  So I started at around node 19 and now I have node 24 installed and things are happy.  Think about that though... in the span of a couple of weeks most of those PR's appeared.  Where as on my backend project I've had a total of about 16 PR's.  Most of which are my own PR's with only a few being dependabot updates.

With the update frequency of the frontend you can't help but think that things are so much more unstable out there.  They're changing things so fast that it really is hard to keep up.  What causes this to happen?  Surely it can't all be security updates?  Do they really have THAT many features they want to implement?  Are they trying to include the kitchen sink?  I don't fully get it, do they NEED all these updates?

Just some random thought for the day.