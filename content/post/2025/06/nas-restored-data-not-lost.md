---
title: "I bought a new NAS"
date: 2025-06-20T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

My rewards stacked up finally, and I was able to replace my NAS chassis. Thankfully the drives were just fine—otherwise it would be a couple of months to get new ones :(. Unfortunately, I didn't have any of the photos that I was hoping I had. All I had were photos that we still have on our phones, which are backed up to iCloud, my computer, and S3. I think that is redundant enough. Oh, also, the NAS itself is backed up to AWS Glacier, so there is even yet another layer of redundancy. Even though I don't have any of those old memories of our old dog Bling, I'm happy to have the NAS back.

Now to modify my backup script from last month to target the NAS instead of S3. The problem? I'm getting permission denied errors... I'm mounting it like so: `sudo mount -t nfs -o proto=tcp <ip>:/volume1/photo /mnt/nasphoto/` and the nasphoto folder there is owned by me. I need to read up on NFS, but I could have sworn it was this easy on my Mac years ago... I even mounted it the same way as I remember... So what is the issue now? I did have to make a change in my NAS for the NFS connection to be accepted—allowing by hostname... does that do something different to how files and folders are owned?

I'll keep these pages posted on what I find and how I solve this issue.