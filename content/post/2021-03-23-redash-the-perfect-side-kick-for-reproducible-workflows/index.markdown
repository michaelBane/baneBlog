---
title: 'Redash: The perfect analytics side-kick'
author: Michael Bane
date: '2021-03-23'
slug: []
categories:
  - RStudio
  - Redash
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2021-03-23T19:31:07+11:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

I love reproducible R or Python based workflows and everything that goes with that. The satisfaction of a crafting a lean one-liner for that tricky problem you had is hard to surpass in the typical day in the life of an analyst. RStudio and Jupyter are amazing tools and improving all the time, but man do they still suck if exploratory SQL is part of your workflow.

Its so bad that for years i stopped working reproducible. Slowly over time, i became a slave to the SQL editor, downloading results, reformatting in excel and sharing though email.

Sure, i hear you, there is a nice tab for setting up connections there now, and you can even run queries and preview them in your IDE. That's nice, but if your day-day involves a lot of SQL iteration, your going to get frustrated with those tools very quickly. Connecting to a DB and previewing a SQL query output are great if you already know your schemas and have your SQL ready to go, but when you don't, your going to want to be working as efficiently as possible to overcome those challenges.

And lets not forget, it can be a real pain setting up connections to your databases thought scripting tools like R and Python. This is especially true for beginners, or if your working with ancient legacy databases, or if your on an OS your not familiar with, or if your admins have put wacky security measures in the way, all of which are very common things in the real world.

If only there was a way to overcome some of these problems 🤔. As someone that loves the reproducible analytics framework but also spends a (probably much larger) portion of their time querying databases, how do you get the best of both worlds? This is where the open source Redash project can really tighten up your workflow.

## What is Redash

Redash is an open source (although [recently acquired by Databricks](https://blog.redash.io/redash-joins-databricks/)) server-based tool for writing, executing, managing, and serving out the contents of SQL queries. For more than that, best to check out the [actual web-site](https://redash.io/) but the stuff that matters for this discussion can be summarized below.

-   A central location to set-up your connections (drivers pre-installed) to your data-sources with a GUI to help you establish those connections.

-   A fully featured query editor, including visualization builder on top of query results.

-   Functionality to build pivot tables on query outputs (yes sounds small but i use this all the damn time!).

-   Functionality to 'save' queries, and have them be re-run on a schedule of your choosing.

-   An API, for accessing the results of saved queries.

I know the word "server" can be scary to a lot of analysts out there but you don't need to be. Redash provides an easy docker-based deployment option that you can use to get it up and running, either on a server or locally (a server is better though). Ultimately, once you have run the setup script you can access the tool from a browser, in much the same way you can access Notebooks or the RStudio Server IDE.

First think you will want to do is set up some connections. Redash provides a GUI for this, with all the necessary drivers pre-installed and configured. You just need to provide the credentials required to set up your connection, and your good to start querying. Personally, i have found this process much easier than trying to set-up connections in R or Python, especially in some of the edge cases i mentioned previously.

Next, you want to play around with the SQL editor. Write a query, maybe build a visual around it, but make sure you save and publish your query. You will be able to go back to that query and re-run it if you need to in the future, and you can even schedule its execution to keep it up to date.

Even in isolation, your already improving your workflow. As analysts, we do a lot of things over and over again, and being able to build a portfolio of managed canned queries to draw on will increase your efficiency. Secondly, your building a central source of truth. Through "public links", you can start referencing and sharing links to your outputs directly, rather than snapshot's in static files being shared via spreadsheets and email for example.

## The Secret Sauce

This is all great but how do we bring it back into the reproducible workflow. The answer is very simple. The most useful function of Redash is its capacity to provide APIs to the data that your publishing. This makes it very easy for R or Python to ingest your data through the basic csv and json parsing routines.

Just navigate to your saved query, click options and then "Show API Key". You can then copy the path to your data in CSV or JSON format. You can then easily load the data with a command like the following.


```r
#R example.
data <- read.csv('path_to_redash/api/queries/query_id/results.csv?api_key=your_api_key')
```


```python
#Python example with Pandas for simplicity.
import pandas as pd
data = pd.read_csv('path_to_redash/api/queries/query_id/results.csv?api_key=your_api_key')
```
