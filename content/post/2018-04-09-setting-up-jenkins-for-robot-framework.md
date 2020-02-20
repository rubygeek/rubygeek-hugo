---
categories:
- robot framework
- jenkins
- automation
comments: true
date: "2018-04-09T00:00:00Z"
title: Setting Up Jenkins For Robot Framework
---

[Robot framework](http://www.robotframework.com) is a test automation framework. There are many uses for it, but I use it for selenium web testing. But no matter what you test you can use it for many things. 

[Jenkins](https://jenkins.io/) a tool to manage "jobs", segments of work that you might use "cron" for.

I use it to run robot framework tests nightly or every 6 hours, or whatever you need.

Jenkins is breeze to set up (at least on mac) and once installed search in the plugins for the [robot framework plugin](https://wiki.jenkins.io/display/JENKINS/Robot+Framework+Plugin). The directions on the plugin page are good but. However, If you wish to view the report you have to enable Javascript on jenkins. 

There have been lots of questions on this subject but it all comes down to jenkins permissions.

To view the beautiful reports by robot framework you must relax the ridged rules explained [here](https://wiki.jenkins.io/display/JENKINS/Configuring+Content+Security+Policy). This new feature was added in Jenkins 1.641.

Once JS is enabled then you can view the report and drill down on areas you want to see, including screenshots.

Another jenkins plugin I've found useful is [Naginator](https://wiki.jenkins.io/display/JENKINS/Naginator+Plugin) to restart a job after a failure.


