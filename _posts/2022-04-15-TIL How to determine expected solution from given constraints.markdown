---
layout: post
title:  "TIL: Determine expected solution from given constraints"
date:   2022-04-15 00:00:00 +0530
excerpt: "Quick reference to the complexity and constraint chart with a few tips"
author: Pavan Kalyan Damalapati
tags: [til, cp]
---

|n | Max Time Complexity | Common Name|
|---|:---:|:---:|
|n <= 12| O(n!) |Factorial|
|n <= 25 |O(2<sup>n</sup>)| Exponential|
|n <= 100 | O(n<sup>4</sup>)| Quartic|
|n <= 500  |O(n<sup>3</sup>)| Cubic|
|n <= 10<sup>4</sup>  |O(n<sup>2</sup>)| Quadratic|
|n <= 10<sup>6</sup> |O(n log n)|  Linearithmic|
|n <= 10<sup>8</sup>   |O(n)  |Linear|
|n > 10<sup>8</sup> |O(log n) or O(1) | Logarithmic and Constant respectively|

One trick to figuring out the expected time complexity is to substitute n and check that the result is 10<sup>8</sup>.\\
Put another way: log<sub>10</sub>(n) should output 8.\\
For example: 
log<sub>10</sub>(n!) is only equal to 8 when n is 12.
Similarly, log<sub>10</sub>(n<sup>2</sup>) is only equal to 8 when n is 10<sup>4</sup>.

We consider 10<sup>8</sup> because that is usually the upper bound on the number of elements in most coding contests.

This technique is particularly helpful for more complicated complexity analysis. If you have O(MN log M) as the complexity of your algorithm, you can subsitute N and M (constraints) to determine if output is 10^8 and consequently whether your algorithm is fast enough.

### References

- [https://codeforces.com/blog/entry/21344](https://codeforces.com/blog/entry/21344)
- [https://codeforces.com/blog/entry/21344?#comment-260153](https://codeforces.com/blog/entry/21344?#comment-260153)