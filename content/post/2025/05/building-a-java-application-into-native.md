---
title: "Building a Java Application Into a Native Image"
date: 2025-06-09T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

So I recently got distracted and started trying to compile VS Industry into a native application using GraalVM. I found some Spring documentation and followed it, only to find out that the majority of the work is already done by Spring Boot. There are some caveats though... one being that XML files aren't included in the native image. The story starts with me finding a way to run GraalVM.

So I navigate to [graalvm.org](https://graalvm.org) and see what I have to do to install this thing. There is no Debian support and no real package manager support besides SDKMAN? I think I've heard of this; maybe I'll explore it later on. They offer a tar.gz, but I'm not a fan of installing things outside my package manager. So I remember... I really enjoy Docker build containers, so I start to see if the GraalVM team offers any images. They do, but I couldn't find one that has Maven installed, and I'm prototyping and don't feel like extending their image for a potentially failed endeavor. So I look some more.

I finally run across [vegardit/docker-graalvm-maven](https://github.com/vegardit/docker-graalvm-maven) and begin to hack. This thing offers a bash shell, some basic CLI tools, and of course Maven and GraalVM. I read their documentation and mess around with their instructions for how to manipulate the POM. I can't remember exactly what happened, but I think it made zero sense. It kept complaining about "`native-image --version` returned non-zero return code" or something. I search around a bit, but this is futile. So I step back and start over.

At first I do a simple Brave search for how to build a Spring Boot native image and find the Spring Boot main [documentation](https://docs.spring.io/spring-boot/reference/packaging/native-image/index.html), which leads me to graalvm.org again. Not too helpful. So I search some more and come up with a reference to Spring Native, but documentation links I found were broken. I then fumbled around some more and ran into [this](https://docs.spring.io/spring-boot/how-to/native-image/developing-your-first-application.html#howto.native-image.developing-your-first-application.native-build-tools.maven) page somehow, which just says that simply executing `mvn -Pnative native:compile` will get you a native executable. Hmm... let's try this in that image.

I execute the command in the container and wait. It spikes my CPU pretty hard for a while, and eventually out the other end I had a VS Industry executable with some .so files in the target folder. Nice, let's execute it... crash nearly instantly. So I go through the stack trace, and it says basically "[config] missing resource 'ehcache.xml' not in the classpath" or something similar. So, OK... I know how this error works when Java and a JAR are involved, but now, how do we include this ehcache.xml file we created?

I went to Grok first because I've found Grok less dry. I think that's the right way to use "dry," lol. I tell it to do a deep search, and it scours the web for me for a while. The [conversation](https://grok.com/share/bGVnYWN5_816daa61-30f1-4534-8c3b-6d1a36d1ddcc) gave me some interesting information, but all of which led to nothing I hadn't already found. So I head over to ChatGPT... I haven't used ChatGPT for a while, so I ask it the [question](https://chatgpt.com/share/6844fe10-1ddc-8007-b835-d5512e75cc34)—basically the same thing I asked Grok. It leads me to a configuration file I can put in my src/main/resources/META-INF/native-image folder, and native-image will pick up on it during compilation. So I start with ChatGPT's hint.

I run the command again, and this time a compilation failure happened. It stated that my configuration file is missing the pattern element. So I search around some more and finally run across a Brave AI result that showed the pattern name in the JSON with a regex as the value. I figure, let's try that instead of glob. So I do, and it compiles. Sweet, now let's run it... execution happens and again with the failure. I quickly conclude that it was the same error—surely Brave AI was just as wrong as ChatGPT and Grok—so I scroll up through the stack trace, and it looks different this time. I keep scrolling until I see:

```
Caused by: org.graalvm.nativeimage.MissingReflectionRegistrationError: The program tried to reflectively invoke method public org.glassfish.jaxb.runtime.v2.runtime.property.ArrayElementNodeProperty(org.glassfish.jaxb.runtime.v2.runtime.JAXBContextImpl,org.glassfish.jaxb.runtime.v2.model.runtime.RuntimeElementPropertyInfo) without it being registered for runtime reflection. Add it to the reflection metadata to solve this problem. See https://www.graalvm.org/latest/reference-manual/native-image/metadata/#reflection for help.
```

I ran across some documentation for this on GraalVM's [site](https://www.graalvm.org/jdk21/reference-manual/native-image/dynamic-features/Reflection/). Apparently, GraalVM doesn't understand reflection, so it provides a way to map how objects interact with other objects through reflection—I think that's right—and the community has built a ton of configurations for a ton of libraries, it seems, and Spring Boot or GraalVM integrates those configs into the build. Except for that Glassfish JAXB implementation, it seems. Damn, I was so close. But I'm tired, and I really don't need native images anyway. While it would be nice, memory is cheap, and this thing runs with 412M of actual memory on the host. In the Docker image I provide, the heap is set as low as 96M, so this app is pretty slim for a Java/Spring-Web/Hibernate/JAXB/EhCache/Spring-Retry/Spring-Scheduling stack.

See all I did to get to where I got, maybe you'll be able to use it.  The commit is here: [mvn -Pnative clean native:compile](https://github.com/maurerit/vs-industry-backend/commit/80346d07bddbe90f7411298db08b2e4bd9d015b3)

Now, through my searches, I've found a different optimization I want to do. Stay tuned.


