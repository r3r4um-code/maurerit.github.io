---
title: "I Need Better Slop Control"
date: 2025-05-25T10:06:24-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

While these AIs are impressive, they also stray from their impressive path. I was just removing some stuff from one of my pages, specifically a page size dropdown, and after removing the elements, I noticed an error in the page. A function was declared but never used. This function set a state variable for the page size, triggering a data refresh. Well, lo and behold, there was an identical function directly below that one which did the exact same thing, called from the page size dropdown at the bottom of the page.

I knew watching these AIs would be hard, but damn, this one slipped through the cracks. I'm trying to ship too fast.