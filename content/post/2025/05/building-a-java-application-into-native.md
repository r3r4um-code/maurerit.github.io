---
title: "Building a Java Application Into a Native Image"
date: 2025-06-09T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

So I recently got distracted and started trying to compile vs-industry into a native application using graalvm.  I found some spring documentation and followed it and come to find out that the majority of the work is already done by Spring Boot.  There are some caveats though... one being that xml files aren't included into the native image.  The story starts with me finding a way to run graalvm

So I navigate to [graalvm.org](https://graalvm.org) and see what I have to do to install this thing.  There is no debian support and no real package manager support besides sdkman?  I think I've heard of this, maybe I'll explore it later on.  They offer a tar.gz but I'm not a fan of installing things outside my package manager.  So I remember... I really enjoy docker build containers so I start to see if the graalvm team offers any images.  They do but I couldn't find one that has maven installed and I'm prototyping and don't feel like extending their image for a potential failed endeavor.  So I look some more.

I finally run across [vegardit/docker-graalvm-maven](https://github.com/vegardit/docker-graalvm-maven) and begin to hack.  This thing offers a bash shell, some basic cli tools and of course maven and graalvm.  I read their documentation and mess around with their instructions for how to manipulated the pom.  I can't remember exactly what happened but I think it made 0 sense.  Kept complaining about `native-image --version` returned non-zero return code or something.  I search around a bit but this is futile.  So I step back and start over.

At first I do a simple Brave search for how to build a spring-boot native image and find the spring-boot main [documentation](https://docs.spring.io/spring-boot/reference/packaging/native-image/index.html) which leads me to graalvm.org again.  Not too helpful.  So I search some more and come up with a reference to spring-native but documentation links I found were broken.  I then fumbled around some more and ran into [this](https://docs.spring.io/spring-boot/how-to/native-image/developing-your-first-application.html#howto.native-image.developing-your-first-application.native-build-tools.maven) page somehow.  Which just says that simply executing `mvn -Pnative native:compile` will get you a native executable.  Hmm... let's try this in that image.

I execute the command in the container and wait.  It spikes my cpu pretty hard for awhile and eventually out the other end I had a vs-industry executable with some .so files in the target folder.  Nice, let's execute it... crash nearly instantly.  So I go through the stack trace and it says basically [config] missing resource "ehcache.xml" no in the classpath or some such.  So, ok... I now how this error works when java and a jar are involved so now, how do we include this ehcache.xml file we created?

I went to Grok first because I've found Grok less dry.  I think that's the right way to use dry, lol.  I tell it to do a deep search and it scours the web for me for awhile.  The [conversation](https://grok.com/share/bGVnYWN5_816daa61-30f1-4534-8c3b-6d1a36d1ddcc) gave me some interesting information but all of which led to nothing interesting I hadn't already found.  So I head over to ChatGPT... I haven't used ChatGPT for awhile so I ask it the [question](https://chatgpt.com/share/6844fe10-1ddc-8007-b835-d5512e75cc34) basically the same thing I ask Grok.  It leads me to a configuration file I can put in my src/main/resources/META-INF/native-image folder and native-image will pick up on it during compilation.  So I start with ChatGPT's hint.

I run the command again and this time a compilation failure happened.  It stated that my configuration file is missing the pattern element.  So I search around some more and finally run across a Brave AI result that showed the pattern name in the json with a regex as the value.  So I figure, let's try that instead of glob.  So I do and it compiles.  Sweet, now let's run it... execution happens and again with the failure.  I quickly conclude that it was the same error, surely Brave AI was just as wrong as ChatGPT and Grok... so I scroll up through the stack trace and it looks different this time.  So I keep scrolling until I see:

```
Caused by: org.graalvm.nativeimage.MissingReflectionRegistrationError: The program tried to reflectively invoke method public org.glassfish.jaxb.runtime.v2.runtime.property.ArrayElementNodeProperty(org.glassfish.jaxb.runtime.v2.runtime.JAXBContextImpl,org.glassfish.jaxb.runtime.v2.model.runtime.RuntimeElementPropertyInfo) without it being registered for runtime reflection. Add it to the reflection metadata to solve this problem. See https://www.graalvm.org/latest/reference-manual/native-image/metadata/#reflection for help.
```

I ran across some documentation for this in graalvm's site.  Apparently graalvm doesn't understand reflection so it provided for a way to map how objects interact with other objects through reflection... I think that's right... and the community has built a ton of configurations for a ton of libraries it seems and Spring Boot or graalvm integrates those configs into the build.  Except that glassfish JAXB impl it seems.  Damn, I was so close.  But I'm tired and I really don't need native images anyways really.  While it would be nice, memory is cheap and this thing runs with 412m of actual memory on the host.  In the docker image I provide, the heap is set as low as 96m so this app is pretty slim for a java/spring-web/hibernate/jaxb/ehcache/spring-retry/spring-scheduling.

Now, through my searches I've found a different optimization I want to do.  Stay tuned.


