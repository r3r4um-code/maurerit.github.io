---
title: "My Autonomous Tank"
date: 2025-04-29T03:36:58-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

Building something that can think and act on its own has always been a dream of mine. During my Arduino phase, I found myself experimenting with all sorts of sensors, including an ultrasonic range finder. This little device, capable of measuring distances using sound, became the cornerstone of an ambitious idea: creating an autonomous tank. What started as a simple curiosity soon turned into a challenging and rewarding project.

During this phase, I was constantly on the lookout for interesting sensors and components to experiment with. One of my favorite finds was the ultrasonic range finder. At the time, I didn’t have a specific project in mind for it, but I knew inspiration would strike eventually. And one day, it did—the idea of building an autonomous tank was born.

I started out by generating a physical diagram and deciding how much power I needed to give it. I knew I needed something more than what the Arduino could handle—5 volts just isn't enough to power those motors fast enough to be anything other than a Mars Rover. I didn't want it to go fast, but I didn't want it crawling around at a snail's pace either. Once the physical diagram was done, I identified my major components and could start hacking on the code.

I opted to use C++ as I wasn't all that great at functional programming. My brain required the objects to work properly. I had a turret class, a 'treads class' (can't remember what I called it), and a battery class. I wanted to also put some telemetry on it, but I never got that far. I tried to keep things as small as possible because the overheads of C++ vs C were quite different. It BARELY fit into the chip.

Finally, the day came when I could test it. I put the tank up on 'blocks' and started experimenting. It was programmed to go in a straight line until it detected an object 2 feet in front of it. At that point, it would rotate the sensor and take readings at different angles until it scanned from right to left. It would then pick the longest distance it found in that scan and start turning in that direction, scanning the whole time until it found a decent vector. Watching it do its scan was fascinating.

Testing this way went okay, but I couldn't easily tell which direction it SHOULD be picking. So, I decided to just put it on the floor and let it do its thing. It only turned left... I guess I trained it on NASCAR data. It also seg-faulted because something in my linked list implementation wasn't freeing its memory. I could have sworn that I freed the readings, maybe not the linked list itself though but I 'cleared it' and went back to adding more reading nodes. That code is somewhere inaccessible to me at the moment, so I can't really tell you what went wrong.

After finally getting it on the floor, I seemed to have lost interest. I don't know if it was the bug that I had found or if I just felt like I had accomplished enough. I wanted to take it another step forward, but I couldn't afford the things I needed to make a more permanent tank. Alas, I was a broke newbie developer starting a family, so I didn't have a ton of excess cash to spend on this hobby... because I had other expensive hobbies, like trying to be competitive at Magic: The Gathering. But that's a story for another day.