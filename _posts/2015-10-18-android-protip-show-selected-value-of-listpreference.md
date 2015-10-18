---
layout: post
title: "Android Protip: Show selected value of ListPreference"
description: "Android Protip: Show selected value of ListPreference"
headline: "Android Protip: Show selected value of ListPreference"
modified: 2015-10-18 06:26:49 +0530
category: [Programming, Android]
tags: [android, ListPreference, protip]
imagefeature: 
mathjax: 
chart: 
comments: true
featured: false
---

Here's a quick Protip for people who are still writing complex `onPreferenceChanged` code to show the selected value of a *ListPreference*

Just add `android:summary="%s"` and Android will automatically do it for you ;) 

For example, your code might look like : - 

```
    <ListPreference
        android:summary="%s"
        android:defaultValue="km"
        android:entries="@array/distance_unit_names"
        android:entryValues="@array/distance_unit_values"
        android:key="distance_unit"
        android:title="Distance Unit"/>

```  

Android will even put the currently selected value when you enter the preference screen. 
