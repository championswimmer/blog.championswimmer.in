---
layout: post
published: true
mathjax: false
featured: false
comments: false
title: "Nativescript-Vue: Native Mobile Apps in Javascript without the Hard Parts™"
slug: nativescript-vue-introduction
description: Using Vue as a framework with Nativescript to make native cross platform mobile apps.
headline: Using Vue as a framework with Nativescript to make native cross platform mobile apps.
categories:
  - nativescript
  - mobile
tags: 'typescript,vuejs,nativescript'
---

![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_0.jpg)A promising alternative to React Native 
## Why Vue ? Why Nativescript ? Why Nativescript-Vue? 

I am a fan of Vue. It is the easiest "framework" to grasp for people who just have bare minimum knowledge of vanilla HTML, CSS and Javascript. And then gives them so much power to build components, complex state management and the blazing fast speed of virtual DOM diffing. 

While the world lapped up React like it is the new jQuery (and I get it's many advantages, I really do), I could never get myself to commit to it. Sure making some fast updates on the page, use React over jQuery - yeah ok. But buy into the ecosystem? That's where I take my leave - because nothing is opinionated in React - you have 10 different blog posts by 10 different React veterans telling 10 completely different *best practices*. State management? Redux is what everyone uses. MobX is what everyone recommends. Even basic setup of project is done by *create-react-app*, which strictly speaking is not made or maintained by the core team. And finally, there are advantages of JSX and CSS-in-JS, but there's a reason markup languages have existed, and it is this - people just find it easy to read declarative markups instead of figuring out how higher-order-functions are recursed to form a view. Vue lets you write familiar HTML and before you fire your project up, it turns it into React-like render function. And that is what a great framework should do - abstract the hard work away from you, not make you write functional view composing functions. 

Anyway, my little React rant is here because I want to preface why we are talking about Nativescript today, and not React Native (the most popular way to write cross-platform Javascript mobile apps today). I have used React Native, and I must say it is a great effort. *I have spent 8 years of my life developing native Android apps using Java, more recently Kotlin, and even a bit of C++. *To get things like animated views and smoothly working lists, simply by throwing an array at a template is not easy in mobile. And writing it in Javascript and expecting the underlying framework to translate it to actual native widgets, while keeping the best performance practices in place is quite a big ask, and React Native delivers. But there are niggles and quirks. 

- The first thing I hate is broken promises. And just after you cross the basic todolist type of apps, React breaks the promise of building the entire project in Javascript. Now I am no stranger to native codebases, but when I am promised pure Javascript, and then for every little task I have to write ***bridges*** and end up with a codebase having 5 languages (JS, Swift, ObjC, Java, Kotlin), or 6 if you count JSX 😅
- The concept of running React Native code in a thread that is **not **the UI thread is actually bad for mobile development. The web is different. You need to keep everything async. But on mobile, if you do have 2 different threads - then you come around to the biggest problem of animating items on screen. The function that calculates the position of a view 60 times a second (in the JS interpreter) and the function that sets the position of the view actually on the screen (in Java/Swift realm) are on two separate threads. And there goes 60 fps flying out of the window. 
*Note: Some of the bridging problems are going to get sorted as there is an upcoming change in their architecture - *[http://facebook.github.io/react-native/blog/2018/06/14/state-of-react-native-2018](http://facebook.github.io/react-native/blog/2018/06/14/state-of-react-native-2018)
- It isn't really **write once.** As has been described in AirBnB's extremely detailed, well written and balanced series on medium - [https://medium.com/airbnb-engineering/sunsetting-react-native-1868ba28e30a](https://medium.com/airbnb-engineering/sunsetting-react-native-1868ba28e30a)
You do end up maintaing 3 apps. The actual view layer, and two entire projects consisting of iOS and Android bridges respectively. Even the UI layer is not 100% common code as many components exist only in iOS or only in Android.

![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_2.jpg)
The reason I am really excited about Nativescript-Vue is the same as the inherent weaknesses I see in React as a framework and React Native as a platform. What I like about Nativescript is unlike React Native - which was an effort to take a web framework into mobile, they created an extremely generic solution as to how Javascript can be executed on mobiles, how native Java or Objective-C classes can be instantiated from exactly equivalent Javascript code, and how native widgets can be constructed from Javascript. Since this solution is generic, accessing native parts from your Javascript does not require special bridges - [because a generic bridge already exists and available at runtime](https://docs.nativescript.org/core-concepts/android-runtime/advanced-topics/execution-flow) - it is synchronous. Your code runs **on** the UI thread just like native mobile apps traditionally do (with the ability to break out into threads for things like File operations or Network operations). [Performance benchmarks ](https://github.com/NativeScript/sample-iOS-Profiling/tree/performance-tests)(synthetic ones, and a bit old, but current numbers are similar) show this difference - NativeScript has overhead of starting up, but once started, the marshalling of data from Javascript to native and back takes much less time compared to other solutions. 
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_5.jpg)
## How Nativescript works

### The Layouts 
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_6.jpg)
Nativescript has in-built layout systems that map pretty much to similar layouts that exist in the Android and iOS world.

![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_7.jpg)![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_8.jpg)The GridLayout is a very powerful layout style, not easy to achievevia XMLs in Android or NIBs in iOS easily too‌‌![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_9.jpg)StackLayouts lay out items linearly in horizontal or vertical manner![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_10.jpg)
### Working with the Native Ecosystem

You can extend your project via other native libraries (i.e. Gradle/Maven dependencies for Android and Cocoapods for iOS), and you can also add NPM modules to your project.
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_11.jpg)
You need to be careful about NPM modules. Those that have C++ bindings are not straightforward to get to work. Also those that depend on NodeJS features that are not present in Nativescript will not work either. My experience with Nativescript has been that most things we require while building a mobile app will usually work out of the box. 

Nativescript uses **JavaScriptCore** (as it is practically the only option) for iOS and it uses V8 for Android. This has some performance gains over React Native using JavaScriptCore on Android, because V8 is better optimised for Android due to the billions Google invests into Chrome for Android. 
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_12.jpg)
The ***most beautiful part ***Nativescript is that you can access Android SDK's Java classes and iOS SDK's Obj-C classes directly from Javascript. In case of Java - whose syntax is similar to Javascript - **it literally is copying Java code, pasting it into Javascript file and voila!! it works.**
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_13.jpg)![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_14.jpg)
There is a fantastic blog post by T J VanToll that describes how this mind-bending trick works under the hood - [https://developer.telerik.com/featured/nativescript-works/](https://developer.telerik.com/featured/nativescript-works/)

### The App architecture and using Frameworks

Nativescript is two things.

1. Executing Javascript on mobile
2. A set of prebuilt views (or widgets) that are basically translated into native iOS/Android widgets at runtime. So a "Button" is `com.android.widgets.Button` on Android and `UIButton` on iOS. 

Apart from that, Nativescript does not depend on being written in any particular way, or using any particulay framework. The creators heavily recommend Typescript, but that's because you get 100% type definition support, and errors are found out at compile stage, that could have creeped into runtime. 

Basically there are many ways you can use NativeScript - 

- Without Framework

- **Only JS files** - construct views with functions and constructors
- **JS for logic + XML for template**  - XML is transpiled into render functions when bundling, or interpreted at runtime
- **TS for Logic + XML for templates** - brings type-safety in. Also recommended in most guides

- With Framework

- **Angular with Typescript** - most common way, starter kit does this
- **Vue** - Nativescript-Vue, still beta, but everything mostly works

The fun part here is, when using Vue, you can use JS, TS, Coffeescript for logic. You can use HTML, Pug, EJS for templates. CSS/SASS/Less/Stylus for styling

![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_16.jpg)![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_17.jpg)![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_18.jpg)![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_19.jpg)
Right now there are two common ways to work with Nativescript-Vue, using the **vue-cli-template **or via **nativescript-vue-template. **When version 2.0 comes out, **vue-cli-template** will be the common one using benefits from both sides (single build step = fast reloads, also .vue single file components). As of version 1.3 you have to chose between fast builds or single file components. 
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_20.jpg)![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_21.jpg)
### Things to take care of . . .

When building mobile apps with web frameworks, one most important thing is to keep in mind it is not the web DOM we are targeting. Things specific to the DOM like event capturing and bubbling aren't similar here. 

Also do **not ** use http and fs APIs from DOM or NodeJS. You need to use Nativescript's own implementation of File and Network APIs. 
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_24.jpg)
And finally, yes, before you start off using NativeScript, you need to prepare your development machine for compiling and debugging native apps. Setting up the Android SDK is pretty easy these days. And the iOS side of things work only on a Mac. On MacOS setting up XCode and iOS SDK is also 2-3 click affair. 

Run `tns doctor` via the Nativescript CLI to make sure all is up and running before you start creating your first app. 
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_25.jpg)
### . . . and you are ? 

I love building mobile and web apps. Vue currently is the framework I am in love most (apart from Ember). You can follow @championswimmer on [Twitter](https://twitter.com/championswimmer) or [Github](https://github.com/championswimmer)

Also I am the co-founder of [Coding Blocks](https://cb.lk) a software development bootcamp with offline classes in India and online courses for anyone over the globe ! Please check out out online courses at -[ https://online.codingblocks.com]( https://online.codingblocks.com) !
![](https://speakerd.s3.amazonaws.com/presentations/631d316c9e6d4b6ab16448dd57b266db/slide_1.jpg)A bit about myself
