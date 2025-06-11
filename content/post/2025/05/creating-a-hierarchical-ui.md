---
title: "Creating a Hierarchical UI"
date: 2025-06-12T00:00:00-04:00
author: Matthew Maurer [maurerit](https://github.com/maurerit)
draft: false
---

I need to create a hierarchical UI. Industry in EVE is multi-layered with multiple tech levels. There is Tech 1, 2, and 3, with Tech 2 depending on Tech 1, but Tech 3 not depending on Tech 2. Materials used for Tech 1 are harvested from asteroid belts and mining anomalies; Tech 2 materials are harvested from moons; Tech 3 materials are harvested from wormhole space. For Tech 2 and Tech 3, there are multiple phases of reactions you have to perform.

Take for instance an Acolyte II—it usually costs somewhere around 450,000 ISK, and we're currently building them for 365,000 ISK. I don't mean to flex; I just want to illustrate the scale of things. A Tech 1 frigate will run you anywhere from 500k to 1m ISK. A Tech 2 small gun is ~900k ISK, and a small Tech 2 afterburner (module to make you go faster) costs ~3m ISK. Each of these things is built from a Tech 1 variant of the same type plus other things. The Tech 1 thing isn't the only thing that can be built—there are some components that have no tech level that can be built. These components' pure purpose is to be consumed in industry with various modules and ships... now... the building of these components is where it starts to get more complex.

The materials that make up the components we're talking about come from an entirely different process. Still an input and output system—put in some materials/things and get some things out the back. Where things get complicated is WHERE you can do these things. You see, the process we're talking about is reactions, and they can't be performed in Non-Player structures or in High Security space at all. We solely operate our industry out of High Sec minus some of our extraction processes. So not only does this process bring extra risk, but it also brings extra costs. They also require fuel... which can be built... If you want to see the Acolyte II's production chain, you can see it [here](https://ravworks.com/tree_view/2205/0).

The Laser Focusing Crystal is this component I was referencing where it has no tech level. You can click on that and it will narrow the chain down to only this item. The level to the right is the reaction process, and the next level to the right is extracting from moons on the top and fuel on the bottom. Then there is one last level, and that is what we call Planetary Interaction—another extraction process. I love this visualization, and I want something that illustrates this and makes it so that you can plan around this entire chain.

Take for instance the choice of purchasing the Laser Focusing Crystal vs. building them. Then consider doing all the reactions as well and vertically integrating all the way down. What is the most profitable? This is what I want to build—something that illustrates the hierarchy and allows the user to configure how deep down the chain they want to go on a per-product basis. I need this planning tool. I know it's not revolutionary; others have done this as well, but none have integrated the warehouse concept.

I'll see about vibing that into existence sometime soon.