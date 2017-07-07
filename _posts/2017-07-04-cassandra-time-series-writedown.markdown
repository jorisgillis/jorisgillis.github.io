---
layout: post
title:  "Cassandra for Time Series - Blogpost"
date:   2017-07-04 12:01:01 +0200
categories: Apache Cassandra
---

## Introduction

I'm a software developer at [TrendMiner][trendminer], where I focus on
the scalability of the TrendMiner software.  TrendMiner is a modelling
free Industrial Analytics Platform to analyze, monitor and predict
asset and process performance.  Which implies that we handle a lot of
time series data.  The goal of this blog post (and talk during the
BigData.be meetup on the 28th of June, for which I already shared the
slides [previous post][slides]), is to explore how to store and query
time series stored in the [Apache Cassandra NoSQL datastore][cassandra].  In this
blogpost I will do a quick write down of the presentations contents,
to accompany the slides.



## Problem statement

### Time series ##

In the (chemical) process industry, all components in a plant are
fitted with sensors.  A sensor produces a **time series**: a sequence of
timestamped values.  A rudimentary time series in Java would be defined as:

~~~ java
List<Pair<Long, Double>>
~~~

A typical plant houses between a thousand to a couple of million
sensors (i.e., time series).  Most of these time series will be
semi-equidistant with a resolution between 5 minutes and 1 second (or
less).  A time series is semi-equidistant if it (roughly) produces a
value every x seconds.  For example,

~~~
(0, 1); (60, 2); (119, 3); (181, 4); ...
~~~

is a semi-equidistant time series with time resolution 1
minute. Typically, for each time series a ten year history is kept.


### Complex Analyses ##

The power of TrendMiner lies in the search, analysis and prediction
algorithms. The complexity of the algorithms necessitates fast data
ingestion. In other words, to facilitate interactive software, time
series need to be loaded into the application as fast as possible.


### Plants across the globe ##

Usually, TrendMiner will not be connected to a single plant, but it
will be hooked up to all plants of a company. This implies a
geographically distributed set up. Consequently, a datastore that
supports geographic distribution of data is a plus.



## Why Cassandra?

Apache Cassandra might not be the first option that comes to mind for
a time series store. There is even a Google Sheet dedicated
to [this topic][timeseriesdbs].


### Time Series Databases

Think for example about KairosDB, Elastic and InfluxDB (to name just a
few datastores). The big advantage of these systems is that they are tuned for 
storing, querying and analysing time series. However, from our perspective
they have two distinct disadvantages:

- **Connectivity**: they only support a HTTP API; transferring large
  sets of data over a HTTP connection, encoded as JSON, is simply too
  slow for our intents.
- **In store analytics**: although these stores offer quite potent query
  languages for analytics purposes, our algorithms are too complex to
  express.


### Relational Databases

On the other hand, there are relational databases, offering a rich,
Turing-complete, query language, with a proven track
record. Consequently, we could express our algorithms in SQL. But
relational databases, e.g., [PostGreSQL][postgresql]



[cassandra]: https://cassandra.apache.org/
[trendminer]: https://www.trendminer.com
[slides]: /apache/cassandra/2017/07/01/cassandra-time-series.html
[timeseriesdbs]:  https://docs.google.com/spreadsheets/d/1sMQe9oOKhMhIVw9WmuCEWdPtAoccJ4a-IuZv4fXDHxM/edit#gid=0
[postgresql]: http://www.postgresql.org

