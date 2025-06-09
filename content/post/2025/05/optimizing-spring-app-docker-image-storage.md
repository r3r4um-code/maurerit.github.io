---
title: "Optimizing Spring App Docker Image Storage"
date: 2025-06-10T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

While I was trying to build a native image, I ran across `mvn spring-boot:build-image`, which I tried in that image I mentioned yesterday. It failed because it was trying to connect to the Docker socket—what? I just wanted a native image... So I read some more and found that this ties into the whole buildpack stuff that I keep seeing referenced. I'm semi-interested, but I do like to have some manual control.

After messing with build-image outside the container and having my vs-industry backend ship up to Docker Hub as vs-industry:1.0.22, I looked deeper into [this](https://www.baeldung.com/spring-boot-docker-images#1-creating-layered-jars) Baeldung article that I found. It was talking about how the build-image task works and how it optimizes both the build and the resulting image. It all comes down to layered JARs. We add a configuration parameter to our spring-boot-maven build plugin and then run `mvn package`. The resulting JAR contains 'layers' which correspond to dependencies, snapshot-dependencies, application, and spring-boot-loader.

They then give you Dockerfile [examples](https://docs.spring.io/spring-boot/reference/packaging/container-images/dockerfiles.html), so I incorporated the key points into my Dockerfile in this commit: [here](https://github.com/maurerit/vs-industry-backend/pull/48/commits/3b7dc91fca62edf7c428d751b6ba7508402789ee#diff-dd2c0eb6ea5cfc6c4bd4eac30934e2d5746747af48fef6da689e85b752f39557R23-R33). There is also a way to define custom layers, so I may look into that, but I haven't been able to come up with a good scheme yet that isn't just an overhead nightmare.

I won't see the benefits of this until I generate multiple containers off the same Spring Boot version. So it's not a huge optimization, but it's something cool to dig into.