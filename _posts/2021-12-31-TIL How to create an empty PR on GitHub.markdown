---
layout: post
title:  "TIL: How to create an empty Pull Request on GitHub"
date:   2021-12-31 00:52:08 +0530
author: Pavan Kalyan Damalapati
tags: [til, git]
---

#### TLDR:
```shell
git commit --allow-empty -m "empty commit to create PR to start discussion"
```

I sometimes want to create a Pull Request(PR) to start a discussion without any code. Other times, I want to create a PR to act as the base for upcoming changes to be merged. But GitHub doesn't allow creating a new PR if there are no differences between the branches.


I found the solution in this StackOverflow [answer](https://stackoverflow.com/questions/46577500/why-cant-i-create-an-empty-pull-request-for-discussion-prior-to-developing-chan). Git allows us to create an empty commit via the flag `--allow-empty` [ref](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt---allow-empty).

The following command is required on the branch from which you expect to create the PR

```shell
git commit --allow-empty -m "empty commit to create PR to start discussion"
```

After pushing that commit to the remote branch, I was was able to create the PR.
