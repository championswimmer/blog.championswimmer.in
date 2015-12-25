---
layout: post
title: "Linux refugee in Windows aylum: A scoop of chocolatey nuget"
description: "A Linux power user adapting to temporary residence on Windows"
headline: "Linux refugee in Windows aylum: A scoop of chocolatey nuget"
modified: 2015-10-20 04:56:29 +0530
category: personal
tags: []
imagefeature: 
mathjax: 
chart: 
comments: true
featured: false
---

I _forgot_ my laptop's charger at my dorm when I came home this weekend, but I had to work on my GSoC project as my contributions were lagging. I had to borrow my Dad's XPS running Windows 8. Fuck. Windows. Without any development tools installed.




So it's not like I don't ever use Windows at all. I have a Windows 7 dual boot set up running on my laptop for the times when I need to twilight as the [Knight of Arkham City](http://google.com/search?q=Batman+Arkham+video+games) or don [the blues](http://chelseafc.com) to give Ferguson's boys a drubbing or [kill some Templars](http://google.com/search?q=Assassins+Creed) in the French/American Revolution. But that's pretty much all that I do on Windows - play games. I do not even have PDF Reader or an Office Suite or any browser other than Internet Explorer on Windows - which should suffice to say how much of anything else I do on Windows.

And here I was supposed to work on my GSoC project which involved

- **Git**; to get the source and commit back
- **Nodejs**; to get bower and gulp to serve the project to localhost to test it out
- A half decent text editor because Notepad is total jackshit
- A commandline that doesnt want arguments with '/' and dir trees with '\' and shows files with 'ls' and  . . you get what I mean ? I want fucking **bash**.

## Package Manager
For a Linux refugee, the first pain point is the obvious absence of a package manager. (I hear Windows 10 **FINALLY** has one). 
It is really such a PITA if you cannot simply get ruby or node or python via `apt-get install nodejs python ruby` 
And as it happens, there are three options in front of you - 
 1. **[NuGet](https://www.nuget.org/)** - The package manager that is now default in Visual Studio as well.
 2. **[Scoop](scoop.sh/)** - My personal favourite and the closest you get to a Linux package manager, and has a repository of almost all known development related packages.
 3. **[Chocolatey](https://chocolatey.org/)** - One of the earliest attempts to get a apt-get like interface on windows.
 
 [Here is a brilliant blog](https://outcoldman.com/en/archive/2014/07/20/scoop/) describing how to get Scoop installed on your Windows machine, plus some tips on how to enable execution policy on Windows Powershell. 

## Git support
I cannot ever stop singing praises for [Github](http://github.com) for all the cool work they do, and one of them is bringing Windows and Git as close to each other as possible. 
The thing is Git was developed by, well none other than **Linus Torvalds**, _the_ guy who made Linux, and so historically, Git (which is possibly the best distributed VCS in the world) has primarily been a relic of the Linux realm.
Github for Windows is great way to bring not just Git to Windows, but via the _**Git Shell**_ that comes with the package, you get a decent `bash`/`zsh` like command line interface too. 

## A more linux-ey CLI
While what Git Shell provides is really nice, if you are weeping silently because you really really want a proper Linux CLI inside Windows (oh dear you, I can feel you man), then there are few tweaks that you can do. 

You can get powershell, and install the Solarized theme, that makes stuff really awesome
```
> scoop install concfg
> concfg import solarized small
> scoop install pshazz
> pshazz init #Run this everytime you open your shell
```

## Best text editors available
I do not want to make it an opinionated advice here, so I'll just list all the options that you have

 1. **Sublime Text** - The most widely used, and has maximum extensions. Pretty fast.
 2. **Atom** - A new javascript-based text editor by Github. Runs in it's own Chromium runtime, so can be slow and bloaty. Might not be suitable for large files. The UI is brilliant, and overall design and experience was the best for me. 
 3. **Brackets** - Adobe's hat in the growing ring of Chromium-runtime based editors. Has got robust live-preview support. And the colors and themes are soothing. 
 4. **Visual Studio Code** - And finally how could Microsoft stay behind. They took `Electron` the runtime used by Atom, and made their own code editor too. If you ask me, they should make VS Code default instead of Notepad/Wordpad on Windows 10. 