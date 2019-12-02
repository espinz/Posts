---
layout: post
title:  Battle of the Hello Worlds
date: 2019-05-23 
categories: [computer, languages]
tags: [bash, c, cpp, swift, js, python]
---

Which computer langauge runs the fastest?

# Lets ask the computer. 
> Who is the fastest of them all?

```sh
for i in $(find . ! -name "*.js" -print | sort);\
do echo "From: $i" >> ../fastest; {time ./$i;} 2>> ../fastest; echo " "; \
done && i=jsApp.js; echo "From: $i" >> ../fastest;{time node ./$i} 2>> ../fastest; cat ../fastest
```

<!-- more -->
# This is what the computer said.
> From fastest to slowest: {Bash, C, C++} Swift compiled, Python, Swift as script, and JavaScript. 

```sh
From: .
From: ./bashScript.sh
./$i  0.00s user 0.00s system 71% cpu 0.004 total
From: ./cPlus
./$i  0.00s user 0.00s system 56% cpu 0.004 total
From: ./cProgram
./$i  0.00s user 0.00s system 56% cpu 0.004 total
From: ./pythonProgram.py
./$i  0.03s user 0.01s system 85% cpu 0.048 total
From: ./swiftApp
./$i  0.01s user 0.00s system 80% cpu 0.014 total
From: ./swiftApp.swift
./$i  0.03s user 0.02s system 85% cpu 0.064 total
From: jsApp.js
node ./$i  0.08s user 0.02s system 92% cpu 0.105 total
```
# Bonus
Who spends the most cpu time xx%?

> From least to most talkative: {C, C++}, Bash, Swift compiled, {Python, Swift as Script} and JavaScript. 



# Code
{% gist c53e5ea2915b9118ca9ca63acece4f38 %}
{% gist 8af813ef75535a8093de2d3741cf73da %}
{% gist ddbe772b10d17d27fdbaede628c06ee9 %}
{% gist b4cd51b2dde5ca82bdd2cbfc70505847 %}
{% gist 0bb073f90c488860a5ee7b9423467201 %}
{% gist ac88cc549ba027ad30fe28c6c504b889 %}

> Photo by Taylor Vick
