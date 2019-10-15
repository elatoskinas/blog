---
layout: posts
title:  "Google Summer of Code with App Inventor"
date:   2019-05-19 01:37:00 +0200
category: Open Source
tags: open-source gsoc appinventor project announcement
---

![Google Summer of Code Logo](https://upload.wikimedia.org/wikipedia/commons/8/85/GSoC-icon.svg){: .align-center width="300px"}

Although some time has passed, I am highly excited to announce that this summer, I have been accepted into the [Google Summer of Code 2019][gsoc] program!
This is truly an exciting moment and a huge milestone for me personally, and a very fun and productive summer awaits!

### What is Google Summer of Code?
The [Google Summer of Code][gsoc] is a program intended to gather students from all over the world to contribute to open source projects. Students apply
to organizations by proposing projects. After being accepted, students spend their summer contributing to their organization by working on their proposed
project.

### Project
![App Inventor Logo](https://img.evbuc.com/https%3A%2F%2Fcdn.evbuc.com%2Fimages%2F61537230%2F30501861831%2F1%2Foriginal.20190501-220337?w=800&auto=compress&rect=0%2C15%2C700%2C350&s=39a8201164e1a1df2732b4b1b6c2d2a1){: .align-center height="300px"}

The [project][project-link] that I will be working on this summer is implementing Chart Components in [App Inventor][appinventor-github], an open-source platform
which is intended to make Android application development accessible and easy to accomplish via a web interface. App Inventor is used worldwide by casual enthusiasts,
students in both schools and universities, teachers as well as advanced application developers. More information can be found on the [App Inventor website][appinventor-web].

The intention of this project is to give users the ability to create Charts in their applications, which will allow to visualize data from different kinds of sources,
such as data added via code, data gathered in real-time or data imported via files.

### Application Period
I would like to share my story on my process overall, from the very beginning, to the very end of the application period.

#### Finding out about GSoC
Since I am a first-year student, this has actually been the first year I could participate in the program. I have encountered the possibility of putting
my developer skills to the test roughly half a year before university by stumbling upon, if I remember correctly, a Quora post. Quite interesting that
encountering a random post on a programming related discussion would spark a new goal and milestone for me.

Fast forward, some thoughts and research on the Google Summer of Code, I have decided that this is a great opportunity to put my skills to the test.
At that point in time, I have not participated in any open source project, or even worked on a project on a scale as large as open source projects.
In fact, the only projects that I have touched were mainly individual projects, or projects involving a few people at most.

I have only started giving serious thought about applying and learning about the program in detail roughly 1 month before the organizations were announced.
At that point, I knew I was seriously dedicated to this task. My goals when applying were primarily to learn how development in open source works,
what technologies are used, how real-world projects look like and exploring new possibilities in programming in general. I did not think much at that point
of getting accepted, and saw this as an adventurous opportunity instead.

#### Picking the projects
After the organizations were announced, the tough part was picking the organizations and projects that I wanted to be a part of.

So, what I did initially was write down the languages and technologies that I am confident in as a head start to my search.
There were some that I could discard immediately. For example, C# projects are not popular at all in the Google Summer of Code, so
that was not really an option. Then, there was JavaScript, which, even though I am capable in and have some experience in, I did not
feel fully confident in to handle tougher projects compared to my other experience. So, my most crucial decision
was to **focus on my strongest points, and what I know best** - Java and Android development.

After establishing my choices, I then spent roughly a **week** to decide on what projects I would like to contribute to.
The reason it took me this much time is the vast choice of projects. There were simply so many interesting projects, and, of course,
I simply could not pick all of them! So, what I did was pick out the interesting ones, and then kept shrinking the list overtime,
filtering out options that either had stiff requirements that I could not meet (for example knowing certain frameworks that I had no
experience with), and projects that were unfortunately slightly less interesting for me personally than others, which was quite
a tough decision.

Initially, I had planned to take up on the maximum allowed limit of 3 projects, however, after being highly indulged in writing 2 proposals,
I have decided I simply do not have the time and energy for a third project. This allowed me to concentrate my focus and efforts towards writing
2 good quality proposals.

The projects I have chosen to work on were Implementing a MongoDB database for [Intermine][intermine], and the Visualization Component project for App Inventor.

#### Research & Contributions
The next obvious step was to delve into my chosen projects.

The Intermine project was more research and knowledge oriented. I spent most of my time simply understanding how the system works and analyzing both PostgreSQL and
MongoDB aspects in great detail. What I did for my proposal was essentially dividing the task into small subtasks so that I would not get lost, and focus
on one small fraction of the work at a time. This was very crucial, because each part of the project required great insight and thorough understanding,
and previous parts were usually important for the upcoming subtasks.

As for the App Inventor project, the process was more technical. I spent most of the time learning how the system works internally, exploring the code base
and tackling on issues. Initially, it was quite hard to get around a large codebase and how things work, but as time passed by, I began understanding
the system, one step at a time. One of my major contributions was the [Join with Separator block][appinventor-separator-pr], which allows to join multiple strings
together into one, each string being separated by a chosen separator string. While it may look like an easy task to implement all in all, doing it in a large system
with many components was more trickier than I expected. After getting familiar with App Inventor, I began exploring the project I have chosen, and provided technical
insights on how I would tackle the problem. I provided a structured plan, as well as more concise details of how the components could be implemented.

#### Proposal
For the App Inventor project, I noticed that the form to be filled for the applications require URLs for answers. So then a thought popped into my head -
why not build a website for my proposal? Not only was this a nice way to present my ideas, but this was also quite a nice way to apply my knowledge
of web site building and gain further experience with HTML, CSS and JavaScript. For this proposal, what I did was add new ideas to the content on a daily basis.
What was initially a rather barebones division of the website into sections gradually turned into an extensive description of the project, my experiences and details.
For the curious readers, the [proposal website can be accessed publicly][proposal].

### Tips
Based on my experience, to every future GSoC applicant, and even to every developer working towards a project, I would recommend the following:

* Focus on your strongest points. This is an exception if your goal is to be adventurous and get involved in something completely new that you have not tried before,
in which case, I would also strongly encourage that!
* Do not be afraid to try. The knowledge and experience that you will gain from the application period, provided you do research and participate,
is quite invaluable nonetheless.
* Treat many things as learning experiences. Trying and learning from your mistakes is one of the best ways to learn. Personally, I find learning by trial and error
to be one of the best ways to get good at something, and I strongly advise everyone to follow the mentality that many things in life (not limited to GSoC) are great learning
experiences.
* Structure your work. It is very easy to get lost when writing a proposal and documentation in general. However, if you have a clear structured plan going, then it becomes
easier, and is more productive.
* Be passionate about what you are doing. If you have no passion, then you will simply not have the drive to work towards something you like. The motivation driven by
passion is what makes the best projects and proposals.
* Research as much as you can. You can never know everything. There will always be something that you have not encountered before. It will be hard in the beginning
to understand something, but as you go further, things become way clearer, and after countless hours spent, you will have your way around things more easily. In my
case, this applied to analyzing codebases and new technologies.


### What to expect next?
One of my inspirations to create this blog was to document my experiences and progress on my Google Summer of Code project this year. This blog for the time being
will contain status updates on the project, including my personal thoughts, challenges and progress overall. So, for the next 3 months, expect to see content primarily
focused on this project!


**Stay tuned for more updates!**

#### Sources
[Google Summer of Code Logo](https://en.wikipedia.org/wiki/Google_Summer_of_Code)
<br>
[App Inventor Logo](https://www.eventbrite.com/e/mit-app-inventor-summit-2019-tickets-61195446227)

[gsoc]: https://summerofcode.withgoogle.com/
[project-link]: https://summerofcode.withgoogle.com/projects/?sp-page=2#5496355911368704
[proposal]: https://elatoskinas.github.io/appinventor-gsoc2019/
[appinventor-github]: https://github.com/mit-cml/appinventor-sources
[appinventor-web]: http://appinventor.mit.edu/explore/
[appinventor-separator-pr]: https://github.com/mit-cml/appinventor-sources/pull/1590
[intermine]: http://intermine.org/