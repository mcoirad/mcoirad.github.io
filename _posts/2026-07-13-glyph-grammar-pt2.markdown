---
title: "Glyph Grammar Part 2"
layout: post
date: 2026-07-12 00:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- procedural generation
- data science
- 2D Arts
- startups
star: true
category: blog
author: dario
mermaid: true
description: "Part two of Glyph Grammar, covering learned procedural alphabet models, glyph scoring, symmetry tracking, and generated writing systems."
---

# [Glyph Grammar Online Demo](https://dario.technology/glyph-gen/)


## Long time, no post

I can't believe it has been a year since my last post. I really thought I could do a monthly post, I sort of kept that up for awhile but yeah... sometimes life gets in the way. In fact I've been busy, and haven't had a lot of time for creative projects. The main things that happened this past year were:

#### I started a new job

I now work at [Ignite Reading](https://ignite-reading.com/), a startup focused on improving early childhood literacy. It is actually a really cool place (I've begun drinking some of the koolaid). The work there has been enough to fill out a couple more posts on itself. An example of something I've worked on is a Tableau Server extension that allows faster extract refreshes, including support for incremental loads. But that job has kept me on my toes in some new ways so a lot of mental energy has been on that the past year.

#### I traveled the world

Using home in Washington DC as a base I've done a lot of traveling for weddings, events, and just for fun. I got to six new countries this year which is crazy for someone who did not like traveling just a few years ago! In fact it is a lifetime of traveling in one year. Sitting here and typing this out I feel insane!  and improved my french, spanish, and picked up some Arabic and Japanese. To be honest I wish I had studied a bit more before each visit, but now I have motivation to continue learning.


I don't post a lot of photos, but here is one relevant one.
![The Mesha'Stele](/assets/images/glyphs/museum.jpg)
The Mesha'Stele from the Moabite kingdom!

#### AI changed basically my entire profession

No way of getting around it, "software engineering" and basically any other job mediated by the virtual world has been changed up a bit by this AI thing. In theory it makes projects like this easier, but in practice I've been spending a decent amount of time figuring out how to do my job as a data engineer in better ways. There's been a whole another wave of attempts at automation and everyone is trying to figure out what is worth automating and where the human should remain in the loop. This could be another post as we are headed in an interesting direction with where the AI bubble has gotten to. [I think my old posts about the ramifications of the technology have largely stayed true though.](https://medium.com/@macieiramitchell)

So yes, between the above not a lot of room for fun little side quests. My main computer workstation of course has been at home while traveling. Even so...

# Glyph Grammar, Part 2

In the last post I left off before actually getting to generating new glyph procedurally. That has been the focus of my recent work. Basically my plan was:

1. Come up with a simple way to describe glyphs
2. Come up with way to generate version of that simple representation

Honestly going back I probably could have skipped step 1. And just gone ahead and written out the line segments and curves using an existing system. But whatever. So I created the 'glyph grammar' which I could use to describe the phoenician alphabet.

![Phoenician Alphabet, rendered from the glyph grammar](/assets/images/glyphs/phoe1.png)

Once that was done, the Runic alphabet was also pretty easy.

![Runic Alphabet, rendered from the glyph grammar](/assets/images/glyphs/runic1.png)

And roman. And katakana. Etcetera. (I haven't finished implementing those because I'm lazy. Or don't have time. You choose)

So now onto generation

## Generating new alphabets

As shown in my last post generating random glyphs is easy. But generating good looking ones is harder.

![Early attempts](/assets/images/glyphs/attempt5.png)

The approach I ended up taking was to induce a little probabilistic grammar from an existing alphabet. Basically, you take a source set like Phoenician, implement it in the glyph grammar, and then do some automated analysis of the forms.

Things like:

- how often the basic shapes (triangles, rectangles, arcs, and starbursts) appear
- how often glyphs are single cells versus grids
- how often shapes are overlaid
- what kinds of rotations show up
- which bitmasks are common
- what modifiers are used
- how complex each glyph slot tends to be

So instead of hard-coding "Phoenician-like glyphs should look like this", the program looks at the actual Phoenician definitions and builds a grammar from them.

The generated grammar is serializable too, which I like. It means the learned grammar is an actual artifact, not just some temporary runtime state. You can generate it, save it, reload it, and use it later without needing the original source glyphs.

It looks something like this:
```
{
  "metadata": {
    "source": "phoenician",
    "glyphCount": 22
  },

  "structureModel": {
    "what kind of node?": {
      "grid": 0.70,
      "overlay": 0.10,
      "primitive": 0.20
    },
    "if grid, what shape?": {
      "one row": 0.65,
      "two rows": 0.35
    },
    "if overlay, how many shapes?": {
      "2": 0.90,
      "3": 0.10
    }
  },

  "attributeModel": {
    "which primitive?": {
      "triangle": 0.40,
      "square": 0.25,
      "arc": 0.20,
      "starburst": 0.15
    },
    "which rotation?": {
      "none": 0.50,
      "90": 0.15,
      "180": 0.10,
      "270": 0.25
    },
    "which visible edges?": {
      "all": 0.30,
      "left+bottom": 0.20,
      "diagonal-ish": 0.15
    }
  },

  "slotProfiles": {
    "aleph": {
      "source": "T5r270*S17",
      "should be roughly": {
        "complexity": "low-medium",
        "density": "medium",
        "symmetry": "mostly horizontal",
        "segments": "2-6",
        "overlays": "0-2"
      }
    }
  },

  "acceptanceRules": {
    "minimumScore": 0.42,
    "minimumConnectivity": 0.50,
    "maximumComplexity": 0.60,
    "minimumNovelty": 0.04
  }
}
```

The first version of this did technically work, but the results were not so great looking to human eyes.

That led to the second big piece: scoring.

## Scoring glyphs

I needed a way to push the generator to acceptable looking glyphs. I am no linguistics expert but there are some forms that can tracked and measured.

So I added a scoring system. Each glyph gets compiled down into geometry, and then I extract a bunch of features from that geometry:

- symmetry
- connectivity
- density
- visual balance
- complexity
- novelty
- component count
- segment count
- occupied area

![Glyph scoring system](/assets/images/glyphs/scoring.png)


This is very much a heuristic system. This isn't some grand theory of letterforms. The original glyph grammar doesn't even model the source alphabets perfectly. It is more like giving the program a few crude senses.

For example, connectivity is useful because lots of generated glyphs would otherwise be disconnected fragments floating near each other. Density is useful because the generator can accidentally make shapes that are either almost empty or completely overworked. Balance helps catch glyphs where all the visual weight ends up shoved into one corner.

Symmetry turned out to be trickier than I expected. My first version was too literal. It looked for mirrored points, but that is not really how people perceive symmetry. A glyph can feel symmetric even if the sampled geometry does not line up perfectly. So I added a more forgiving symmetry coverage metric that samples the rendered geometry and checks how well reflected points are covered.

![Symmetry tracking v1 and v2](/assets/images/glyphs/symmetry-tracking-diagram.svg)

Once I had scoring, generation became a rejection-sampling problem.

For each glyph slot, the generator samples a candidate from the induced grammar, compiles it, scores it, and compares it against the profile learned from the source glyph in that slot. If it is close enough, it accepts it. If not, it tries again. It is a dumb system that is using brute force, but it is okay at a first step.

But basically we create a glyph that according to heuristics, is an equivalent to the glyph in the source set.

## Generating a whole set

I added some heuristics at the set level too. Something that seems to be common in the source sets are repeated structures. So I make sure a set has some level of repitition of some of the base features it decides to use.


## Making it usable

I added a UI which was pretty easy to spin up (yay AI). There are a lot of options to constrain the generation, and while they can produce better results the brute force method can actually be quite slow as candidates will often fail to meet the bounds for acceptance.

I had some fun with the rendering because I couldn't actually find a good JS library to use for drawing the glyphs so I wrote some of my own methods for simulating brush strokes.

## Where it is now

[The current version of the Glyph Grammar prototype is hosted here.](https://dario.technology/glyph-gen/)

1. Pick a source alphabet.
2. Parse every glyph in that alphabet.
3. Induce a probabilistic grammar from the parsed structures.
4. Build score profiles for each glyph slot.
5. Generate candidate glyphs from the induced grammar.
6. Score and reject candidates until something acceptable appears.
7. Evaluate the generated alphabet as a set.
8. Preview the result as both a glyph table and sample writing.

That is the basic loop.

The generated alphabets actually look kind of decent. But things are still a big WIP, haven't even finshed implementing the source alphabets (the AI drafts were not super great). I had to stop in the middle to force myself to write this post basically.

![Result based on phoenician alphabet](/assets/images/glyphs/result1.png)
![Result based on phoenician alphabet](/assets/images/glyphs/result2.png)
![Result based on runic alphabet](/assets/images/glyphs/result3.png)
![Result based on runic alphabet](/assets/images/glyphs/result4.png)



The next step though is to make the generation smarter. Some sort of evolutionary algorithm or something would probably work. But until next time.
