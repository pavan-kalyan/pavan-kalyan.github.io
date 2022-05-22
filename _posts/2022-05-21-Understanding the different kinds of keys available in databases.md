---
layout: post
title:  "Understanding the different kinds of keys used in databases"
date:   2022-05-22 00:00:00 +0530
excerpt: "Understanding the difference between primary keys, sort keys, partition keys, etc."
author: Pavan Kalyan Damalapati
tags: [db]
---

Working with different kinds of databases and data warehouses can be confusing as they all provide different kinds of keys but they also name the same keys differently.
So, I worked on understanding and categorizing them based on what they do so that I can immediately recognize what X key provided by database Y actually means.

## Primary Key
The most commonly used and well-known type of key. A primary key is a column or a set of columns that uniquely identifies a tuple in a relational schema.
One of the core concepts of relational schemas is that each table **must** have its own primary key. It's almost always automatically indexed.
Usually supported as an auto-incrementing number column. It's used in all the relational and row-oriented databases like MySQL, PostgreSQL, etc.

The concept of a primary key doesn't exist in column-oriented databases (typically data warehouses), though they usually have some way of defining unique keys.

#### Difference between Primary key and Unique key
They are essentially the same in most databases. Both uniquely identify a tuple. "primary key" is the formal terminology used in relational designs. Whereas unique keys are used more for validation on inserts or updates whereas primary key is usually an auto-incrementing key, so it might not be able to validate business context.
One exception to this is that unique keys in some databases ignore null values when determining if a tuple is unique.

## Foreign key
Foreign keys enforce relations between different tables. Usually, a table might have a foreign key referring to another table. This other table is the parent table.
Inserts and updates would be rejected if the corresponding entity does not already exist in the parent table. Similarly, there are different validations that can be defined for inserts, updates, and especially deletes.


## Partition Key
Partition keys are used to segment the data into separate files/blocks.
In distributed systems, this usually means storing segments of a table on different machines.
Partitioning randomly is a bad idea for query performance because any query might end up needing to scan all the partitions across all machines.
So, usually partitioning is only allowed on date or time columns.
For example, if we partition monthly, we will be able to only scan the blocks that are actually required instead of the entire thing.
Some warehouses also allow partitioning by varchar columns based on a prefix.


So far so good, but the following keys are where there is little consensus on what to call them.

## Sort Key
Sort key defines the list of columns based on which the data is sorted and stored on disk.

The main advantage of sorting the data and storing it on disk is that certain queries would be much faster. Filters, aggregates, and joins would all benefit from this representation. For example, range predicates would allow skipping huge chunks of data because we can prune data that will not fall under the predicate.

This is particularly common in column-oriented databases like Redshift, Firebolt, BigQuery, and Snowflake. Though they are referred to differently in different warehouses. For example, BigQuery refers to sort keys as **clustering keys**, which should not be confused with **cluster keys** (join keys). Similarly, Firebolt calls it **Primary index** which is even more confusing.


## Join key
Join index is a concept used in databases to specify that data in 2 or more relations should be colocated together on the file system.
Also sometimes called a **cluster key** to represent that the data is physically clustered together. Not to be confused with **clustering key** in BigQuery which is about sorting.

Consider A and B with a common column c. If we set the cluster key as c, records of A with c value that matches records of B with the same c value are stored physically together in the same block. This helps join queries become highly optimized as we automatically perform the join on read. But it comes at the cost that simple queries end up taking more time. i.e, simple `select * from A` will have to access a lot more blocks from B just to get records of A.

Firebolt [supports join indexes](https://docs.firebolt.io/using-indexes/using-join-indexes.html) by caching the join results in ram in the form of an index rather than storing the data together on disk.


## Aggregate Key
Set of columns or which we pre-calculate aggregate statistics to serve aggregate queries faster.
Queries like MAX(), MIN(), and COUNT() would benefit from this as a full table scan can be avoided.

Setting an aggregate key can involve also registering an aggregate expression because the index won't be able to serve everything quickly if the input query is unbounded. For example, changing the aggregate function or widely varying the where clauses would not be served by the existing aggregate index and would default to a table scan. Unlike all the other keys, this is very different as it is essentially, a materialized summary table.

Firebolt calls it Aggregate Index. But the functionality probably can be crudely replicated by using a materialized summary table in the form of a [multi-dimensional cube](https://en.wikipedia.org/wiki/OLAP_cube) to support some more aggregate queries rather than the ones purely pre-decided.


### References
- [https://cloud.google.com/bigquery/docs/clustered-tables](https://cloud.google.com/bigquery/docs/clustered-tables)
- [https://en.wikipedia.org/wiki/OLAP_cube](https://en.wikipedia.org/wiki/OLAP_cube)
- [https://docs.firebolt.io/using-indexes/using-join-indexes.html](https://docs.firebolt.io/using-indexes/using-join-indexes.html)

