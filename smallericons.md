---
layout: post
title:  Smaller macOS launchpad icons
date: 2019-06-06
categories: [os, macos]
tags: [macOS, launchpad]
---

Making springboard show smaller icons.

``` sh
defaults write com.apple.dock autohide-time-modifier -int 0;killall Dock
defaults write com.apple.dock springboard-rows -int 8
defaults write com.apple.dock springboard-columns -int 8;killall Dock
```

