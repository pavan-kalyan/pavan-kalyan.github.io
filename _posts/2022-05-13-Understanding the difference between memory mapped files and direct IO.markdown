---
layout: post
title:  "Understanding The Difference Between Memory Mapped Files And Direct IO"
date:   2022-05-13 00:00:00 +0530
excerpt: ""
author: Pavan Kalyan Damalapati
published: false
tags: [db, storage, linux]
---


I recently came across this research paper from CMU's database group that talks about the performance impact of using memory mapped files for databases.
Although the paper was very interesting, I did not fully understand a few aspects of the comparison. I felt that there was some prerequisite that I was missing which was blocking me from grasping the entire picture.
The paper compares workload performance for O_DIRECT and mmap. In this comparison, it's demonstrated that using mmap leads to a lot of page evictions resulting in poor performance due to (i) TLB shootdowns, (ii) Page table contention, (iii) Single threaded page eviction
A few questions I had after reading the paper:
- Why are TLB shootdowns not possible when using O_DIRECT? Aren't all pages used by the process mentioned in the page table and also cached in the TLB?

To understand the paper thoroughly, a lot of background knowledge regarding the way we read file data needs to be present.
This blog post attempts to provide the background knowledge to understand why O_DIRECT is supposedly better than using memory mapped files for larger than memory database workloads.




