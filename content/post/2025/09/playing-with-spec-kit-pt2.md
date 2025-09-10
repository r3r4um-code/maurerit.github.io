---
title: "Playing With Spec Kit Pt2"
date: 2025-09-13T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I'm moving forward, and so far things are getting smoother. I'm learning lessons and applying fixes to my workflow. I've been impatient and just rushed forward with the project, but now that I have my backend tests, I'm stopping until they're all approved and reviewed. I want to know what I'm getting into once I move onto implementations.

The first issue I have is that I didn't fully 'approve' of the spec before moving forward. I just barreled forward without a second thought because I was impatient and wanted to see how this model worked. I did at least look at the api.yml in an OpenAPI spec viewer, so that validated the spec and also gave me a readable output. I should note that site for later... let me dig that up from my history before I forget. Here it is—it was [API Bldr](https://www.apibldr.com/) and its interface was alright. I think I'll use it again.

The second issue I was about to have was my branching and merging model. Once I stepped into 3.3, I was going back up to the 3.2 branch and creating another 3.3 branch with the thought that each 3.3 branch would merge into the 3.2 branch. While this would have worked, I think it would have introduced too much wonkiness. So I'm chaining the 3.3 PRs as well, where each relies on the prior section. This way, once all of my PRs are approved, I just merge up into the spec branch and move onto implementation.

The third issue is one that I've been having and talked briefly about in the prior post. It is the whole 'buffer is out of date' issue where VS Code prompts me to compare or overwrite all while the AI chugs forward making edits that aren't being reflected in the file. This of course confuses the AI and it goes into tailspin mode, burning down forests. I think my issue was mostly that I was biting off too big of chunks. I was asking the AI to implement up to three tasks in one prompt. That's a bit much, I've found. Not only was VS Code prompting me about long-running jobs, but this whole buffer issue was cropping up more. I have now only had the issue once out of six tests. I was having it about 1 for 1.

Now onto some thoughts. I'm foreseeing a wall in my future, and I think the AI is going to have an extremely hard time figuring it out. For one, the OAuth workflow for Spring is to not expose the JWT to the user by default—I'll be curious to see how the AI solves for this. The second is that I foresee major issues with the tests that are going to be hard for the AI to figure out, but I remain optimistic. I'm curious as to how this whole process plays out and I'm trying to remain optimistic, but things are starting to look bad.

Onto the review and next section.