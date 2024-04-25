---
title: "Building a Multiplayer Framework for Interactive Novels"
layout: post
date: 2024-02-24 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- game development
- interactive narratives
star: true
category: project
projects: true
author: dario
description: Building a framework with Unity3D
---

## The ease of developing interactive narratives

Today there exist many tools and engines that allow for the easy creation of interactive literature.

As an example, [Renpy](https://www.renpy.org/) allows the creation of complex interactive novels, while using a scripting language that closely mirrors that of writing a movie script. Its features can also be greatly expanded through use of Python, making it a great tool for teaching introductory programming concepts.

I was interested in creating similar games, but found most frameworks for interactive storytelling to be too limited. As great of a tool that Renpy is, I found it to be most useful for replicating experiences that have been already created. I.e. one could remake the Ace Attorney games their own way, but what is the exact point? I got to the point of creating a demo of my own game, but quickly got to the point of wanting to implement features that Renpy was not built for.

![Renpy Screenshot](/assets/images/1.png)

![Game Demo Screenshot](/assets/images/2.png)

![Game Demo Screenshot](/assets/images/3.png)

## Creating my own framework

I wanted to create a system for multiplayer storytelling. Something like a novel, that can be shared with multiple people. So I started work on a framework for my own interactive novel efforts. The basic goal was to replicate the gameplay of Ace Attorney, as if it were a multiplayer game.

This precluded the creation of my own scripting language, which has borrowed heavily from Renpy. So far I have used Unity as the view layer, but have also been looking at Godot as an alternative. 

At this point [I'm about 50-60% in on what I'd like to implement in my base feature set for the framework](https://docs.google.com/document/d/1zSwSUPcDkkznFxj8uB94N8eU8m4bri6jeO3hq-L-hWw/edit?usp=sharing). I'm kind of caught between two goals -- developing the framework as to use as the basis for my own game, and developing the framework as something that could be shared and published. To the first end I've been working on getting out some art mockups of what I'd want the game to look like. And to the second I'm working on cleaning up the code and creating better documentation. But I probably don't have the time to do both so I need to determine my goals so I know what to focus on.

![Game Demo Screenshot](/assets/images/preview.png)
