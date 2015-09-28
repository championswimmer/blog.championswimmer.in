---
layout: post
title: "Bash : Use data from two different text files in a for loop"
description: "Bash : Use data from two different text files in a for loop"
headline: "Bash : Use data from two different text files in a for loop"
modified: 2013-08-28 15:22:17 +0530
category: [Bash, Linux, Programming]
tags: [Shell, Bash, Linux, Scripting, Loop, Programming]
imagefeature: 
mathjax: 
chart: 
comments: true
featured: false
---
Sometimes we have data in the format of

```
cat
dog
lion
```
and

```
meow
bark
roar
```

and we need to output in the following format

```
cat meow
dog bark
lion roar
```

If you have faced such a situation already and are not able to figure out how to accomplish it using shell/bash and not going into the murky worlds of python/perl, then here is a handy script at your service

```bash
#!/bin/bash
while IFS='|' read -r i j; do echo "${i} ${j}" >> combined_list.txt; done < <(paste -d '|' animal.txt sound.txt)
```
