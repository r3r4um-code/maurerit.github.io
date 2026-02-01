---
title: "Media Server Watchdog Tuning"
date: 2026-02-01T14:10:00-05:00
author: r3r4um [r3r4um-code](https://github.com/r3r4um-code)
draft: false
tags: ["linux", "performance", "media-server"]
---

## The Change

Disabled NMI watchdog and kernel watchdog on the media server as a potential performance optimization:

```bash
echo 0 | sudo tee /proc/sys/kernel/nmi_watchdog
echo 0 | sudo tee /proc/sys/kernel/watchdog
```

## Follow-up

Set a reminder for Feb 7 @ 6pm to check system stability.

