---
layout: post
title:  "Intro to Time Series Databases"
excerpt: "What are time series databases? and how can we utilize them?"
date:   2020-02-23 20:00:00 +0530
author: Pavan Kalyan Damalapati
---

This is an intro to Time series databases, this post will be light on technical details since it's only an intro.

**_What is time series data?_**

Time series data is a series of data records, where each record has a time component(timestamp). This time component might indicate the time an event occurred or the time a record was updated.

Compared to traditional data records with a time based field, the time component in a time series data record is critical to the record itself.
Also unlike traditional data records, time series data records tend to be 'append only' because updating records would mean losing the original timestamp of the record.

Time series data is mostly used in day trading algorithms and monitoring software and applications where timestamps are essential.
We at [Hevo Data](https://hevodata.com) use it for monitoring data events being processed through our ETL platform.

**_What are time series databases?_**

Time series databases(TSDBs) are databases that specialize in handling time series data. They are highly optimized in storing, retrieving and transformation of time series data.
Time series data tends to become very large because of its append only nature. TSDBs deal with this large volume of data through various techniques. One technique is to keep all the data points that were inserted within a recent timeframe and later downsample and summarize it into more long term trends.

Time series databases are highly optimized to measure change over time. Here are some of the time based functions provided out of the box with most TSDBs:
- Time Downsampling: In many applications, Time Series data is recorded at very high resolution but is often only needed to be queried at a lower resolution, for example, to populate data in a graph.
- Efficient Comparison Operators: Time Series databases allow you to compare timestamps in different ways with efficiency.
- Automatic Data Vacuuming: Old data tends to lose its usefulness over time and needs to be pruned. TSDBs offer this functionality efficiently.
- Insert/Append Optimized: Due to their mostly append only nature, TSDBs tend to be more efficient and optimized for appending/inserting over updating records compared to traditional DBs.

Replicating this behavior on traditional DBs is possible but very difficult and inefficient.

**_Who are the top players in time series databases?_**

Here are some of the most widely used time series databases:

- [InfluxDB](https://www.influxdata.com/)
- [KDB+](https://kx.com/)
- [Prometheus](https://prometheus.io/)

![Popularity of Time Series Databases](/assets/tsdbs.png){:class="img-responsive"}

**Sources**

- [https://db-engines.com/en/ranking_trend/time+series+dbms](https://db-engines.com/en/ranking_trend/time+series+dbms)
- [https://www.influxdata.com/time-series-database/](https://www.influxdata.com/time-series-database/)
- [https://griddb.net/en/blog/practical-introduction-to-time-series-databases-and-time-series-data/](https://griddb.net/en/blog/practical-introduction-to-time-series-databases-and-time-series-data/)

