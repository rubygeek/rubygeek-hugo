---
categories:
- productivity
date: "2010-07-16T00:00:00Z"
description: GNU Screen commands and settings for productivity
title: Getting around with GNU Screen
---

In past few months, I worked on a project developed entirely on linux. Previously, I mostly did rails development on mac with textmate. I had a brief period of rails and perl where I did vim and screen...oh maybe 4 years ago. Anyways, so I didn't forget much vim over the years, but I had forgotten how to use screen. I looked some documentation to refresh my memory and this post is mainly notes for me :) 

Most commands start with CTRL-a
I refer to each spawning of a new screen in the current session as a window.

### Detach
One things i really like about screen is I can detach it on one computer... then log in somewhere else and reattach. Also handy when you are on a wifi card on the train and you get disconnected. (doh!)
``` bash
ctrl-a d   to detach
```
then start  with -r  to reattach:
``` bash
 screen -r
```

### Create window
This creates a new terminal window
``` bash
ctrl-a c
```

### Name that window
Name your window, so its easier to keep organized
``` bash
ctrl-a A (Yep, ctrl-a then SHIFT-a)
```

### List the windows
See a list of your sessions and their number (this is why you name them) and you can use arrows to select)
``` bash
ctrl-a "
```

### Navigating Windows
You can flip through the windows in order or specify the number:
``` bash
ctrl-a p   #previous window
ctrl-a n, ctrl-a [spacebar]  #next window
ctrl-a #   #the number you want to go to (starts at 0). 
```

### multiple regions in one
``` bash
ctrl-a S   #create a split, creates a new region
ctrl-a TAB #switch to next region
ctrl-a c   #create a terminal session in region 
ctrl-a X   #close the region
ctrl-a C   #clear, this is like typing clear at the prompt to clear the screen
```

### Closing
To exit a window, simply type exit. To exit and kill all windows do
``` bash
ctrl-a ctrl-\
```

### Scrolling
Using terminal on mac or linux won't capture the scroll back.... so you must do it through screen
``` bash
ctrl-a [    #use the arrow key to navigate up 
```
### Refresh
``` bash
ctrl-a l  #refresh the current display
```
### HELP!!
To see the two (!!) pages of screen commands type:
``` bash
ctrl-a ? 
```
### Command mode
Do you like typing?
``` bash
ctrl-a :   #to get to command mode, then you can type commands instead of the ctrl foo jibberish
```
### Need the time?
``` bash
ctrl-a t   #displays the current system time
```
### Named Screen Sessions
Maybe you are working on two separate projects at once, give each one its own screen session
``` bash
screen -S ProjectOne
screen -S ProjectTwo
screen -list 
```
then later you can do
``` bash
screen -r ProjectOne
```
to reattach it and continue

One thing you can do it make it easier is to add this to your .screenrc
``` bash
hardstatus alwayslastline "%?%{yk}%-Lw%?%{wb}%n*%f %t%?(%u)%?%?%{yk}%+Lw%?"
```
It will show the names of the windows you have and highlight the current one. You can see the numbers too so you can do ctrl-a # quickly to jump around.


Anyways, hope this was useful to someone. Let me know any suggestions or anything I can do better!


Sources:
  * [Unix Screen](http://hriday.org/blog/?page_id=595)
  * Screen Man Page
