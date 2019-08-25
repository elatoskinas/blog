---
layout: posts
title:  "App Inventor Chart Components: Project Summary"
date:   2019-08-25 19:35:00 +0200
category: Open Source
tags: open-source gsoc appinventor project
---

![Google Summer of Code Logo](https://upload.wikimedia.org/wikipedia/commons/8/85/GSoC-icon.svg){: .align-center width="300px"}
![App Inventor Logo](https://img.evbuc.com/https%3A%2F%2Fcdn.evbuc.com%2Fimages%2F61537230%2F30501861831%2F1%2Foriginal.20190501-220337?w=800&auto=compress&rect=0%2C15%2C700%2C350&s=39a8201164e1a1df2732b4b1b6c2d2a1){: .align-center height="300px"}

## Overview
Throughout the last 3 months, I have worked on implementing Chart Components for [App Inventor][appinventor].
Now, this project is in a relatively finished state, and is in review process.

With this post, I would like to summarize the project by providing links to my work, as well as some
remarks.

## General
The main, general links are listed below:

* [App Inventor Pull Request][final-pr]
* [GitHub Project Board][project-board]
* [Pull Request history][pr-history]
* [Issue history][issue-history]
* [Documentation & Tutorial][documentation]
* [Google Summer of Code Project Link][gsoc-project]

## Project Summary
![Chart Components Preview]({{ site.baseurl }}/assets/images/posts/gsoc2019/preview/views/ChartsComponentPreview.png){: .align-center width="300px"}

Briefly, the project that I have worked on implements drag and droppable Charts for use in [App Inventor][appinventor].
The Chart components can be customized to the user's liking, and supports five different types - Line, Area, Scatter,
Bar and Pie Charts. An intuitive workflow has been implemented in the web editor, which quite accurately depicts the
Chart component that will be visible in the applications made with App Inventor.

In addition, a system was created for importing data into the Chart, both manually, and via
pre-defined Data Sources. The system implemented is fully and quite simply extendible to add
support for many more Data Sources.

## Challenges
Initially, I expected the project to be relatively straight forward and rather simple. However,
there were quite a few challenges that occurred during the project. I would outline these 3 points
to be hardest challenges faced during the project:

1. Making use of a library that is not thread safe in asynchronous methods; The Charts library
used on the Android side (MPAndroidChart) was not fully designed to support dynamic, asynchronous
data importing. Crashes would often occur, and it took a while for me to pinpoint the issue.
Exploring possible solutions without modifying the library itself took more time than
any other individual feature, and this was a challenge that I had to postpone a few times in
order to keep up with my schedule. In the end, I found a workaround which made copies of the
data. Two pull requests cover the fix -- [Refresh, Chart Legend and t value fix][pr-chart-refresh-fix]
and [Refresh fixes][pr-chart-refresh-fix-final]

2. Getting the hang of documenting and designing features for the project. This was the first real-life
larger scale project that I have been involved in that was not a personal project, and not a university
assignment. In a real setting, I felt the need to document and design features in such a way that they
would be accessible to the entire open source community, and that future developers could understand the
code that I have written and the features I have implemented. Initially, I found this to be quite an
overwhelming task, however, as time went by, I got more comfortable with it. Generally, I made sure
to write JavaDocs on all methods, and clarify the more difficult to understand implementations.
Moreover, I made sure to leave detailed comments on code which is not instantly apparent, and
left TODOs on features which I think should still be worked on further in the future.

3. Keeping up with the schedule of writing documentation. Generally, I found writing blog posts
to be quite time-consuming process which required full attention and a lot of focus. There
have been quite a few weeks where I could not make a blog post in time due to various other
matters to take care of, and somewhere around the 5th week I have simply dropped the idea
of having a weekly blog post. In the end, however, I have written all the blog posts that I
have planned to write.

## What I Learned
Overall, the project was a great learning experience, and has really helped me to sharpen my
skills as a programmer and as a Software Engineer.

The main points that I would outline that I have learned during the project are as follows:
1. Multithreading. Before the project, I was completely unfamiliar of the sheer amount of
tools available to handle various multithreading scenarios. My general experience with
multithreading before was simply usage of Threads, synchronized methods and Coroutines (in C#).
However, throughout the project I have gained a lot of knowledge about various Java utilities
available for concurrent usage handling and multithreading, such as Latches, Executors,
AtomicReferences, Timers and Handlers. Moreover, I have deepened my understanding of AsyncTasks,
and how they work internally.

2. Interface-based pattern implementations. During the project, I have deepened my understanding
in patterns which are based on interfaces. The one which I would outline is the Observer pattern,
which has one object, observable, store a collection of observers, and then notifies them manually
via an interface method. Throughout code examples both outside and inside the repository, I felt
like I gained a better understanding of such design patterns overall.

3. Working around large repositories. During the project and my first contributions to App Inventor,
I have gotten the hang of navigating around projects as large as App Inventor. Generally, whenever
I need to find how or where a certain feature is implemented, I search for keywords related
to the feature, and then follow the method usages in the found classes or methods. With practice,
eventually I felt that I became much more efficient in working around large projects.

## Posts
During the project, a series of blog posts were made which provided status updates and covered
the implemented features throughout the project in depth, mostly from a technical point of view.

The list of posts can be found here:
* [GSoC 2019 Announcement][post-gsoc-announcement]
* [Community Bonding][post-community-bonding]
* [First Steps][post-first-steps]
* [Prototype][post-prototype]
* [Chart Workflow Preview][post-workflow-preview]
* [Chart Workflow Implementation][post-workflow-implementation]
* [MPAndroidChart Concentric Pie Charts][post-mpandroidchart-concentric] (the following post was not included in the series of GSoC posts,
but the concepts presented in the post were used in the project)
* [Data Importing from Tuples][post-tuple-import]
* [Chart Type Selection, Area and Scatter Charts][post-type-selection]
* [Data Source Concepts & Database Importing][post-data-source-concepts-db]
* [CSV, JSON and Web importing][post-csv-json-web]
* [Real Time Data Importing][post-real-time-import]
* [Pie Charts][post-pie-chart]
* [Bar Charts & Chart Customization][post-bar-chart-customization]

## Pull Requests
While working on the project, I have divided the problem into smaller
subtasks, and focused on implementing features, one at a time. As such,
quite a lot of pull requests were made and checked through manually
on each feature implementation.

The complete list can be found here:
* [Line Chart Prototype][pr-chart-line-prototype]
* [Mock Line Chart Prototype][pr-chart-mock-line-prototype]
* [Bar Chart Prototype][pr-chart-bar-chart-prototype]
* [Pie Chart Prototype][pr-chart-pie-chart-prototype]
* [Chart Data Core][pr-chart-data-core]
* [Data Core Properties][pr-chart-data-core-properties]
* [Mock Chart Data][pr-chart-mock-chart-data]
* [Data Core Refactor][pr-chart-data-core-refactor]
* [Chart Core Tests][pr-chart-core-tests]
* [Chart List Import & Data Operations][pr-chart-list-import-data-operations]
* [Charts Component Redesign (Type property)][pr-chart-component-redesign]
* [Chart CSV Importing][pr-chart-csv-import]
* [Component redesign update][pr-chart-update-component-redesign]
* [Area Chart][pr-chart-area-chart]
* [File component refactor][pr-chart-filebase-refactor]
* [CSV Importing Designer Workflow][pr-chart-csv-designer-workflow]
* [Scatter Chart & Mock Tuple Concepts][pr-chart-scatter-chart-mock-tuple-concepts]
* [Mock Chart Data Model Tuple Concepts][pr-chart-mock-tuple-concepts]
* [Chart Type refactor][pr-chart-type-refactor]
* [Chart Data CRUD][pr-chart-data-crud]
* [Data Source Core Concepts][pr-chart-data-source-concepts]
* [TinyDB Importing & Data Source observing][pr-chart-tinydb-data-source-observing]
* [CloudDB Import][pr-chart-clouddb-import]
* [TinyDB & Data Source observing (update)][pr-chart-tinydb-updated]
* [Mock Chart attachment fix][pr-chart-mock-chart-attachment-fix]
* [Real Time Data Importing][pr-chart-real-time-importing]
* [Pie Chart][pr-chart-pie-chart]
* [JSON Importing][pr-chart-json-import]
* [Bluetooth Data Importing][pr-chart-bluetooth-import]
* [Refresh, Chart Legend and t value fixes][pr-chart-refresh-fix]
* [Bar Chart][pr-chart-bar-chart]
* [Web Import][pr-chart-web-import]
* [Data Source Changing][pr-chart-data-source-changing]
* [Chart Customization][pr-chart-customization]
* [Refresh fixes][pr-chart-refresh-fix-final]
* [Sensor Compatibility][pr-chart-sensor-compatibility]
* [Finalizing][pr-chart-finalizing]


## Stay tuned!
This post concluded the project that I have worked on as part of Google Summer of Code 2019.
Right now, the project that I am working on is still in review, and hence I am still putting
quite a lot of attention into the project, especially in terms of formal matters.

However, at some point, I will get around to posting more blog posts on various projects
that I am working on.

Stay tuned!

[appinventor]: https://appinventor.mit.edu/explore/
[final-pr]: https://github.com/mit-cml/appinventor-sources/pull/1776
[project-board]: https://github.com/lightingft/appinventor-sources/projects/1
[pr-history]: https://github.com/lightingft/appinventor-sources/pulls?q=is%3Aopen+is%3Apr
[issue-history]: https://github.com/lightingft/appinventor-sources/issues
[documentation]: https://docs.google.com/document/d/1QqWWgkoZ9VIhkb1DLizhZD60I7Nt6SrM8jCAIWhtxM8
[gsoc-project]: https://summerofcode.withgoogle.com/projects/?sp-page=2#5496355911368704

[post-gsoc-announcement]: {{ site.baseurl }}{% post_url 2019-05-19-gsoc-2019-announcement %}
[post-community-bonding]: {{ site.baseurl }}{% post_url 2019-05-26-gsoc-2019-community-bonding %}
[post-first-steps]: {{ site.baseurl }}{% post_url 2019-06-01-gsoc-2019-first-steps %}
[post-prototype]: {{ site.baseurl }}{% post_url 2019-06-02-gsoc-2019-prototype %}
[post-workflow-preview]: {{ site.baseurl }}{% post_url 2019-06-10-gsoc-2019-workflow-preview %}
[post-workflow-implementation]: {{ site.baseurl }}{% post_url 2019-06-15-gsoc-2019-charts-workflow-implementation %}
[post-mpandroidchart-concentric]: {{ site.baseurl }}{% post_url 2019-06-24-mpandroidchart-concentric-pie-charts %}
[post-tuple-import]: {{ site.baseurl }}{% post_url 2019-07-10-gsoc-2019-data-importing-from-tuples %}
[post-type-selection]: {{ site.baseurl }}{% post_url 2019-07-27-gsoc-2019-chart-type-selection-area-scatter %}
[post-data-source-concepts-db]: {{ site.baseurl }}{% post_url 2019-08-14-gsoc-2019-chart-data-source-concepts-and-db %}
[post-csv-json-web]: {{ site.baseurl }}{% post_url 2019-08-20-gsoc-2019-chart-data-source-csv-json-web %}
[post-real-time-import]: {{ site.baseurl }}{% post_url 2019-08-21-gsoc-2019-chart-data-source-real-time-import %}
[post-pie-chart]: {{ site.baseurl }}{% post_url 2019-08-24-gsoc-2019-chart-pie-chart %}
[post-bar-chart-customization]: {{ site.baseurl }}{% post_url 2019-08-24-gsoc-2019-chart-bar-chart-customization %}

[pr-chart-line-prototype]: https://github.com/lightingft/appinventor-sources/pull/18
[pr-chart-mock-line-prototype]: https://github.com/lightingft/appinventor-sources/pull/23
[pr-chart-bar-chart-prototype]: https://github.com/lightingft/appinventor-sources/pull/30
[pr-chart-pie-chart-prototype]: https://github.com/lightingft/appinventor-sources/pull/31
[pr-chart-data-core]: https://github.com/lightingft/appinventor-sources/pull/37
[pr-chart-data-core-properties]: https://github.com/lightingft/appinventor-sources/pull/46
[pr-chart-mock-chart-data]: https://github.com/lightingft/appinventor-sources/pull/47
[pr-chart-data-core-refactor]: https://github.com/lightingft/appinventor-sources/pull/52
[pr-chart-core-tests]: https://github.com/lightingft/appinventor-sources/pull/55
[pr-chart-list-import-data-operations]: https://github.com/lightingft/appinventor-sources/pull/70
[pr-chart-component-redesign]: https://github.com/lightingft/appinventor-sources/pull/78
[pr-chart-csv-import]: https://github.com/lightingft/appinventor-sources/pull/83
[pr-chart-update-component-redesign]: https://github.com/lightingft/appinventor-sources/pull/84
[pr-chart-area-chart]: https://github.com/lightingft/appinventor-sources/pull/86
[pr-chart-filebase-refactor]: https://github.com/lightingft/appinventor-sources/pull/94
[pr-chart-csv-designer-workflow]: https://github.com/lightingft/appinventor-sources/pull/98
[pr-chart-scatter-chart-mock-tuple-concepts]: https://github.com/lightingft/appinventor-sources/pull/101
[pr-chart-mock-tuple-concepts]: https://github.com/lightingft/appinventor-sources/pull/101
[pr-chart-type-refactor]: https://github.com/lightingft/appinventor-sources/pull/104
[pr-chart-data-crud]: https://github.com/lightingft/appinventor-sources/pull/105
[pr-chart-data-source-concepts]: https://github.com/lightingft/appinventor-sources/pull/106
[pr-chart-tinydb-data-source-observing]: https://github.com/lightingft/appinventor-sources/pull/107
[pr-chart-clouddb-import]: https://github.com/lightingft/appinventor-sources/pull/112
[pr-chart-tinydb-updated]: https://github.com/lightingft/appinventor-sources/pull/115
[pr-chart-mock-chart-attachment-fix]: https://github.com/lightingft/appinventor-sources/pull/116
[pr-chart-real-time-importing]: https://github.com/lightingft/appinventor-sources/pull/119
[pr-chart-pie-chart]: https://github.com/lightingft/appinventor-sources/pull/120
[pr-chart-json-import]: https://github.com/lightingft/appinventor-sources/pull/124
[pr-chart-bluetooth-import]: https://github.com/lightingft/appinventor-sources/pull/125
[pr-chart-refresh-fix]: https://github.com/lightingft/appinventor-sources/pull/129
[pr-chart-bar-chart]: https://github.com/lightingft/appinventor-sources/pull/135
[pr-chart-web-import]: https://github.com/lightingft/appinventor-sources/pull/131
[pr-chart-data-source-changing]: https://github.com/lightingft/appinventor-sources/pull/132
[pr-chart-customization]: https://github.com/lightingft/appinventor-sources/pull/137
[pr-chart-refresh-fix-final]: https://github.com/lightingft/appinventor-sources/pull/138
[pr-chart-sensor-compatibility]: https://github.com/lightingft/appinventor-sources/pull/141
[pr-chart-finalizing]: https://github.com/lightingft/appinventor-sources/pull/144
