---
layout: post
title: "Replication to Github Enterprise with Gerrit"
description: "How to use Gerrit replication with Github Enterprise installation"
headline: "Replication to Github Enterprise with Gerrit"
modified: 2016-02-17 00:06:32 +0530
category: [Git, 'Open Source']
tags: [git, gerrit, github, enterprise, ecdsa]
imagefeature:
mathjax:
chart:
comments: true
featured: false
---
Even if you are a gerrit/git sysadmin having done a dozen code review setups already, you might get stumped with the 'reject HostKey' error when trying to replicate from Gerrit to a Github Enterprise instance.

So this error stems from the following reasons -

 1. Gerrit used Eclipse's JGit library to implement Git
 2. Github Enterprise enforces ECDSA SSH v2 for SSH connections

Basically this is an issue about JGit not able to parse hashed known_host file lines
for ECDSA Protocol 2. But we can neigher change the JCh (as we are using compiled)
gerrit, nor can we (or want to) disable ECDSA 2 enforcement in Github Enterprise

So I'll keep it short and jump to straight to how to solve this.

First we need to set an alias of our Github Enterprise server in ssh config.
Edit `~/.ssh/config` and add this

```
Host mygit git.yourserver.com
HostName mygit.yourserver.com
Protocol 2
HostKeyAlgorithms ssh-rsa,ssh-dss
```
Replace git.yourserver.com with your actual server address. **mygit** is also upto you.
Name it whatever you like

In your `$GERRIT_HOME/etc/replication.config` in the url fields change references to
`git@git.yourserver.com` to `git@mgit`

Here is an example from my setup

```
[remote "internal"]
    url = git@mygit:${name}.git
    projects = yu-aosp-internal/*
    push = +refs/*:refs/*
    timeout = 50
    threads = 4
    replicationDelay = 0
    defaultForceUpdate = true
```

Purge your `~/.ssh/known_host` file. You can just delete the *ecdsa* lines.

Run `ssh -T git@mygit` (run it as the same user that runs gerrit - usually it might be
user **gerrit2** ) to add the ssh remote again.

Now restart gerrit.

Your replications should be working now.
