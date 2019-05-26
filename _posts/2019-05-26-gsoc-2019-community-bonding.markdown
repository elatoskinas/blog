---
layout: posts
title:  "App Inventor GSoC 2019: Community Bonding"
date:   2019-05-26 20:17:00 +0200
category: Open Source
tags: open-source gsoc appinventor project
---

As the community bonding period is nearing to an end and the coding period is about to begin, I would like give a status update on what
happened in the last 3 weeks since the acceptance of my proposal. Overall, the experience has been really splendid!


### Community
From the communication perspective, I have acquainted myself with the mentors well, have participated in helping users of
[App Inventor][appinventor-web] solve issues they are having with the platform, both while making applications and working with the source code. Moreover, over these 3 weeks, there have
been quite a lot of discussions with the developer team about the plans for the project, the design decisions as well as the goals.

Overall, it feels really nice to be part of a small, very friendly and helpful developer team. I believe the 3 weeks have been a sufficient amount of time to get acquainted and feel comfortable amongst the people that I will be working and communicating with throughout the Summer!

### Decisions
The community bonding period also involved making practical decisions, mainly:
* Productivity tools for planning. Scrum planning via GitHub project boards was chosen for this aspect
* Documentation & design tools to use. This has been somewhat a looser decision, but primarily Google Docs will be used for design documents.
* Git branching strategy & workflow
* Testing approach
* Status update method, which was one of the motivating factors to create this blog

### Development Workflow
Setting up the development environment was a priority that I wanted to handle before coding begins. Sometimes, setting up an IDE for a project is really harder than it seems! With App Inventor, setting up involved manually reconfiguring certain options, such as the JDK version, class paths and Android manifest references, all of which were rather tedious and not straight forward issues.

An interesting aspect that I explored during the 3 weeks was an efficient development workflow when working with App Inventor. Following up on some community suggestions, as well as [my mentor's blog post][appinventor-dev-tricks], I have well acquainted myself with various shortcuts that would greatly increase the time it takes to build the App Inventor system. It was really surprising to see how something that takes as long as 4 minutes to compile can be turned to around 15 seconds of compile time in certain circumstances!

### Planning
Some planning has been done throughout the community bonding period. With the flexibility of the project and possible design changes, it was quite difficult to put together a thorough plan that would take up the entire Summer. However, recently some important design choices have been made, and the work that is due for the next 6 weeks is clear.

While planning too much never hurts, I believe a right balance is nice to have, because there might be big changes of plans. And this is in fact something that I took into account, hence the thorough planning done for only the first six weeks of the project, rather than the entire Summer. More planning will of course follow up as time goes on!

### Designing
After all the formal matters, a significant amount of work has been put into designing. For a project such as App Inventor, it is really important to not only implement features, but to also have a design that would prioritize usability. After all, App Inventor is used for educational purposes among many schools in the world, and that is a very important aspect to take into consideration. And, of course, designing something first and implementing it later gives a really great advantage, as the general view is much clearer.

There have been multiple designs that have been done throughout the last 2 weeks. Initially, I started by creating a diagram that would outline the specifications of the project. This design has been followed by designs that focused on smaller parts of the project. The general idea that I follow in my design pattern in this project is to first focus on something broadly, and then keep narrowing it down until reaching the impementation level. I have found this technique to be really effective, highly informative and overall a good way to focus on smaller parts of the task as well as understanding the problem at hand.

### Process
Overall, the rough process that I will follow throughout the project itself this Summer is the following:
1. Planning and setting goals
2. Setting up the development environment (this is the crucial part when starting any project at all)
3. Understanding specifications by modelling them
4. Narrowing the specifications down to smaller sub problems
5. Designing solutions for narrowed down design problems
6. Implementing the solutions to problems according to design (when the design is finished)

With that being said, the points I have mentioned were fairly brief. More status updates will follow in future posts, and will contain more concise details.

**Stay tuned!**

[appinventor-web]: http://appinventor.mit.edu/explore/
[appinventor-dev-tricks]: https://josmasflores.blogspot.com/2014/10/app-inventor-dev-tricks-reducing.html