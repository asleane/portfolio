---
layout: project
title: Explorer for GitHub
github: Explorer-for-Github
category: project
image: images/explorer/explorer_trending.png
tags:
- Swift
- MVVM-C
- GraphQL
- Firebase
---

Explorer for GitHub is a native iOS application written in Swift. It enables you to view GitHub's currently trending repositories in all programming languages and it also gives you the ability to explore repositories based on different conditions like count of stars or if the repository has issues that are beginner-friendly.

<ul class="images">
    <a href="{{ site.baseurl }}/images/explorer/explorer_welcome.png" data-lightbox="roadtrip"><img src="{{ site.baseurl }}/images/explorer/explorer_welcome.png"></a>
    <a href="{{ site.baseurl }}/images/explorer/explorer_trending.png" data-lightbox="roadtrip"><img src="{{ site.baseurl }}/images/explorer/explorer_trending.png"></a>
    <a href="{{ site.baseurl }}/images/explorer/explorer_contribute.png" data-lightbox="roadtrip"><img src="{{ site.baseurl }}/images/explorer/explorer_contribute.png"></a>
    <a href="{{ site.baseurl }}/images/explorer/explorer_under50.png" data-lightbox="roadtrip"><img src="{{ site.baseurl }}/images/explorer/explorer_under50.png"></a>
    <a href="{{ site.baseurl }}/images/explorer/explorer_languages.png" data-lightbox="roadtrip"><img src="{{ site.baseurl }}/images/explorer/explorer_languages.png"></a>
    <a href="{{ site.baseurl }}/images/explorer/explorer_settings.png" data-lightbox="roadtrip"><img src="{{ site.baseurl }}/images/explorer/explorer_settings.png"></a>

</ul>


# Idea behind the app
Last year [Daniel Leivers](https://twitter.com/SofaRacing) gave a talk about [GraphQL](https://graphql.org/) at an [NSLondon](https://www.meetup.com/NSLondon/) meetup. NSLondon is London's biggest iOS/macOS developer meetup taking place only a couple of times a year. If you ever want to visit make sure to reserve your ticket as soon as possible since the meet-up always fills up very quickly. This was the first time I heard about GraphQL and I really liked the approach of it and wanted to learn more. I saw that GitHub recently changed the public API to GraphQL so I decided to build a small proof of concept around the GitHub API.

This is currently a work in progress and although I very much enjoy learning more, it is as of yet not further documented. 