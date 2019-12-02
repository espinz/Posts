---
layout: post
title:  Using ncurses
date: 2019-08-04 15:00:00
categories: [terminal, shell]
tags: [shell, bash, scripting]
toc: true
published: true
---

I got tired of typing in my categories. So, I created a menu based on my categories with ncurses. This way I can select a category from a menu. Cool!

<!--more-->

# ncurses for bash — dialog

Here is the case statement.


```sh
case "$menu_choices" in
  1)
    menu_choices=`dialog --stdout --menu 'Please pick a category' 0 0 0 1 hardware 2 languages`
    if [ $menu_choices == 1 ]; then
        category_choices="computer, hardware"
    else
        category_choices="computer, languages"
    fi
    ;;
  2)
    menu_choices=`dialog --stdout --menu 'Please pick a category' 0 0 0 1 linux 2 macos 3 windows`
    if [ $menu_choices == 1 ]; then
        category_choices="os, linux"
    elif [ $menu_choices == 2 ]; then
        category_choices="os, macos"
    else
        category_choices="os, windows"
    fi
    ;;
  3)
    menu_choices=`dialog --stdout --menu 'Please pick a category' 0 0 0 1 shell`
    if [ $menu_choices == 1 ]; then
        category_choices="terminal, shell"
    fi
    ;;
  4)
    menu_choices=`dialog --stdout --menu 'Please pick a category' 0 0 0 1 thought`
    if [ $menu_choices == 1 ]; then
        category_choices="unique, thought"
    fi
    ;;
  5)
    menu_choices=`dialog --stdout --menu 'Please pick a category' 0 0 0 1 updates`
    if [ $menu_choices == 1 ]; then
        category_choices="website, updates"
    fi
    ;;
  *)
    echo "exit"
    ;;
esac

```

