---
title: "Backup Solution 2025 Pt2"
date: 2025-05-14T01:42:26-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Yesterday I talked about setting up backups to S3 instead of buying a new Synology NAS.  That plan is potentially changing as I forgot I don't have to buy the drives... probably...  If I don't have to buy drives then a new unit is just 200 bucks.  I think I should have enough rewards for that fairly soon so it won't cost me anything that wasn't already trapped in a spending loop.  So, things may change in the future.

I did implement the backup solution I talked about yesterday and it worked a treat.  Being on a laptop the system isn't always guaranteed to be alive.  It may sleep or I may reboot it because it needs it.  Mainly the sleeping part.  Although sleeping SHOULDN'T bring any issue's since the OS pauses the world an waits.  Going to sleep WHILE uploading though may have some issue's...  But I digress, the solution worked with multiple stops and starts.

Due to how the author wrote the script, each file is logged as it's uploaded so killing the script doesn't have any terrible side effects.  Ideally any upload that is happening finishes first but if not then it's not a HUGE deal.  We'll lose like maybe one picture.  I'd rather that than my whole library.  Right now I have about 14 years of memories on this phone.  From our vacations with our nieces and nephews to our sons first breath and then through school and life so far.  Too many memories to lose all of them.  This is how you write a script.

The backup took a bit of time.  I didn't time it but I started the backup around 8pm and with all the restarts and sleeps I was finished at around 9am.  So right around 13 hours or so for the full backup.  It probably went faster than this because the laptop was put to sleep when I went to sleep and then woken up throughout the night due to my broken sleep schedule.  All in all, I'm happy with that time, it's not a time that will happen often as we're only doing incremental backups from now on.

Now to see how much transfering 20 some gigs of data into AWS will cost me, lol.