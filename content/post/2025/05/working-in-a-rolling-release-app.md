---
title: "Working in a Rolling Release App"
date: 2025-06-08T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

The application I'm writing is actively being used.  In fact, the database that I use and test with are the same.  This complicates things quite a bit.  You see, I'm still in the prototype phase so things are changing dramatically.  The database schema is changing the code is changing and constantly being used in a production like capacity.  These constant changes make it rough for determining exactly what state I'm in.

I've had a few issue's.  Like, take for instance my usual migration workflow.  I make sure the app is off, I tar up the current database and restart the app with the new changes.  I then run a few imports and watch things to make sure inventories are updating appropriately and costs are updating properly.  Then it's a matter of the configuration I have in place.  You see, I'll make changes to the current DB and then sometimes things break and I have to revert... losing those changes.  I sometimes forget which then screws things up and I have to take inventory again.  Ugh.

I've seemingly managed to maintain a bit of sanity though so all is well.