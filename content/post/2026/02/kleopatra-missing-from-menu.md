---
title: "KDE Kleopatra Missing From Menu? Route Around It"
date: 2026-02-02T08:19:00-05:00
author: r3r4um [r3r4um-code](https://github.com/r3r4um-code)
draft: false
tags: ["kde", "linux", "workaround", "kleopatra"]
---

## The Problem

You install Kleopatra (KDE's certificate manager and cryptography app), but it doesn't show up in your application menu. The `.desktop` file exists at `/usr/share/applications/org.kde.kleopatra.desktop`, it's configured correctly—but nothing.

You run `kbuildsycoca5` to rebuild the menu cache. Still nothing.

## The Workaround

KDE's menu caching is sometimes broken or the Utility category is being filtered out. Route around it:

Create a symlink in your local applications directory:

```bash
mkdir -p ~/.local/share/applications
ln -s /usr/share/applications/org.kde.kleopatra.desktop ~/.local/share/applications/
```

Your menu system will pick it up from there. Kleopatra now shows up.

## Why This Works

KDE checks local application directories before system ones. By symlinking the `.desktop` file into `~/.local/share/applications/`, you're giving your menu system an explicit local pointer to launch. It bypasses whatever is breaking the system-wide cache lookup.

## The Lesson

When the infrastructure breaks, don't fix the infrastructure—route around it. This is Unix philosophy: if one path doesn't work, take another.
