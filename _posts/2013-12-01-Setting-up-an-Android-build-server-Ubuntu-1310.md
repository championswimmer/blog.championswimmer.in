---
layout: post
title: "Setting up an Android build server (Ubuntu 13.10)"
description: "Setting up an Android build server (Ubuntu 13.10)"
headline: "Setting up an Android build server (Ubuntu 13.10)"
modified: 2015-06-10 07:51:27 +0530
category: [Linux]
tags: [linux, android, aosp, building, ubuntu]
imagefeature: 
mathjax: 
chart: 
comments: true
featured: true
share: true
---
Per se this is not a guide to set up a brand new Android buildbox. (And no I am definitely not going to go through continuous integration setup etc)
I have an old box at home, that I use for building Android ROMs. The HDD just borked up today, so I am setting it up from scratch again. Thought why not write down the steps.

This time around I am setting up Ubuntu 13.10 to see what things have changed, but seriously to set up a server, I strongly suggest using the latest LTS (12.04.3 as of now). During the installation you’ll need a head. i.e. a keyboard and a monitor connected. Once installation is done, you can keep the box headless.

Do the regular drill. Use unetbootin or LiLi USB Creator or the Startup Disk Creator from inside a graphical installation of Ubuntu to create a live USB. Plug that in, and start installing it.
I have two HDD’s in my box. If you also have two disks, create a 100GB ext4 partition on the disk on which you will NOT install the OS to house the out folder of the android build system. Having the source and the out folder on separate drives significantly lowers build times. If you have one SSD, put the out directory on SSD, if you have two SSDs well do I need to say ?

While installing, Ubuntu provides you with the option to select a few pre-packaged bundles.
OpenSSH -> enable it   +
DNS Server -> enable it if your server would be accessed over internet without a router in between   +
LAMP -> install it so that you can easily download the built files over http etc
Other packages are as per your choice (depends on what other purposes you might need the server for) +

Now let the installation get over. Enter your name and hostname as per your liking. Reboot the box after installation is over.

Beyond this step, the need of a head is over. Once after the server has booted back up, run

```bash
ifconfig  
```

to find out the local ip address of your server. Mine happens to be 192.168.1.108. To connect to your box from your laptop/tablet/PC that is on the same network (your home LAN or WLAN) just enter the following command

```bash
ssh 192.168.1.108 -l championswimmer   
```

replace ‘championswimmer’ with the username which you set up on the server during installation.

Once you have logged in to the machine, my personal recommendation is to FIRST install aptitude and then perform all packag manager jobs using aptitude instead of apt-get.
Anyway moving on, let’s first get the essential packages needed to build android.

Very first is java-6

```bash
sudo add-apt-repository ppa:webupd8team/java   
sudo apt-get update && sudo apt-get install oracle-java6-installer   
```

Now that we have java, lets install all the other packages that we will need

```bash
sudo apt-get install git-core gnupg flex bison gperf build-essential \    
  zip curl zlib1g-dev libc6-dev lib32ncurses5-dev \     
  lib32z1 lib32ncurses5 lib32bz2-1.0 \     
  x11proto-core-dev libx11-dev lib32readline-gplv2-dev lib32z-dev \     
  libgl1-mesa-dev g++-multilib mingw32 tofrodos python-markdown \    
  libxml2-utils xsltproc    
```

We also need to create a couple of libGL symlinks

```bash
sudo ln -s /usr/lib/i386-linux-gnu/mesa/libGL.so.1 /usr/lib/i386-linux-gnu/libGL.so    
sudo ln -s /usr/lib32/mesa/libGL.so.1 /usr/lib32/mesa/libGL.so    
```

Okay then, as of now, your server is ready to blow the bits away of an Android build. But, let’s do another little thing. Setting up ccache. Ccache will drastically improve build times when you make incremental builds. C sources that are same as when you compiled last time are not going to be compiled again.
Add the following lines to your .profile or .bashrc file

```bash
export USE_CCACHE=1   
export CCACHE_DIR="/path/to/a/place/that/has/~60GB/free/space"   
```

Now you are almost all set to start with the Android sources. First we will need repo. Repo is the tool that downloads all android sources and helps committing changes back to Android’s code review if you want to add something to Android :)

```bash
mkdir -p  ~/bin    
curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo    
chmod a+x ~/bin/repo
```
And now is the perfect time to restart your server.

Now let’s come back to actual setting up of sources and compilation. I am not explaining this part, just writing down the commands

```bash    
mkdir -p ~/android/aosp      
cd ~/android/aosp     
repo init -u https://android.googlesource.com/platform/manifest -b android-4.4_r1    
repo sync -j8    
. build/envsetup.sh && brunch full_mako-userdebug     
```
