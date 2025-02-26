---
title: "Procedural Generation for Diplomacy Maps"
layout: post
date: 2025-01-25 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- game development
- procedural generation
star: true
category: blog
projects: false
project: Procedural Fantasy Maps
author: dario
description: Generating Random Maps for the Diplomacy Board Game
---

## Returning to Maps

[WebDiplomacy](https://github.com/kestasjk/webDiplomacy) is a great open source gaming project. It basically creates an online platform for playing the Diplomacy board game, something that has been played by mail since the 1960s. It is a very simple and elegant game that provides a sometimes brutal introduction to the game theory concepts of cooperation and betrayal.

In the past I had my own server that I set up to play with coworkers during the pandemic. [I even created my own Game of Thrones variant](https://github.com/mcoirad/gameofthrones-diplomacy)

![Game of Thrones Diplomacy Map](/assets/images/maps/got.png)

I was recently on the road and had the idea to return to it, but with the question of what would be involved in generating fairly balanced random maps.

## Diplomacy Random Map Editor

Compared to my last attempt at random map generation, I tried to keep things lightweight and relatively efficient.

The main steps were:
- Generate perlin noise
- Smooth/Modify with a gradient
- Set the water level dividing into water and land
- Place cities, using a minimum distance that they must be apart
- Calculate the actual territories off of the city placement, using the landscape to determine boundaries
- Create players, assign them territories
- Place supply centers

The most fun was coming up with the gradients, which are sort of the flavors for each map type.

### Radial Map
![Randomly Generated Radial Diplomacy Map](/assets/images/maps/diplomacy_radial.png)

### Mediterranean Map
![Randomly Generated Mediterranean Diplomacy Map](/assets/images/maps/diplomacy_med.png)

### Spiral Map
![Randomly Generated Spiral Diplomacy Map](/assets/images/maps/diplomacy_spiral.png)

### "Flat" Map
![Randomly Generated Diplomacy Map](/assets/images/maps/diplomacy_none.png)

And of course:

### Custom Image Map

The silliest is probably the ability to build a random map off of an imported image. This allows the creation of some rather cursed variants.

#### Battle of Austria Hungarys
![Randomly Generated Diplomacy Map](/assets/images/maps/austriabattle.png)

#### Battle for McDonalds
![Randomly Generated Diplomacy Map](/assets/images/maps/mcdonalds.png)

## Map Editor
Here is the editor itself:
<iframe src="https://editor.p5js.org/mcoirad/full/om-x0gXQF" width="850" height="1200"></iframe>
