---
layout: post
title: "Sharing your Laptop's WiFi with your BeagleBone"
description: "Sharing wifi over USB with Beaglebone Black"
headline: "Sharing wifi over USB with Beaglebone Black"
modified: 2015-09-18 16:35:28 +0530
category: [Hardware, Networking]
tags: [Beaglebone, Linux, Networking, Hardware]
imagefeature: 
mathjax: 
chart: 
comments: true
featured: false
---

If you have tethered your BBB with your laptop, you'd know how magical that device is. You can SSH into it, you can view it over a browser on http://beaglebone.local and it takes power from your laptop and what not. But one very obvious feature that you might miss is, not being able to use the laptop's internet connection. 

Basically what we are looking to do here is share the Wifi/LAN network that the Laptop uses, with the BBB. 

As it so happens it is not that hard after all. In your BBB (as in when you are SSH-ed in), open the file `/etc/resolv.conf` and change the contents to 

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

Just FYI those are DNS nameservers maintained by Google. You might want to use OpenDNS name servers too if you so wish to.  
Close the file and now restart networking interface by  
`/etc/init.d/networking restart`

Tada, you should now be able to `ping www.google.com` from your BBB. 

If you are on Windows on the Laptop, bridge your Internet connection with the BBB connection. 

If you are on Linux, perform the following steps

```bash
   sudo iptables -A POSTROUTING -t nat -j MASQUERADE
   sudo echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward > /dev/null
```
