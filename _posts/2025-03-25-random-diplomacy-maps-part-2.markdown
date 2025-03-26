---
title: "Random Diplomacy Maps Part 2"
layout: post
date: 2025-03-25 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- game development
- procedural generation
star: true
category: blog
projects: true
project: procedural_maps
author: dario
description: Environmental modeling for Diplomacy board game maps
---

## Continued working on random map generation for Diplomacy

I got some feedback on my map generation tool from Diplomacy players that mentioned some more features that would be useful for balance:

- Every player has access to the water
- All water zones are connected
- Each player has access to at least one neutral supply center
- Central players have a bonus supply center
- Starting supply centers do not border other players' starting supply centers

### Managing water access
To ensure water access, I added a check for each player to see if they are bordering water. To ensure water connectivity I had to implement a check to group all water zones in groups that are connected. If the list is larger than 1 the water zones are not all connected. 

First we connect all the coastal zones. For now for simplicity we are only supporting "Kiel" type coasts, where a naval unit can move across land to another adjoining body of water.
![Water connections](/assets/images/maps/details/waterconnections.png)

Then all the deep bodies of water that are no adjacent to land (these are tracked separately)
![Water connections](/assets/images/maps/details/waterconnections2.png)

In this example all players can access water, but not all water zones are connected.
![Water connections](/assets/images/maps/details/waterconnections3.png)

To solve this, we want to be able to draw rivers that can connect bodies of water. We want our rivers to look natural, so we'll adjust their paths to following terrain height values.

The first attempt was drawn with too wide a width lol.
![Water connections](/assets/images/maps/details/waterconnections4.png)

A lower width looks better.
![Water connections](/assets/images/maps/details/waterconnections5.png)

Instead of a hardcoded start and end point, we want the starting and ending points to be from two different groups of unconnected water zones -- we'll find the closest zones in each group and attempt to connect them. We'll also want to keep drawing rivers until everything is connected. Provinces are regenerated each time a river is drawn so the political map will look different.
![Water connections](/assets/images/maps/details/waterconnections6.png)

We also added a couple of improvements -- random names for regions -- as well as an additional method to find large groups of land pixels and add them as territories (usually adds a couple of islands as regions which is fun)
![Water connections](/assets/images/maps/details/waterconnections8.png)

What is not pictured is the starting composition of units. They can be armies or navies. This is addressed in the WebDiplomacy variant export which includes an option to specity naval/army unit balance. As an option there can be a fleet minimum, such as 1, to ensure every player has a fleet.

### Supply center balance
As here the most central player (calculated by average distance to each other players' territories) has been given an extra unit. The number of most central players to boost has been added as an option to the generation process
![Water connections](/assets/images/maps/details/waterconnections9.png)

In addition supply center generation has been changed from randomly placing supply center to now attempting at the beginning to give each player a supply center at the closest available neutral location. This is not always 100% fair but is a lot more fair than the random placement.

What is still to be done is avoiding starting supply centers between different players that border each other. That will be an option for a next update.

### WebDiplomacy Variant export
The maps themselves are fun to play with, but the end goal was always to support use in Diplomacy game hosting software. The main platform in-use is WebDiplomacy, so I've added experimental support for exporting the required files for us in WebDiplomacy. These can be exported with the 'Export' button. What this does is generate several versions of the map (small and large), as well as place the city names in separate images finally. It generates the necessary PHP scripts to install the variant. Everything is zipped client-side and then downloaded from the browser. At some point it would be cool to add the ability in WebDiplomacy to have a 'random map' game mode where the system generates the variant dynamically -- but that will take some more work to fully integrate.
![Water connections](/assets/images/maps/details/variant.png)

That's it for now as I rotate back to my other projects. [p5.js](https://p5js.org/) has been a great tool and has been much more performant that the generation [I was pursuing in Unity3D with C#]({% post_url 2024-03-11-creating-fantasy-maps %}). At some point I want to return to this again as a starting point for some more Game Dev but that will wait for now.

<iframe src="https://editor.p5js.org/mcoirad/full/NxyGxcgZK" width="1050" height="1550"></iframe>






