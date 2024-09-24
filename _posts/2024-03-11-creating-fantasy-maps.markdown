---
title: "Procedural Fantasy Map Generation"
layout: post
date: 2024-03-11 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- game development
- procedural generation
star: true
category: project
projects: true
project: Procedural Fantasy Maps
author: dario
description: Generating 2D Fantasy Maps
---

## Building fantasy maps

On taking a break from some of my other projects I got into another interest. I really love fantasy maps. There is something about the gestalt escapism of new worlds that is captured by them for me. To the point where they maybe interest me more than reading fantasy books, for example. I used to spend time drawing them by hand and imagining the cultures and peoples that would inhabit them. So eventually I got to thinking about how I could build rules for generating my own.

I found some great already existing work:

- [Red Blob Games Mapgen4](https://www.redblobgames.com/maps/mapgen4/)
- [mewo2's excellent walkthrough](https://mewo2.com/notes/terrain/)

So naturally I cribbed some to create my own.

### Environment modeling

The basic steps to follow are to do a simple simulation of natural forces. I used fractals but you can get good results from a variety of noise generators.

First randomly generate a heighmap of elevation.

![2D Pixel Art Fantasy Map](/assets/images/maps/height.png)


Then do a gradient for temperature.

![2D Pixel Art Fantasy Map](/assets/images/maps/heat.png)

Simulate moisture and flux.

![2D Pixel Art Fantasy Map](/assets/images/maps/moisture.png)
![2D Pixel Art Fantasy Map](/assets/images/maps/flux.png)

Then use these different values to color your biomes.

![2D Pixel Art Fantasy Map](/assets/images/maps/biome.png)

From there you can do a lot of different things, but you may find that attention realism may slow you down and actually produce worse results. For example, I tried to include an erosion simulation. But I don't really like the results.

![2D Pixel Art Fantasy Map](/assets/images/maps/erosion.png)

Placing map symbols though turned out very nicely.

![2D Pixel Art Fantasy Map](/assets/images/maps/symbols.png)

### Political/economic boundaries

Next I came up with some different country boundaries. Also creatd a method to place cities.

Then roads to connect the cities, placed with an A* Pathfinding algorithm. This however turned out to be quite slow so I wonder if there is a better way to build realistic looking results with a lot less computation.
Once we get too deep into optimization this stops being a side project though lol.

![2D Pixel Art Fantasy Map](/assets/images/maps/territory.png)

### Turning it into a video game

After spending so much time on the map I tried adding some gameplay to flesh out a minigame. Since I had countries and cities I added units that can be built and sent out to fight other units or conquer cities. For now I just used a Fire Emblem style Horses vs. Spears vs. Swords rock-paper-scissors type of deal. 

At this point this turned from a side-project into a real project! I think this could be the basis of a very fun game. But I don't have unlimited time so I'll put it back to the side and return to my other projects.

![2D Pixel Art Fantasy Map](/assets/images/maps/game.png)

## More maps

I'm happy with how they've come out generally. I don't really like how the sea monster came out.
But they have a unique style. If I return to this there will be some need for optimization. Currently things really bog down after 3000px size maps. 
Ideally would want to be able to generate a chunk of a map, and then continually generate other chunks as needed.
The game simulation behavior, other than the pathfinding, would scale fine since it doesn't have to be attached to the view.

![2D Pixel Art Fantasy Map](/assets/images/maps/map3.png)
![2D Pixel Art Fantasy Map](/assets/images/maps/map392.png)

Larger Examples:
[2000x3000px](/assets/images/maps/map470.png)
[3000x2000px](/assets/images/maps/map324.png)



