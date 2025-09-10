---
title: "Playing With Spec Kit Pt2"
date: 2025-09-13T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

Should have fully approved the specify/plan/task phase documents before moving forward
    Not the biggest of deals but it clutters the git history merging back down
    Went with using labels to show approved PR's
Phase 3.3 being a lot of parallel tasks I'm merging them into the prior 3.3 task PR.
    Everything is new so no conflicts should arise... hopfully configs aren't changed...
Started in again on the 'Buffer out of date' bullcrap
Much more success with having it implement one task at a time instead of multiples
    Should have thought of this before but I think I was trying to save prompts
    Working very much smoother, made it through 6 tests with only 1 buffer issue
I think I'm going to get through this section (section 3.3) then approve all PR's and merge down to the spec.  I've approved the spec (I mean, I can't do it because I created the PR but I do approve of it :P) so we will know that once all features are merged up to the spec branch then it can merge into master... which will be renamed develop and a main branch created.
Having a hard time holding back from running into a wall.
    Although I foresee a wall in my future.  These tests, while structured fine, are going to have issue's I just know it.  I kind of want to watch this process come crumbling down.
        At least I know I have a decent spec.  But maybe I'll be pleasantly surprised by this whole process.
------------------------------------------------------------------------------------------------

I'm moving forward and so far things are getting smoother.  I'm learning lessons and applying fixes to my workflow.  I've been impatient and just rushed forward with the project but now that I have my backend tests I'm stopping until they're all approved and reviewed.  I want to know what I'm getting into once I move onto implementations.

The first issue I have is that I didn't fully 'approve' of the spec before moving forward.  I just barrelled forward without a second thought because I was impatient and wanted to see how this model worked.  I did at least look at the api.yml in an openapi spec viewer so that validated the spec and also gave me a readable output.  I should note that site for later... let me dig that up from my history before I forget.  Here it is, it was [API Bldr](https://www.apibldr.com/) and it's interface was alright.  I think I'll use it again.

The second issue I was about to have was my branching and merging model.  Once I stepped into 3.3 I was going back up to the 3.2 branch and creating another 3.3 branch with the thought that each 3.3 branch would merge into the 3.2 branch.  While this would have worked I think it would have introduced too much wonkiness.  So I'm chaining the 3.3 PR's as well where each relies on the prior section.  This way, once all of my PR's are approved, I just merge up into the spec branch and move onto implementation.

The third issue is one that I've been having and talked briefly of in the prior post.  It is the whole 'buffer is out of date' issue where VS Code prompts me to compare or overwrite all while the AI chugs forward making edits that aren't being reflected in the file.  This of course confuses the AI and it goes into tailship mode burning down forests.  I think my issue was mostly that I was biting off too big of chunks.  I was asking the AI to implement up to 3 tasks in one prompt.  That's a bit much I've found.  Not only was VS Code prompting me about long running jobs but this whole buffer issue was cropping up more.  I have now only had the issue once out of 6 tests.  I was having it about 1 for 1.

Now onto some thoughts.  I'm foreseeing a wall in my future and I think the AI is going to have an extremely hard time figuring it out.  For one, the OAuth workflow for Spring is to not expose the jwt to the user by default, I'll be curious to see how the AI solves for this.  The second is that I foresee major issue's with the tests that are going to be hard for the AI to figure out but I remain optimistic.  I'm curious as to how this whole process plays out and I'm trying to remain optimistic but things are starting to look bad.

Onto the review and next section.