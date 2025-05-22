---
title: "Node.js and Libraries That Restrict"
date: 2025-05-22T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I just recently grabbed my frontend code on my work laptop and tried to run `npm install`. I can't remember exactly what version of Node.js I had on there—it's been about 2 years since I upgraded, so it was old and crusty. I expected to get an error from a React component, which I did. So I grabbed the version they said they wanted, and then another library complained. This led me to just make sure I had the latest LTS installed. This resolved the issue.

This got me thinking—I've been a Java dev for roughly 18 years at this point, and I've never had to deal with this kind of a moving target. We just pinned to versions, though, so we may or may not have allowed security holes in our apps. These were internal apps, though, so they wouldn't be exposed to the raw web and therefore not AS exposed to all the threats. All in all, though, today's way of doing things is more than likely more secure.

There is a difference in ecosystems, though. Take my little project as an example. In the frontend, I've already dealt with 46 PRs. A good portion of these were Dependabot updates, some of which required intervention such as upgrading Node.js. I started at around Node.js 19, and now I have Node.js 24 installed and things are happy. Think about that, though... in the span of a couple of weeks, most of those PRs appeared. Whereas on my backend project, I've had a total of about 16 PRs. Most of these are my own PRs with only a few being Dependabot updates.

With the update frequency of the frontend, you can't help but think that things are so much more unstable out there. They're changing things so fast that it really is hard to keep up. What causes this to happen? Surely it can't all be security updates? Do they really have THAT many features they want to implement? Are they trying to include the kitchen sink? I don't fully get it—do they NEED all these updates?

Just some random thoughts for the day.