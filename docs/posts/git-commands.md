--- 
date: 2020-04-13
slug: git-commands
title: Useful Git Commands

authors:
  - kono94 
categories:
  - Programming
  - Tooling
tags:
  - Git
  - Gitlab
---

Just a collection of useful git commands.

```
git commit --amend --no-edit
```

<!-- more -->

The ``no-edit`` flag will allow you to make the amendment to your commit without changing its commit message.
You need to force push if already pushed to a remote.

```
git push -f origin master
```
