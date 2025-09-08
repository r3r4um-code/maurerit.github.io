---
title: "Playing With Spec Kit"
date: 2025-09-12T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I just heard about this new model that GitHub kicked out—they're calling it [spec-kit](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/), and it's a way of putting an AI model on guard rails and having it help out with creating a software feature spec. They break things up into four phases: Specify, Plan, Tasks, and Implement.

Part of me wanted to start applying this to my current project, but when I did, the plan phase started inventing data structures that were CLOSE but not quite right, and I didn't feel like rectifying it just yet. More to come on that :D. So I decided to try this model on a new app I wanted to code up for the family—a simple name drawing app. One probably exists out there, but why not create my own? Let's try to vibe code one.

I'm going to dive into the steps, but I want to preface this all with this: During the plan phase, I discovered that Functional Requirements were going to shift, so I decided to have the AI audit itself. Basically, I told it in the constitution that it should create a memory folder in the spec folder, and inside there I want numbered files that capture my prompts and its responses summarized. This made it easy to see where FRs came from and how they changed. It's also nice for later review as I can see where decisions were made.

The first phase generates a spec sheet and is targeting BAs/Functional Experts, as they can describe the feature in plain English and the AI will create the doc. It will also generate edge case scenarios and NEEDS CLARIFICATION markers, which should be worked through during the specify step until all are handled and answered. I recommend keeping one conversation for this interaction and don't break it up into multiples, as the context is handy.

The second phase is the Plan phase, where you give the AI some more technical instructions, which makes this step target Technical Architects or Principal Developers. They describe the tech stack (that isn't already in the constitution) and other key considerations they can think of, and the AI generates quite a few docs. You can 'follow along' by peering at the [spec PR](https://github.com/maurerit/name-draw-spec/pull/1) and see all the docs that are generated. The Spec document comes from the prior phase, the Tasks document comes from the next phase, and everything else comes from this phase. The quickstart, the datamodel, the research document, etc. All of this comes from the second phase.

In the third phase, you generate all the tasks. I simply executed this step as I spent quite a lot of time on the prior two steps to get to this point, and I didn't know what else should be talked about. Plus, the prompt file didn't mention arguments at all, so it had no instructions to do anything with my input. So for this round, I spent the least amount of time on this step and moved right on to implementation without fully reviewing everything because I was getting impatient.

The final phase is where you give all this documentation to a model to create the code. I decided to go with chained PRs, but then I got to section 3.3 and it has multiple subsections... so I decided to implement a couple of tasks, go up a branch, implement a couple more tasks, etc. On the second iteration, the model got lost. This was with me keeping the conversation flowing. So when I went to do the second bit of tasks, T013 and T014, I ran into issues. At first, it kept running in circles trying to solve for Spring Security issues in the tests, but yet on the previous tasks it didn't worry about this at all.

So I started a new conversation because the AI was completely lost. I didn't have the tasks file open, but it seemed to read the right file in and find the tasks. But then when it went to start implementing things, it was expecting the files to already be there and went into a tailspin for a few seconds trying to find the files. It finally gave up and created them, but when it went back to edit them, VS Code was bitching about the buffer being out of date and that I should compare them or overwrite. So I said overwrite, but it never did... this kept going as the AI kept trying to run tests and understand how to move forward... but it couldn't move forward because its edits weren't being recorded... BREATH... and that was very frustrating to have to sit through. I finally stopped that interaction, reverted what it did, and got back to it... for the third time.

I have restarted this three times so far, and I'm finally at a point where I have tests that are running and failing. They're failing for a bad reason right now, though, but they might actually work once we have controllers. I'm trying to get past this testing phase to see how well this model works, and I'm curious if these AI models can do TDD...

If all else fails, I am happy with the spec doc, so I can start at the plan phase again and archive this stuff off for later reference and learning.

More to come on this topic.
