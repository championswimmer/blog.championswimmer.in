---
layout: post
title: "Autoconnect Ubuntu Server to a Wifi network"
description: "Autoconnect Ubuntu Server to a Wifi network"
headline: "Autoconnect Ubuntu Server to a Wifi network"
modified: 2015-09-28 07:23:18 +0530
category: [Linux, Networking]
tags: [Linux, Networking, Ubuntu, Server, Wifi]
imagefeature: 
mathjax: 
chart: 
comments: true
featured: false
---

So I just turned my old Lenovo Z580 into a Android Build Server. And the first problem I faced was that of making it auto connect to the home Wifi everytime I turn it on. 

Basically we have to edit the file `/etc/network/interfaces` and make the contents somewhat like this - 

```
auto wlan0
iface wlan0 inet dhcp 
                wpa-ssid YOUR_WIFI_SSID
                wpa-psk  YOUR_WIFI_PASSWORD
```

This is assuming you are on a WPA/WPA2 PSK encryption. If you are on WEP, why dear why ? Move to WPA. 
If you want to affix a permanent static ip address to your server, the contents should rather be like this 


```
auto wlan0
iface wlan0 inet static
				address 192.168.1.111
				netmask 255.255.255.0
				gateway 192.168.1.1
                wpa-ssid YOUR_WIFI_SSID
                wpa-psk  YOUR_WIFI_PASSWORD
```

This is assuming your router's ip is `192.168.1.1`, and the ip you want to affix the server is `192.168.1.111`. Change them to suit your needs. 

