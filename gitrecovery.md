---
layout: post
title:  Git recovery
date: 2019-09-01 21:00:59
categories: [terminal, shell]
tags: [git, shell]
toc: true
published: true
---

A smaller reminder. If you rebase/squash, and reset hard, you will lose your important addition; use git reflog to recover. The commit "Important addition." was at HEAD@{3}.

```sh
git add && git commit -am "Important addition."
git rebase -i HEAD~3 # did a squash
git reset --hard HEAD~2
git reflog
git reset --hard HEAD@{3}
```

