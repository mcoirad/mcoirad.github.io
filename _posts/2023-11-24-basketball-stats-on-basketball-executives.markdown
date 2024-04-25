---
title: "Turning the Table on Basketball's Advanced Stats"
layout: post
date: 2023-11-24 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- data engineering
- basketball
star: true
category: project
projects: true
author: dario
description: Building a framework with Unity3D
---

As a basketball fan of a bad basketball team, something has always bothered me about the inaccessible nature of major league sports leadership. It seems... to put it nicely... to be less than a meritocracy. While playing sports at a professional level seems to be an extremely unique talent, I would argue that managing a sports team is much less than that. Of any NBA team, for example, there are probably some percentage of the fans that could conceivably make better decisions than the team's own front office. (The same could probably said for large corporations)

The performance of NBA players is scrutinized at a level that exemplifies data science's "race to the bottom." Just about everything is tracked statistically in the hope that some advantage can be gleaned. Draft evaluations include not just basic anthromorphic data but also surveys of NBA prospects mental states.

I find it disappointing that we don't have the same data points to measure the performance of the basketball executives that are at the end of the day responsible for the entire team's performance. (Once again I feel the same way about executives of corporations in general) Thus I set out to create my own statistic for this.

### Assembling the dataset

Most stats are easily available. However I needed transactions. I got this from basketball-reference.com and matched players to basic and advanced stats from there. The goal was to be able to assign a value to each transaction an executive made. Transaction are grouped into the following categories:
- Trade
- Draft
- Free agent signing
- Sale

Free agent losses are hard to track and I would have to do more work to build the dataset for this type of transaction -- so I have left that out for now.

For the value of the transaction I used a very simple formula of:

Player BPM * Number of minutes played for team

By creating a running total of these values I can create a stat I'll call "Executive BPM".

This works alright -- but like any stat has its shortcomings:
- trading for/playing a prospect heavy minutes will generate a lot of negative EBPM
- Intentionally tanking will lead to a lot of negative EBPM
- Trading a good player for picks also leads to negative EBPM

We account for some of these like doing things like assigning a value to future first round and second round picks. But these will always be lower than that of a star player (and in the case of second round picks, may actually have negative value if an executive is bad at drafting.)

### Visualizing results

In getting some results up and out I opted for a static site generator. Creating a full-stack website would be fun but I'm more interested in the data and analysis for this rather than digging into the infrastructure.

I know Jekyll is falling behind Hugo these days in popularity as a static site generator. But Jekyll is still so much simpler than Hugo to set up and get going. I used it to generate pages for each executive in the data I collected. I also used Chart.js to generate some interactive visualizations. I took a long look at D3 but I don't really like the way its gone with Observable (with all due respect to the folks using Observable) it the attempt to emulate Juptyer/R Markdown style notebooks. It seems that a lot of d3 examples got nuked and are no longer available. Chart.js was a lot easier to get up and running with.

I've begun to publish my results [here](https://www.dario.technology/the-grunfeld)





