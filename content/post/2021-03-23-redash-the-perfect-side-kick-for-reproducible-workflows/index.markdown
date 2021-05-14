---
title: 'The perfect analyst side-kick'
author: Michael Bane
date: '2021-03-23'
slug: []
categories:
  - R
  - Python
  - SQL
  - Redash
  - Workflow
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

I love reproducible R or Python based work-flows and everything that goes with that. The satisfaction of a crafting a lean one-liner for that tricky problem is hard to surpass in the typical day in the life of an analyst. RStudio and Jupyter for example are amazing tools and improving all the time, but imo they are still significantly lacking features for heavy SQL-based workflows.

Its so bad that for years i stopped working reproducible. Slowly over time, i became a slave to the SQL editor, downloading results, reformatting in excel and sharing though email.

Sure, i hear you, there is a nice tab for setting up [connections](https://support.rstudio.com/hc/en-us/articles/115010915687-Using-RStudio-Connections) there now, and you can even run queries and [preview results](https://blog.rstudio.com/2018/10/02/rstudio-1-2-preview-sql/) in your IDE. That's nice, but if your day-day involves a lot of exploratory SQL iteration, your going to get frustrated with those tools very quickly. Connecting to a DB and previewing a SQL query output are great if you already know your schema and have your query ready to go. But when you don't, your going to want to be working as efficiently as possible to overcome those challenges.

And lets not forget, it can be a real pain setting up connections to your databases through R and Python. This is especially true for beginners, or if your working with ancient legacy databases, or if your on an OS your not familiar with, or if your admins have put wacky security measures in the way, all of which are very common things in the real world.

If only there was a way to overcome some of these problems 🤔. As someone that loves the reproducible analytics framework but also spends a (probably much larger) portion of their time querying databases, how do you get the best of both worlds? This is where the open source Redash project can really tighten up your work-flow.

## What is Redash

Redash is an open source (although [recently acquired by Databricks](https://blog.redash.io/redash-joins-databricks/)) server-based platform for writing, executing, managing, and serving the results of SQL queries. For more than that, best to check out the [actual web-site](https://redash.io/) but the stuff that matters for this discussion can be summarized below.

-   A central location to set-up your connections (drivers pre-installed) to your data-sources with a GUI to help you establish those [connections](https://redash.io/integrations/).

-   A fully featured [query editor](https://redash.io/help/user-guide/querying/writing-queries), including visualization builder on top of query results.

-   Functionality to 'save' queries, and have them be re-run on a [schedule](https://redash.io/help/user-guide/querying/scheduling-a-query) of your choosing.

-   An [API](https://redash.io/help/user-guide/integrations-and-api/api), for accessing the results of saved queries.

I know the word 'server' can be scary to a lot of analysts out there but it doesn't need to be. Redash provides an easy docker-based deployment option that you can use to get it up and running, either on a server or locally (a server is better though). Ultimately, once you have run the [setup script](https://redash.io/help/open-source/setup#docker) you can access the tool from a browser, in much the same way you can access Jupyter Notebooks or the RStudio Server IDE through a browser.

First thing you will want to do is set up some connections. Redash provides a GUI for this, with all the necessary drivers pre-installed and configured. You just need to provide the credentials required to set up your connection, and your good to start querying. Personally, i have found this process much easier than trying to set-up connections in R or Python, especially in some of the edge cases i mentioned before.

Next, you want to play around with the SQL editor. Write a query, maybe build a visual around it, but make sure you save and publish your query. You will be able to go back to that query and re-run it if you need to in the future, and you can even schedule its execution to keep it up to date.

Even in isolation, your already improving your work-flow. As analysts, we do a lot of things over and over again, and being able to build a portfolio of managed queries to draw upon will increase your efficiency. Secondly, your building a central source of truth. Through [public links](https://redash.io/help/user-guide/dashboards/sharing-dashboards), you can start referencing and sharing links to your outputs directly, rather than snapshot's in static files being shared via spreadsheets and email for example.

## The Secret Sauce

This is all great but how do we bring it back into the reproducible work-flow. The answer is very simple. The most useful function of Redash is its API functionality which provides endpoints to access the results of your queries. This makes it very easy for R or Python to ingest your data through the basic csv and json parsing routines.

Just navigate to your saved query, click 'options' and then 'Show API Key'. You can then copy the path to your data in CSV or JSON format. You can then easily load the data with a command like the following.


```r
#R example.
data <- read.csv('redash_url/api/queries/query_id/results.csv?api_key=your_api_key')
```


```python
#Python example with Pandas for simplicity.
import pandas as pd
data = pd.read_csv('redash_url/api/queries/query_id/results.csv?api_key=your_api_key')
```

With this methodology there is no need to connect to a database and query the data every time you log into your R or Python session. You don't even need to install and configure the tools typically associated with connecting to databases. Once you have read your data, your free to interact with it however you see fit.

Bringing it all together, with this combination of tools;

1.  you are writing queries efficiently with a fully featured SQL editor,
2.  your building out a portfolio of queries to reference back too, improving your reproducibility,
3.  your able schedule execution and keep results up to date, and share direct links to that data,
4.  you can build simple visuals, pivot tables and dashboards on top of those queries,
5.  and finally, you can easily import to your scripting language of choice for anything more advanced.
