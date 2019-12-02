---
layout: post
title:  Python at night
date: 2019-08-14 00:00:00
categories: [computer, languages]
tags: [python]
toc: true
published: true
---

Working on some python classes. Feels like I been MIA for far too long. 
Why am I writing this site's "UI" in python and not in node.js/JavaScript? Remember from my last post? I was getting rusty in python so I decided ... who am I kidding? I am terrified of js so I am writing my scripts in python.

Here is a snippet.
<!--more-->

```python
class NewPost:
    def __init__(self):
        print("Create a new post.")


class Clean:
    def __init__(self):
        print("Clean up site images.")


class Edit:
    def __init__(self):
        print("Edit latest post.")


class Build:
    def __init__(self):
        print("Build and copy into deployment folder.")


class Unsplash:
    def __init__(self):
        print("Download unsplash images.")


    parser.add_argument('--newpost', action="store_true",
                        help='Create a new post.', default=False)
    parser.add_argument('--clean', action="store_true",
                        help='Clean up weekly images.', default=False)
    parser.add_argument('--edit', action="store_true",
                        help='Edit latest post.', default=False)
    parser.add_argument('--build', action="store_true",
                        help='Build and copy into deployment folder.', default=False)
    parser.add_argument('--upload', action="store_true",
                        help='Upload to GitHub.', default=False)
    parser.add_argument('--image', action="store_true",
                        help='Download unsplash image.', default=False)
    parser.add_argument('--watermark', action="store_true",
                        help='Adding watermark to unsplash image.', default=False)
    parser.add_argument('--imageformat', action="store_true",
                        help='none replacement.', default=False)

```

# The python script
I'm 170 lines into the script; it is still a work in progress. Once I am done, if there is such a thing as done, I will upload it to GitHub.

# A bit about the bash script
The bash script is broken into three files: variables, functions and main; also it has over 600 lines. I transferred the python way of thinking over to bash; or should I say the OOP style over to bash?


