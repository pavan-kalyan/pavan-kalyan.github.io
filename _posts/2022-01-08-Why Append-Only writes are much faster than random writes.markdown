---
layout: post
title:  "Why append-only writes are much faster than random writes"
excerpt: "The common design pattern found in high write-throughput databases"
date:   2022-01-08 1:00:00 +0530 
author: Pavan Kalyan Damalapati
tags: [db]
---


In the last decade, many database systems utilize log structured data structures to support high write throughput like RocksDB, LevelDB etc.

A fundamental property of these data structures is that they are append-only. 
This is mentioned online in mutliple places, but usually they don't explain further. I did not immediately understand why a data structure being append-only automatically made it better for writing.

After reading [DDIA][ddia] [1] and looking it up, I understood that it has entirely to do with the underlying storage. 
There are 2 main types of disk storage used now-a-days. On both types, append-only writes are entirely **sequential writes** as opposed to **random writes**.

#### Magnetic Disk Drives (HDDs):

On HDDs, we have a read-write head that seeks to the specific locations and writes. For random writes, the head needs to do a lot more seeking. But for sequential writes, this seek time is reduced drastically. This directly results in a higher write througput as I/O bandwidth is not being wasted for unnecessary operations.


#### Solid State Drives (SSDs):

For SSDs as well, log structured data structures like [LSM-trees][lsm] achieve high write throughput. But this is for entirely different reasons than HDDs.
Unlike HDDs, most SSDs do not support incrementally updating a page on the disk. Due to the way flash works, before we update a page, we need to read it, update the value, erase the entire block and write the updated page. Each page is around 4-8 kilobytes in size. This phenomena is called [Write Amplification](wa) [2], when one logical write or update causes multiple writes on the underlying storage system. This [article](ssd) [3] goes in-depth explaining why this limitation of flash storage exists.

Append-Only helps here because it allows the database to fill in a page completely with rows of data and write it to SSD at once. This reduces write amplification because all the new writes/appends go to a single page on disk reducing the impact of an Erase-Write cycle.

<br />
To conclude, writing is an expensive operation when compared to reading. The best we can do is to use append-only as it's the simplest write possible due to how our current storage systems work. This is where traditional B+Trees don't hold up and newer data structures like LSM-trees and their variations are centered around append-only to support higher write throughputs.


### References:
- [1] [https://dataintensive.net/][ddia]
- [2] [https://en.wikipedia.org/wiki/Write_amplification][wa]
- [3] [https://codecapsule.com/2014/02/12/coding-for-ssds-part-3-pages-blocks-and-the-flash-translation-layer/][ssd]

[ddia]: https://dataintensive.net/
[lsm]: https://en.wikipedia.org/wiki/Log-structured_merge-tree
[wa]: https://en.wikipedia.org/wiki/Write_amplification
[ssd]: https://codecapsule.com/2014/02/12/coding-for-ssds-part-3-pages-blocks-and-the-flash-translation-layer/