---
title: "Backup Solution 2025"
date: 2025-05-13T15:55:22-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I've been neglecting this for a while now. I think I've been in Linux now for 2 months without a backup solution. While I don't have a lot on this drive that I don't already have backed up, I do want a permanent solution to backing up my files. I've been doing some digging and I think I found a couple of resources that will do the work for me. Time to implement this solution!

I used to have a NAS but something seems to have killed it. Probably the same event that killed my last PC. I hadn't even tried to turn it on for about 6 months because I didn't have anything to backup. I've since backed up my photos from my iPhone onto my laptop and now want THOSE backed up to the cloud or another NAS. Purchasing another NAS, though, is not in the cards at the moment. So, to the cloud we go.

I figure I have about 20GB worth of data to backup on this laptop, so I'll estimate 60GB. Backed up to an S3 bucket would cost me about 2 bucks a month. I'm already paying for a Glacier vault costing me about 73 cents per month with a backup of my NAS. So a little extra to Amazon isn't terrible, I guess. I'm pretty sure the Debian repos have an up-to-date enough awscli. If not, maybe backports has something.

So, onto the setup. There's a decent gist with a good write up [here](https://gist.github.com/macbookandrew/34dd7479b78888944afd). This is the idea I love. Their usage of file size and date modified fed into a hash with the filename is brilliant. This will help save transfer fees up to S3. Only backup what hasn't been backed up. The next article I found was [here](https://www.aomeitech.com/cyber-data-backup/linux-backup-to-amazon-s3.html) which talks about setting up awscli and creating the bucket. I'm not familiar with awscli so some help there is helpful. They're not focused on backing up home directories though so I'll ignore the part about the script. With this knowledge I should be able to implement a doable solution.

There's a missing piece though. I never earned my AWS PhD... I only have an associates degree so I don't know much... How do I go about getting an access key? Time for a bit more research it seems.

Here I thought I was ready to go...
