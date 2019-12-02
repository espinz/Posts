---
layout: post
title:  Different file extensions
date: 2019-07-20 14:00:00
categories: [website, updates]
tags: [hexo, espinz]
toc: true
published: true
---

A clean up script was hanging for "no apparent reason". After reviewing the script it happen to be a difference in file extensions. The script was looking for dot md extensions while the current file being read was a dot markdown. I suppose some error checking would of helped.

I renamed all the dot markdown to dot md files. I thought I had done this already. Oh well, it seems to work now.
