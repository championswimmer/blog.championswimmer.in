---
layout: post
title: "Using NVIDIA Optimus switchable graphics with GTX 860M on Ubuntu 15.04"
description: "Getting switchable graphics to work in Ubuntu"
headline: "Using NVIDIA Optimus switchable graphics with GTX 860M on Ubuntu 15.04"
modified: 2015-11-30 06:05:06 +0530
category: [Linux, Ubuntu]
tags: [linux, ubuntu, nvidia, bumblebee, primus, optimus]
imagefeature:
mathjax:
chart:
comments: true
featured: false
---
Right since NVIDIA named their switchable graphics solution as _Optimus_ the whole graphics scene on Linux has been a veritable bedlam of Transformer names.

I have been a fan of Bumblebee from the early stages of it's development, and on my earlier Lenovo Z580 having a GT 740M, it was my goto solution and worked seamlessly right from Ubuntu 12.04 to 14.04.

Times have changed. I have a new laptop now - A Lenovo Y50-70 that has the GTX 860M. And now another project has gained momentum called 'NVIDIA Prime'.

After wrestling with Bumblebee and Primus for over a week, I finally gave up because there was no stable solution in sight, and the closest I had come to was to manually unload and load the nvidia and bbswitch modules every time I wanted to use the NVIDIA GPU. Without the ease of having `optirun glxgears` available on my shell, without having to do multiple modprobe-fu, I considered there was little benefit of using Bumblebee.

So I set off on the other path to use NVIDIA Prime instead. Using **nvidia-340-updates** driver from the xord-edgers ppa, as of 30th Nov, 2015, this works perfectly. So let's go through the steps to make it work.

First let's do away with bumblebee if you've installed it

```bash
sudo apt-get remove --purge bumblebee* primus nvidia*
```
Perform a reboot here, so your system safely configures itself to run on Intel/mesa.

Let's add edgers' repo and install nvidia-340.  

```bash
sudo add-apt-repository ppa:xorg-edgers/ppa
sudo apt-get update
sudo apt-get install nvidia-340-updates
```
Again, it is safe to perform a reboot here.

Let's get the prime and settings packages now.  

```bash
sudo apt-get install nvidia-settings nvidia-prime
```

Now before we reboot, we need to make sure of one thing, that is nvidia is loaded before bbswitch always. For that, edit `/etc/modules` and add these two lines  

```
nvidia-304-updates
bbswitch
```

Now reboot your system. If all has gone right, you can switch between NVIDIA and Intel using the `nvidia-settings` configuration utility. **You have to logout and login every time you switch graphics cards. Also unlike optirun/primusrun, when you have enabled NVIDIA, everything works with NVIDIA and similarly for Intel**.
