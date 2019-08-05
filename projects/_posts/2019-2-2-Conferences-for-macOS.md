---
layout: project
title: Conferences for macOS
github: Conferences.digital
category: project
image: images/conferences/conf_mac.png
tags:
- Swift
- TinyConstraints
- Realm
- Programmatically UI
- Fabric.io
- Fastlane 
- git flow
- Vapor 
- MySQL
- Docker
---

Conferences for macOS is the best way to watch the latest and greatest videos from your favorite developer conferences for free on your Mac. Either search specifically for conferences, talks, speakers or topics or simply browse through the catalog - you can add talks to your watchlist to save for later, favorite or continue watching where you left off.

<a href="{{ site.baseurl }}/images/conferences/conf_mac.png" data-lightbox="roadtrip"><img alt="Without caching" src="{{ site.baseurl }}/images/conferences/conf_mac.png"></a>

---

## Idea
I have already been to a couple of iOS developer conferences and had the same problem every time - I was not able to see every talk. Sometimes it was not possible because the conference had multiple tracks and sometimes it was just nicer to talk to friends and meet new people. So after almost every conference, I ended up searching the internet for conference videos. This was really annoying and I thought it would be really nice to watch these videos in a single place. After a little bit of research, I found some websites like [Swifttube](http://www.swifttube.co) and [LearnTalks](http://learntalks.com) but wasn't really satisfied. I wanted to have the ability to add videos to a watchlist and track progress of videos in a native macOS application. I was not able to find such an app, so I decided to build one on my own.


## Native macOS App vs. Electron app

Before I started, I had to make the decision whether I wanted to create a native macOS app or an Electron-based app. I had no experience with either of them, but worked with JavaScript-based mobile apps before - which I didn't like - therefore I chose to develop a native macOS app written in Swift.

Like always when developing for new platforms, using new frameworks or learning new programming languages, you are a little bit lost in the beginning, read a lot of documentation and finally get the hang of it. So I started reading the documentation and realised very quickly that AppKit and UIKit share a lot of concepts. `UIView` is `NSView`, `UIImage` is `NSImage`, `UIControl` is `NSControl`, `UIViewController` is `NSViewController` and so on - I encountered only a few differences and was able to build a POC within two days.

<br>


**Multi-Window Support** - As a mobile/backend developer, the concept of multi-window support was new to me. MacOS apps can have multiple windows, for example: a window for app settings, a window for shared dialogue, a window for your main app or even no window when developing a menu bar app. And again, I was really surprised when I found that iOS is also able to support multiple `UIScreens`. When you connect or disconnect an external monitor to an iOS device, the system sends notifications like `didConnectNotification` and `didDisconnectNotification`. Based on these notifications you can either add or remove a UIScreen to the external monitor.

**Flexible Layout** - Again, it pleasant to see that Auto Layout is supported on macOS, since I've already worked a lot with it in the past. My usually go-to method, in order to interact with Auto Layout, is the Visual Format Language. But I thought it would be worth to give a third-party framework like TinyConstraints a try and I have to say that I liked working with it. It is super easy to set up complex views with a minimum amount of code. Compared to iOS, one difference I encountered was that the size of `NSWindow` and `NSSpitViewController` can be changed by the user - so if you ever work on a macOS app, make sure to set a minimum size for `NSWindow` and keep your constraints as flexible as possible.


## Server-Side

The best way to provide new content for the app was to implement a dedicated server with a REST-API. I had to chose between a stand-alone server or services like Firebase. Since I have worked with Firebase a lot in the past, I thought it would be a great chance to refresh my backend developer skills. On the server side, I mainly worked with [Node.js](https://nodejs.org/) and [TypeScript](https://www.typescriptlang.org) in the past and again thought it would be nice to try something new. I've decided to give Vapor (Server-side Swift) a try and I can say now it was a good choice. I probably would not have said this in the beginning, because in my opinion the documentation could definitely be improved - so if you want to look into Vapor, I would recommend reading [Server Side Swift with Vapor](https://store.raywenderlich.com/products/server-side-swift-with-vapor) rather than the official documentation.

In terms of data storage I decided to set up a simple MySQL database with the following scheme:

<a href="{{ site.baseurl }}/images/conferences/conf_database.png" data-lightbox="roadtrip"><img alt="Without caching" src="{{ site.baseurl }}/images/conferences/conf_database.png"></a>


The server and the database are currently hosted on the cheapest available droplet on [DigitalOcean](https://digitalocean.com). In the beginning, I wasn't sure if DigitalOcean was the right choice, which is why I looked into technologies like [Docker](https://www.docker.com/) in order to simply deploy my server onto different machines. This was my first experience with docker and have to say it's an incredible tool, it takes a little bit of time to configure your project, but trust me, in the end, it will save you a lot of time and stress. 

It's always hard to tell how many users will use your product in the end, but I wanted to make sure that my API was able to serve a lot of requests without crashing. So I ran a few tests and the results were awful. 

I tested my API with 15,000 requests per minute, ambitious I know, and after only 13 successful requests the response time was so high that the test automatically stopped. In order to fix this I developed a custom caching middleware and after a few tests and some adjustments the final results were great. Now, in one minute 15,000 requests were successful, 4.11 GB data was transferred and all of that with an average response time of 14 ms.

<br>

<p align="center">
    <a href="{{ site.baseurl }}/images/conferences/caching_0.png" data-lightbox="roadtrip"><img alt="Without caching" src="{{ site.baseurl }}/images/conferences/caching_0.png" width="40%"></a>
    <a href="{{ site.baseurl }}/images/conferences/caching_2.png" data-lightbox="roadtrip"><img alt="Second iteracton caching" src="{{ site.baseurl }}/images/conferences/caching_2.png" width="40%"></a>
</p>

<br>

So overall, DigitalOcean and Vapor were the right choices for me, I never had any issues and my server has an uptime for more than 90 days.

## Release flow

Probably the only thing I was certain of in the beginning, was that I would want to publish my app Open Source. In my opinion Open Source software has a lot of benefits. One of them is that you allow anyone to contribute to your project. In order to help contributors understand your project and its needs, it's important to provide some [guidelines](https://github.com/atom/atom/blob/master/CONTRIBUTING.md). Even more important is to use version control (i.e. [git](https://git-scm.com/)). There are a lot of different opinions on how to use version control in terms of branch and tag naming, commit messages etc. I'm a big fan of [git-flow](https://danielkummer.github.io/git-flow-cheatsheet/) since it's clearly defined and scaleable to any project size.

![Conferences Git]({{ site.baseurl }}/images/conferences/conf_git.png)
<p style="font-size: 7pt">(Commit messages could me improved)</p>


As soon as you're ready to ship an initial version to beta testers, it's always a good idea to use build and crash reporting tools. My favourite tools for this job are [Fastlane](http://fastlane.tools) and Crashlytics ([Fabric.io](https://fabric.io/)), they are both commenly known, super easy to set up and provide a variety of functionality. If you are using Fabric.io make sure to keep an eye on their current [roadmap](https://get.fabric.io/roadmap).

## What next?
The macOS app has reached a couple of thousand users so far and gained widespread support from the community. The app was mentioned in
* [iOS Dev Weekly](https://iosdevweekly.com/issues/395)
* [Stacktrace Podcast](https://9to5mac.com/2019/03/13/stacktrace-podcast-027-lets-spelunk-my-life/)
* [Swift over Coffee Podcast](https://itunes.apple.com/gb/podcast/swift-over-coffee/id1435076502)
* [Swift News](https://m.youtube.com/watch?v=zvAlFwZjWzw)
* [iOS Goodies](https://ios-goodies.com/post/183450329877/week-273)

Since then we have grown into a small community of developers working on an initial versions of the iOS and tvOS app. If you want to learn more about it or even contribute - there's also a lot to do besides writing code - please feel free to join our [slack channel](https://join.slack.com/t/conferencesdigital/shared_invite/enQtNTc3MjEyNzE0NTYxLTU2NTgxOTI3YjBlN2JkZTA2ODAxMTQ0OWJlMDhmMjZmZWMzNTA1OTM3YjQ3YzRkZjZhZGEzNzdhN2M2ZjAxNDI).


## Conclusion
It was really fun to work with a lot of different new technologies and I never thought that this app could reach so many people. After writing this blog post, I realised that I focused more on build tools/techniques rather than on coding problems I encountered. But honestly, working as a software developer also means that you spend a lot of time with build tools, so it's a valuable asset to know your tools!




