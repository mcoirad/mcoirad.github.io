---
title: "Multiplayer Visual Novel Framework Update"
layout: post
date: 2024-11-15 02:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- game development
- interactive narratives
- 2D Arts
star: true
category: blog
projects: false
project: multiplayer_novel_framework
author: dario
description: Game Engine Update
---
## Hitting a milestone

After a lot of hard work and effort, finally hit a preliminary milestone in this game's development. Basically I've now got the scripting engine to the point where I can do just about everything that it needs to replicate the original game demo I produced in Renpy. Certainly have not replicated all of Renpy, but a minimum of functionality for the dialogue and exploration systems.

I've taken the trouble of creating [an automated running count (using Google App Script)](https://gist.github.com/mcoirad/e4b7de3f9794f51207882a3733e7043b) of [where we are in the engine implementation](https://docs.google.com/document/d/1eqjGHiL9UlSgFJ7AQlC8XVqqODP9ANND7Dhm50MLwaY/edit?usp=sharing):

![Game Engine Progress](/assets/images/gameart/progress1.png)

We are still about halfway through, despite all the continued work, but that's more due to me continuing to flesh out some of the needed features of the game and thus adding more detail to the roadmap.

The main part of it is the scripting engine, which like Renpy, allows for writing out the dialogue in script files.

### Creating a scene
![Game Demo Example](/assets/images/gameart/engine_example1.gif)

```
# Begin
scene
{
	addHotspot(newspapers, [look], (0.25, 0.1), (0.22, 0.19))
	addHotspot(certificates, [look], (0, 0.55), (0.25, 0.35))
}
fadeToBlack(noise)
setBackground(boss_office_back);setForeground(boss_office_front);fadeFromBlack(noise)
```

#### Hotspots and items
Our scripting engine defines a 'scene' by a collection of items and hotspots. Items are just images that are separate from the backgrounds. Hotspots are areas of interactivity that may or may not contain an image. We don't have a real name for the scene, but we use a label prepended by a hashtag '#' which also functions as a comment.

The current example doesn't include any items, which can be added by `addItem()`. Instead there are hotspots for 'newspapers' and 'certificates'. Each hotspot has a list on interactions that will come up on a popup menu if the hotspot is interacted with, in this case each just has 'look'. The final arguments just provide the position and size of the hotspots, relative to the size of the screen.

The actual images of backgrounds are defined separately from the scene.

#### Background action scripting
Here we make use of some redefined actions to control the image backgrounds.

`fadeToBlack` and `fadeFromBlack` control scene transitions, using a 'noise' effect.

`setBackground` and `setForeground` use our layer system to provide two levels of backgrounds, one in front of and one behind our characters.

### Dialog System
<video controls>
  <source src="/assets/images/gameart/example_engine2.mp4" type="video/mp4" style="max-width:100%;">
  Your browser does not support the video tag.
</video>
```
# Intro
narrator "Here we are, at the boss' office."
enter(Boss,interactive=true,options=[look,talk]);setPosition(Boss,0.5,0.65)
Boss "So you're wondering why I called you in tonight." 
Boss "This is a real important case.{a} I don't know if you knew this about Gonzalez but she was colonel on the police force before making a big career in politics."
...
Boss "Mess up, and you'll be on the K9-unit clean-up detail."
@AP "<i>Uh oh.{a} They won't even let the robots do that kind of work. If it came to it, I would rather be fired.</i>"
@AP "<i>I think my Boss is watching out for me, but it's hard to tell sometimes.</i>"
Boss "And before you leave, one more thing..."
Boss "Kirke fixed up the...{a} whatchamacallit{a} <b><color=red>EMP scanner</color></b> thing, make sure to drop by the lab and take that with you." addKeyword(emp_scanner)
jump(Explore)
```

#### Character Management
We enter the 'boss' character using `enter()`, setting him to `interactive` (meaning the character will also have an interaction hotspot in exploration mode), as well as interaction options 'look' and 'talk'. We also use `setPosition()` to place him in the middle of the scene. The engine will automatically search for this character's art files in the Resources folder. The framework will also automatically search for a speaking animation and play it while the dialogue for a character is in progress.

Similar to Renpy sytax, we start off dialog lines with a character's name, and then dialog in doublequotes. Thing like dialog stops `{a}` and HTML styling like italics and bold text are all supported.

One difference is that since this is multiplayer is that there is support for relative dialogue. In this case we have the `@AP` which stands for 'active player', i.e. generally whoever initiated the dialogue or interaction.
In addition we have support for running our action functions within and at the end of each speech definition.

Finally we use a very important action, `jump()` which jumps to the aforementioned labels, which are defined with hashtags. In this case, the script jumps to a label which begins the exploration mode.

### Going from here
You'll notice I haven't posted the actual code that is behind the framework. Earlier I did consider the idea of releasing the framework, potentially as a licensed asset. That still is the goal, however, my priority now has changed to just working on my game. Plenty of behaviors are hardcoded and I'm not yet at the point where I want to spend too much time on cleaning things up and responding to questions and bugfixes. There will be time for that later, once the game is in a more advanced state. (Happy to still share code on a private basis with anyone interested)