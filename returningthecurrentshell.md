---
layout: post
title:  Returning the current shell
date: 2019-09-23 08:00:28
categories: [terminal, shell]
tags: [bash]
toc: true
published: true
---

I always used `echo $SHELL` to return the current shell. Going over some studying material I rediscovered echo $0. This positional parameter, `$0`, assigns the current shell into that shell variable.
Try it:
```sh
echo $0 #Result bash
```
