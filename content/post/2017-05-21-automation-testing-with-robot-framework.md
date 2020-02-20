---
categories:
- testing
- python
- automation
- robot framework
comments: true
date: "2017-05-21T00:00:00Z"
title: Automation Testing with Robot Framework
---

On one of my projects recently, I was tasked with finding an automation framework. Looking at my options..

* Cucumber (ruby, although other languages could be used)
* Capybara / Rspec (ruby) 
* Robot Framework (python) 

All of the options for web testing use either Selenium or Watir. I have only did a little with Watir, so I focused on something with Selenium for my first pass of experimentation. 

I love testing if you know anything about me, I'm nuts about testing. Front End Testing however has been fruitless IMHO .. until a few years ago I discovered [page objects](https://github.com/cheezy/page-object) and it became tolerable to do frontend testing. My projects changed and I didn't really do much for a few years with web frontend testing.

Recently I have been looking into Python and discovered [Robot Framework](http://robotframework.org/), meant to test a variety of things in addition to the web. The Selenium2 package is especially nice. 

You can have a robot script as a text field, in a TSV, or other types of strutured text. I find the text file works for me.

Here we can define a Test Case:

```
Homepage Loads
  Open Browser  https://www.youtube.com  Firefox
  Element Should Be Visible  id=yt-masthead-logo-fragment 
  [Teardown]  Close Browser
```

This script opens the browser to youtube using firefox. Then checks that element located by id=yt-masthead-logo-fragment is visible (ie on the page, not display: none). 

you must make sure to have two spaces between the elements

Open Browser`  `https://www.youtube.com`  `Firefox

Or it won't be able to parse. I guess this is when having it in a spreadsheet (TSV) would come in handy. But it's not too bad to get the hang of. Plus if you use nice editors like [PyCharm](https://www.jetbrains.com/pycharm/) and the Robot Framework Plugin, it has nice syntax highlighting for you.

Running the test case like this:

```
robot youtube.robot
```

And results look like this:

```
▶ robot youtube.robot
==============================================================================
Youtube :: A test to demo testing YouTube
==============================================================================
Homepage Loads                                                        | PASS |
------------------------------------------------------------------------------
Youtube :: A test to demo testing YouTube                             | PASS |
1 critical tests, 1 passed, 0 failed
1 tests total, 1 passed, 0 failed
==============================================================================
Output:  robot-demo/output.xml
Log:     robot-demo/log.html
Report:  robot-demo/report.html
```

There that is just a taste of what it can do. If you want to see what all you need installed you can see my [robot demo](https://github.com/rubygeek/robot-demo) and some more complex tests. More testing posts to come on this, I think this is a very excellent framework ... even if it is not ruby :)
