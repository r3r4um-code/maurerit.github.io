---
title: "VS Industry v1.0.22 Released"
date: 2025-05-24T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

The main thing I like out of this little release is the new charts. I was perusing GitHub one day this week and decided to go to Apache's organization page to see what they had. One of their pinned repos was called ECharts and was a TypeScript repo. This intrigued me because who doesn't like looking at their data as an interactive chart?

I created an issue in GitHub and moved on about my day. I couldn't stop thinking about implementing some charts though, but I was lost as to exactly where I would add them. There's really not a whole lot of chartable data in the app right now. Maybe some charts in the dashboard, but that's pretty slim pickings right there. Then it struck me—a pie chart in the Market Orders page.

I got off work and went straight to Junie to see about implementing the pie chart. The AI was quick to implement a rough chart with the number of orders as the metric. I tweaked this manually to make it ISK value and had VS Code with Claude 3.7 Sonnet fix up some styling. The default colors did not go well with my background, and I used VS Code because it allowed pasted screenshots. Junie did not allow the pasting of screenshots :(

The first draft looked like this:

![First Draft](../2025-05-24-MarketOrders-FirstDraft.png "The first draft of the pie chart on the market orders page")

I quickly discovered that I did not like the buy orders graph being right there. There were a couple of reasons for this: 1) It's not near its data, and 2) It pushes the sell orders chart over too much and messes up tooltips. So I had VS Code split the charts, sell orders by its data and buy orders by its data. After that, I enlarged the size of the charts and landed on the second draft.

![Second Draft](../2025-05-24-MarketSellorders-SecondDraft.png "The second draft of the pie chart on the market orders page")

This draft also shifted the values from number of orders to total value of the remaining listed items. This is a much more useful metric to be looking at and planning around. This ended up being what I wanted to ship, but I packaged the app off of my develop branch instead of master... I need to setup some actions in github... and ended up with this:

![Final Release](../2025-05-24-MarketSellOrdersPieChart.png "Final Version")

I added the inner chart which shows the number of items that are remaining. It will closely mirror the outer ring but will be slightly different. Ideally, I'm in a situation where my light drones are heavier in quantity than mediums and nearly the same ISK value. That's long term though; I don't think we have the ISK to achieve that just yet. Plus, we're pretty heavily invested in medium drones at the moment.

I rather like how the charts interact on the page. The tooltips are fast and responsive with easy formatters in the setup. This led the AI to reuse a function to format the ISK value appending the ISK suffix. Pretty slick that it reused a function it wrote weeks ago. Interacting with the legend is fast and seamless with items "jumping out" as you hover over their legend and then animating in and out as you turn them on and off. So far, I like the chart implementation. Good job, Apache.

These AIs are pretty impressive. The more I have them do, the more I'm impressed with what they can do. All you have to do is have a decent idea of how an app should be structured, slice YOUR app up to meet these best practices, and then feed these bite-sized chunks to the AI. As long as you're testing hard, committing often, and feeding the AI bite-sized chunks, it'll do what you want.