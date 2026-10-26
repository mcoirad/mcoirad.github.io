---
title: "Thoughts on Tableau and the Hyper Database"
layout: post
date: 2026-10-26 00:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- data engineering
- software development
- startups
- research
- data science
star: true
category: blog
projects: false
author: dario
description: "Why Tableau Cloud becomes frustrating when it is treated as production data infrastructure, from observability gaps to extract refreshes and version control."
---

_More like complaints about Tableau Cloud._

Tableau is not exactly the gold standard for data reporting, but it is certainly a workhorse. Just about every analyst has heard of it, and most know how to use it. Those two facts are probably its major selling points at this point.

Anyone who has dealt with Tableau support, tried to achieve anything reasonably complex, or been responsible for paying for the subscriptions has probably wished they were using something else at some point.

I've been in all three camps, and I have to report that my experience has not been good. Yes, it is clunky as all hell. But this post is not even about that. It is about the fact that Tableau Cloud is not a reliable piece of data architecture.

_It's also to advertise the fact that I -- with the help of coworkers at my current job -- created a Tableau mod basically that optimizes extract refreshes for Redshift connections in Tableau -- [check it out!](https://github.com/mcoirad/tableau-hyper-refresher)_

## Lack of observability

You get a Jobs page that, in theory, tells you which tasks are failing or passing. But the results do not always contain the name of, or a link to, the content or task that failed. Sometimes all you learn is that a task failed.

The Metadata API does not always reflect the most recent state, either. Recent workbooks and data sources may not be visible through the API, spawning orphaned job results. An October 26 Tableau Cloud update somewhat improved the presentation without fixing the metadata problem. It also caused a major outage, which I discuss below.

Even when you can identify what failed, the best you get is an error message—there are no logs. If you care about logs, you need to use Tableau Server.

## Tableau uses a generic PostgreSQL driver to connect to Redshift

In recent updates, Tableau introduced a dedicated Redshift driver, which mostly fixes the issue I am about to describe. But I believe Tableau also uses a generic PostgreSQL driver as a fallback, without telling you when it does so.

If you have a multi-node Redshift setup and look closely, you will often see your leader node running very hot while the other nodes remain underutilized. I believe this is caused by two things, although I cannot confirm them because of the aforementioned lack of observability:

- Sometimes, for whatever reason, Tableau uses the generic PostgreSQL driver, which does not parallelize the results.
- Even with the Redshift-specific driver, compute nodes send everything through the leader node.

In either scenario, you get a performance bottleneck. If you have many report and data-source refreshes, they will clog up your system while using only a fraction of the available resources.

[To get around that, I had to implement my own Tableau–Redshift connector that fully parallelizes data transfer from Redshift to Tableau data sources.](https://github.com/mcoirad/tableau-hyper-refresher)

## Incremental refreshes are a joke

Tableau's incremental refreshes are append-only, other than time-window-based delete-and-insert functionality. That means they work for only a fraction of use cases.

[For this reason, I implemented UPSERT functionality in my Tableau Hyper refresh process.](https://github.com/mcoirad/tableau-hyper-refresher#configuration)

## Hyper is a memory demon

The backend for Tableau extracts is Hyper, an in-memory-optimized analytical database. And boy, does it like memory.

Although Hyper does not require the entire dataset to fit in RAM, it aggressively uses available memory to accelerate processing. [Tableau's documentation notes that Hyper processes can consume up to 80% of system memory.](https://tableau.github.io/hyper-db/docs/hyper-api/hyper_process/)

This makes working with large extracts locally—or running multiple concurrent Hyper processes—particularly fun. If you are building your own extract pipeline, you had better plan your memory allocation and job concurrency accordingly. I imagine that this also contributes to the price of Tableau Cloud, given all the compute they have to spin up.

## Major outages are not always reported on the Tableau Status page

A recent Tableau Cloud update took out most of our reporting for four days, followed by additional recovery time. Worse, it broke our filters and resulted in a major information-security incident.

Our root-cause analysis identified some things that could have been designed more defensively on our end, but the outage ultimately started with Tableau. Unfortunately, we had no way to detect the issue other than reports from end users. Tableau confirmed that it was a bug in the update and that other users were experiencing it, but it was not reported on the official status or community pages.

This has left us needing to write smoke tests for our reporting—something else to maintain on top of software for which we are already paying a good deal of money.

## Poor to non-functional version control

Expecting to pass all changes to Tableau reports through Git is probably asking too much, although you can do that with other reporting tools. At a minimum, I would like to be able to restore previous versions of data sources and workbooks through the UI. You can do that for workbooks, but not for data sources.

[As I found out, changing a connection type—for example, switching a data warehouse from Redshift to Snowflake—also corrupts the past versions stored in Tableau Cloud.](https://help.tableau.com/current/pro/desktop/en-gb/qs_revision_history.htm#data-changes-and-deletions) You cannot undo a connection-type change by restoring a previous version, so you had better make your own backups before a migration like that.

## TL;DR

Tableau is a self-service analytics platform that becomes frustrating when treated as a production data-infrastructure component. It is designed to make building and sharing reports easy, not to meet the reliability and observability standards that engineers expect from production systems.

Yet Tableau provides just enough functionality to encourage teams to build critical reporting infrastructure around it without offering the tools necessary to maintain that infrastructure properly. It is a great tool to start a team or company with—as long as you plan to leave the team after it grows.
