---
title: "Working With the Ai Again Today Upd3"
date: 2025-05-31T14:58:46-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Just cleaned up a serious issue in my project after asking the ai to create a db table.  They indexed a new stat value... this is always going to be the value we're looking up... not what we're filtering by... don't waste the disk space and index calculated fields.

Don't over index!