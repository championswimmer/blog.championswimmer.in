---
layout: post
title: "GSOC-An-exciting-beginning"
description: "Beginning coding on the Google Summer of Code"
headline: "Google Summer of Code : An-exciting-beginning"
modified: 2015-06-10 07:51:27 +0530
category: Open Source
tags: [gsoc, code, fossasia, open-event]
imagefeature: 
mathjax: 
chart: 
comments: true
featured: true
share: true
---
This is ofcourse a bit too late to actually write a post about the beginning of the GSoC 2015 session, but nevertheless, the excitement hasn’t rubbed off yet, so I’ll go forward with it.

To state the obvious, this year my proposal to build an App and Management Utility for Tech Conferences and related events was selected under Google Summer of Code 2015. I will be working with the organisation [FOSSASIA](http://fossasia.org) which is the largest Open Source organisation in Asia. To add to my excitement, my mentor is [@mariobehling](http://twitter.com/mariobehling) who is none other than the founder of Lubuntu. And my other mentor is [@dukeleto](http://twitter.com/dukeleto) who has been teaching us a lot of useful stuff like how to write scrum mails, manage issue tracking and documentation on github, among others.

Along with me, *Rafal* and *Manan* are also working on the same project, and after some initial hiccups where I and Manan trampled over each other’s codes a bit, I think we make a great team 😉

So, let me describe a little more about the project. We call it ‘Open Event’, and we aim to make it a easy to use solution for all Open Source (or otherwise) event/conference/seminar/workshop organisers to be able to host data about their sessions, speakers, tracks, and be able to show this to the attendees using a Webapp and an Android App. You can always read more about the project goals and track the current progress at the [umbrella project on Github.](https://github.com/fossasia/open-event)

Rafal is mainly working on the [server that will host the data](https://github.com/fossasia/open-event-orga-server) and he’s writing that up in Python, and using flask-admin to create the dashboard. Manan is responsible for the [Android app](https://github.com/fossasia/open-event-android) (which I secretly wish I had got, because I love to work more on Android than anything else), while I am going to create [the webapp .](https://github.com/fossasia/open-event-webapp)

The idea behind both the Android and webapp will be to allow event organisers to generate their own apps, without having much technical knowledge. Ideally, when our project is finished, The organisers should be able to create an Android app and a webapp, without writing a single line of code. They would just fill the event name, the color scheme etc, and then fill the data about the sessions, speakers, sponsors, tracks, locations etc of the event.

We hope by the end of the summer we can be ready with a service/product that all event organisers would love to use to host their events on in the future. I’ll follow this blog post up soon with technical details about each component, and the API schema we have adopted.

Right now, it’s 4 days past the ‘coding start’ date, and I have not coded a lot, because I have spent most of my time trying to learn ***AngularJS***, which was very new to me. I should get back to writing more code, and stop writing blog articles, or else Duke and Mario won’t be too happy with me 😛
