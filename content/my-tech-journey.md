---
title: "My Tech Journey"
date: 2025-04-29T00:28:49-04:00
draft: false
---

After high school, I was lost and drifted from factory job to factory job until I decided to enlist in the army. That lasted a few years, taught me a few good lessons, and better prepared me for a life of hard work.  But in other ways it wasn't good for me.

The next phase of my life was a bit of a rebellion. After the military, I grew my hair out, grew my goat out, and again drifted from job to job. It wasn't until I played some EVE Online and got in some voice channels with other adults that I realized... I only have EVE Online to talk about with these people. I had no career, no prospects, no stable job even... let alone a family of my own. So I decided to go to school and try to break into the tech industry.

ITT took me in (because I could get a loan apparently) and did a pretty stellar job in introducing me to this world. I started out on the systems admin path and, while I liked it, it just didn't completely strike a chord with me. I met a guy that was constantly on his little laptop—seriously, this thing was cool. It was like a mini laptop, and I have no clue what it was to this day, but it looked cool. I approached him one day and asked what he's constantly doing on his little laptop, and he said programming. I thought... hmm, he seems to be having a good time with it. Seems interesting enough, it's in tech, I get to make things, and so I finished up the systems admin path and then moved over to their application programming curriculum.

Not long after that, I was hacking my first program for EVE Online to better help me manage my industry... oh, how many times have I written something to help me do something in that game. Let me count the ways:

* **First program**, written in VB back in 2005.
* **Second attempt**, integrated with the CREST APIs (2016/2017):
  * This ended up being mostly a Slack bot.
  * Remnants can be found on GitHub: [abandapart](https://github.com/maurerit/abandapart).
    * Don't judge me by code I wrote 8 years ago :D
  * Written in Java.
* **Third thing**, a Discord bot + companion site to authenticate with EVE and then with Discord:
  * If you know EVE, then you know everyone is paranoid and everyone else is out to get you.
  * Provided a security mechanism so players in A Band Apart could be added to their corporation's channel.
  * Remnants still on GitHub: [chremoas](https://github.com/chremoas).
  * Written in Go.
* **Fourth attempt**, more successful this time but was cludgy:
  * It has no remnants except probably on my old hard drive.
  * Required another program to download API data.
    * That other program was hacked as well.
  * Sometime around 2021.
  * Written in TypeScript.
* **This time**, started around summer 2024:
  * Mostly working, seems to have gremlins in warehouse management though.
  * Written in [Java](https://github.com/maurerit/vs-industry-backend) and [TypeScript](https://github.com/maurerit/vs-industry-frontend).

There were other personal projects intermingled amongst that mess of code. I once built a little [autonomous tank](/post/2025/04/my-autonomous-tank) out of popsicle sticks, a wooden plank, some tank treads I got from a hobby shop, twin motors, an 8-count AA battery pack, Arduino, and an Arduino motor driver board. I bet I still have that code on my Mac... at least I think I developed it on my Mac...

Anyways, it only turned left. I ended up blowing up the heap on the little Arduino because I didn't manage my memory properly. I _THINK_ I know how I went wrong, but without the code, I couldn't be sure. Fairly certain my linked list implementation didn't free when it went to scan the area again. I mostly gave up after that incident because I was pretty worn out I think.  I needed a pallete refresher.

I moved on to messing with XBee's and my RC Car.  Was easy enough to integrate with the ECS and the front steering servo.  I couldn't foresee any issue's with the implementation.  I think I was always limited by my inability to create something physical that I was proud of.  Like enclosures for the controller and custom pcb's.  I dabbled but I never went any further.  I just bounced around interacting with different sensors and generating idea's that I never implemented :D.

There was also the Magic: The Gathering card tracker as well.  Back in around 2013 or so, before I got back into EVE Online I learned ExtJS at a workshop since my company was standardizing on it... that didn't go well... when they didn't give me a project to work on I found my own.  This thing had a scheduled job that went out and scraped a certain web site for prices.  It was tcgplayer.com.  Boy was their site easy to scrape :D.  The frontend... little as it was since I'm not really a frontend guy... was written in javascript with ExtJS as the widgets.  The backend, you guessed it... Java with Spring.

Those are the prominent projects in my mind.  I'm sure my repository list says a different story.  I hack on things, study things hoping to find better ways of doing certain tasks.  And now with the emergence of LLM's I feel like I have a coding partner.