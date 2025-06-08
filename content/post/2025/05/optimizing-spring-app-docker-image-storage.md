---
title: "Optimizing Spring App Docker Image Storage"
date: 2025-06-10T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: true
---

While I was trying to build a native image I ran across `mvn spring-boot:build-image` which I tried in that image I mentioned yesterday and it failed because it was trying to connect to the docker socket?  What?  I just wanted a native image...  So I read some more and find that this is tying into the whole buildpack stuff that I keep seeing referenced.  I'm semi interested but I do like to have some manual control.

After messing with build-image and having my vs-industry backend ship up to docker hub as vs-industry:1.0.22 I look deeper into this Baeldung article that I found.  It was talking about how the build-image task works and how it optimizes both the build and the resulting image.  It all comes down to layered jars.  We add a configuration param to our spring-boot-maven build plugin and