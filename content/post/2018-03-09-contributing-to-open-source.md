---
categories:
- open source
- practice
- learning
- job search
- interviews
comments: true
date: "2018-03-09T00:00:00Z"
title: Contributing to Open Source
---

You probably use open source software if you are programmer and you know you *should* probably at some point give back right? I've been programming since I was a kid and working as a professional programmer for 17 years (at the time of this post). I freaking love code but I wouldn't say I've contributed a lot in the past years. :(

I want to talk about some of my experiences and what you can do to get started.

I contributed to Clojure 1.8 by adding more functions to the string namespace. I took off two weeks from consulting for "vacation" and worked on that. I found this particular task by posting on twitter asking for any ideas for a project I could complete in a short amount of time. [Alex Miller](https://twitter.com/puredanger) saw it and said he had a ticket I could work on and he would help me. It was a great experience and I learned a lot. The Clojure Team has a system of applying patches so they don't use the typical "github pull requests" but it was an easy enough process to learn and it was fine.

Most open source projects on Github use pull requests. I remember one of my first pull requests to a project was to [organize the examples](https://github.com/jnunemaker/httparty/blob/master/examples/README.md) at [httparty (ruby)](https://github.com/jnunemaker/httparty). At the time I was using a lot of httparty in my project and kept going back to the examples and always forgetting which one was which. Organizing it make it easy to see which examples did what.

My current company [Condé Nast](http://www.condenast.com/) has been holding Open Source Hack Nights, where we meet as a company and work on project that helps us with our job. I have been working with [Robot Framework](http://robotframework.org/) a test automation tool. This was the push I needed. There have been a few things that have come up when I was working with it that I wished it had. I now have 3 contributions to the project which we currently use!

Now that I've been so active with Robot Framework, I've been asked to join the list of active contributors which of course I will do, and happy to help :) 

Now you may wonder how you can find things you can do? 

1. Begging :) Like when I was looking for a project to work for Clojure for my "vacation".
1. You could ask the maintainers if there is anything you can do to help.
1. Something you need. When working with httparty, I was constantly referring to the examples directory to find something I knew was there. With Robot Framework, there was some functionality I needed.
1. Look through the existing issues on a project to see if you can help.
1. Keep checking the issues that come through on a project. One of the tickets I did for Robot Framework was something someone *else* requested. 

Now, you found something you need or you think you can help with.

Ask on the chat channel explaining your need, they might said oh do it this way or its already there or may say, go ahead and submit a PR. You should always ask because you want to make sure you at least have a chance at being accepted.

When I saw someone ask for a feature on Robot Framework, I asked if he didn't want to make a PR, that I would be happy to help. He said he didn't have time and that I could do it. Come to think of it, I also asked on that Clojure ticket since someone had already completed half the work but didn't finish. I asked if I could finish and the other party said sure go ahead! ... which leads me to this..

It is just polite to ask :) I witnessed someone reporting a bug with a project, the maintainers said "oh thats a bug can you submit a PR?" and before my friend could act, someone not involved in the conversation submitted a PR and "took it". Great, but he could have asked first. Needless to say, my friend still holds a grudge. So speak up first and be polite. 

Look on the contributor guidelines for the project. Style guides, tests etc all make your PR go smoother.

You can use your open source contributions as "proof you know what you are doing" at your next job interview. In the past, when applying for Clojure jobs I talked about the PR I did for Clojure 1.8. Now I can use the PRs I did for Robot Framework. I even put them on my resume in an Open Source section!

I hope I've inspired you to go out there and submit some open source code (or some docs, like my submission to httparty). Worst they can say is No and you may even something in the process.



 