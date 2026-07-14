---
title: "Migrating my basketball stats workflow to the cloud"
layout: post
date: 2025-04-09 22:44
image: /assets/images/markdown.jpg
headerImage: false
tag:
- data engineering
- data science
star: true
category: blog
projects: true
project: nba_executives
author: dario
description: "Migrating a basketball statistics workflow from a local setup to the cloud, including pipeline architecture, automation, and deployment tradeoffs."
---

## Moving to Google Cloud

With the NBA season finishing up I returned to my basketball stats project.
So far I've been running it locally or in Cloud IDEs such as Cloud9. Cloud9 sunset awhile ago so I switched over to the Google Cloud IDE after experimenting with Github CodeSpaces. Both seem pretty comparable but I went with the Google Cloud IDE since I was aiming to use that cloud provider anyways.

I chose Google Cloud over AWS since it is better for analytics and a bit more lightweight for single-person projects like I am working on. I use AWS much more professionally but it is always good to branch out lol.

My goal this around was to get my whole workflow which downloads and uses data locally to fully use the cloud. The architecture I settled on iteratively was:

- Data stored in Bigquery
- ETL, basic analysis, static site generation in Cloud Run Functions & Jobs
- Orchestration with Workflows
- Site publishing with Jekyll + Github Actions (already done)

![Basketball Stats Pipeline Architecture](/assets/images/nba/bk_pipeline.png)

Nothing too fancy but pretty well optimized for cost, costs less than a quarter to run. For now am running it weekly.

### Next steps

Now there are a few different directions to go in. A big fun goal would be being able to produce automated trade grades on trades as they happen. To do that I would need to do a few things:

- Player projections (i.e. what total BPM will a player produce in the next couple of years?)
- Team projections (i.e. will this team be good or bad, relevant for values of picks)
- Team strategy (i.e. is the team tanking -- in which being bad would be a good thing)
- Salaries (i.e. does a move save money, there is some value in that)

None of these pieces needs to be super in-depth or rigorous. But we'd like to have these data points for the trade grade model.

For now will be thinking these through while continue to iterate on the current pipeline. I am also thinking about a GraphQL back-end for the site rather than a statically generated site. But I haven't decided yet.

[Site is currently accessible here](https://www.dario.technology/the-grunfeld)
