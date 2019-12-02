---
layout: post
title:  Python installation
date: 2019-06-26 23:00:00
categories: [computer, languages]
tags: [python]
toc: true
published: true
---

I was having trouble with my python installation. This helped.

## beautifulsoup

```sh
pip3 install beautifulsoup4
brew install python
brew link python3
```
<!--more-->
## Python 3

```sh
brew rm python
brew rm python3
rm -rf /usr/local/opt/python
rm -rf /usr/local/opt/python3
brew prune
brew install python3
brew postinstall python3
```

# Virtual environments

```sh
mkdir ~/.virtualenvs
python3 -m venv ~/.virtualenvs/myvenv
source ~/.virtualenvs/myvenv/bin/activate

deactivate
```

## .zshrc

```sh
export WORKON<sub>HOME</sub>="$HOME/.virtualenvs"
export VIRTUALENVWRAPPER<sub>PYTHON</sub>=/usr/local/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```
