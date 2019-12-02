---
layout: post
title:  Github remote configuration
date: 2019-06-26 18:00:00
categories: [terminal, shell]
tags: [command, git]
toc: true
published: true
---

Here are some git commands I seem to forget. Having them near by helps. 

<!--more-->

```sh
git checkout -b feature OR git branch feature
# list branches
git branch

# push to remote
git push --set-upstream origin feature

# delete remote branch
git push origin --delete feature/login

# delete master branch after push
git push origin +master

# delete local branch (merged branch)
git branch -d

# delete local branch (unmerged branch)
git branch -D

# view remote branches
git remote show origin
git branch -r
```


## Delete remote branch
```
git push origin --delete branchName
```


## Git ignore
-  if it is not working

```sh
rm -r --cached .
git add .
git commit -m ".gitignore is now working"
```

## View Remote Files
```
git ls-tree -r --name-only <commit> (where instead of <commit> there can be <branch>).
```
## Gist

```sh
git init helloWorld
echo "Hello World" > Hello.sh
git add .
git commit -m "init"
gist -p -d "test" Hello.sh
git remote add origin git@github.com:ID.git
git pull
git reset --hard origin/master
git push -u origin master
```
