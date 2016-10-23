---
layout: post
published: true
mathjax: false
featured: false
comments: false
title: Don't jump onto the Firebase bandwagon yet
description: ''
headline: ''
modified: ''
categories: ''
tags: ''
imagefeature: ''
---
**Firebase** is the rage. It's the talk of the town, and every frontend conference, be it _Web_ or _Mobile_ can't stop talking about [Firebase](http://firebase.google.com). 

But hold your horses a bit our there. Now I am not saying that do not use Firebase. I use Firebase extensively. At [CodingBlocks](http://codingblocks.com) I make it a point to teach using Firebase to the students of the _Android_ programme. But all said and done, Firebase has a **specific** use case. And we cannot overlook that. 

Firebase came about to be used as a _**realtime, object storage**_ system. Both the terms _realtime_ and _object_ storage are important. For one, being realtime is not something we always need. For a chat app yes, for a CRM, probably not needed at all. Secondly the data format of Firebase is very dubious. 
It is an object store meaning, it resembles no-SQL databases like `MongoDB` and `DocumentStore` more than it does RDBMS like `MySQL` or `Oracle`. NoSQL is also something the cool kids have been harping on since 2010, but is again, useful for specific cases. 

Now where my criticism of Firebase stems from are two easily overlooked points -   

1. The "object storage" uses a Database that is not transparent. We do not actually know if it used MongoDB on the backend or some other kind of database. And being proprietary, we do not actually know the availability/consistency tradeoffs, have any perf benchmarks etc of the underlying datastore that powers Firebase.
2. The issue with realtime is, it's too good to be true. And it is. Since more than 7 or 8 cases out of 10, you do not **actually** need something _really_ realtime (as in making a WebSocket connection and doing live 2-way updates), just getting realtime for the lulz is not a good idea. It will be resource intensive, expend more internet bandwidth on your client and devices, and _most important of all_ is a pricey business. 

