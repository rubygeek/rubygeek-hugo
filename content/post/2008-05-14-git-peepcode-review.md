---
categories: git
comments: true
date: "2008-05-14T00:00:00Z"
title: Git Peepcode Review
---

I'm watching the <a href="https://peepcode.com/products/git">Git peepcode by Geoffrey Grosenbach</a> and as usual with other peepcode screencasts it is very good. This is the ultimate in geek-tv. I swear, i will burn these to a DVD sometime so i can watch them on my tv!   :) 

Things I like about git so far:

* branching, since I am often on the road without internet access. 
* clean, just creates one .git dir in your project root. I just found a bash command that will clean out my .svn dirs from a project I've modified so much on the road that I've borked up the svn process and I'm going to convert it to git

BTW here's that bash command if you are interested

from: http://www.anyexample.com/linux_bsd/bash/recursively_delete__svn_directories.xml

its simply:
```
rm -rf `find . -type d -name .svn`
```

Haha, I remember I wrote a perl script I think to do this like 4 years ago for someone. Who knew it was just a rather short bash command...apparently not me.

Branching is freaking awesome. But what is even more interesting is the stash. Geoffery said it was like a clipboard. You can be working away and you have some changes you are in the middle of but you don't want to commit yet, so you stash them, then go do something else, then later get that stash and apply to your code. Interesting. 

There is a progam gitk that will give you a GUI look at your branches and commits. Thats cool.

get-svn lets you use git locally and then check back into svn. 

BTW I keep typing get instead of git! ... grrr.. I am about ready to make an alias! :)

I like! .. and the Peepcode screencast is great, as usual. I will need to watch the branching again but I highly recommend getting this <a href="https://peepcode.com/products/git">screencast</a> if you are interested in learning git. Its only 9 bucks! 


