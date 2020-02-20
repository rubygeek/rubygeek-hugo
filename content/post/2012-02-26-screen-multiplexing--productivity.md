---
categories:
- productivity
date: "2012-02-26T00:00:00Z"
description: Some of my .tmux.conf settings and keyboard shortcuts
title: Screen Multiplexing == Productivity
---

A few friends have been telling me about tmux and since I've been using a mac full time again (rather than ssh into linux) I haven't used a screen multiplexer. I wrote awhile back about <a href="http://www.rubygeek.com/2010/07/16/getting-around-with-gnu-screen/">Getting around with GNU Screen</a> and I really liked screen. I was listening to Changelog's podcast <a href="http://thechangelog.com/post/17827235767/episode-0-7-3-tmux-with-brian-hogan-and-josh-clayton">Episode 0.7.3 - Tmux with Brian Hogan and Josh Clayton</a> on one of my walks, it seemed useful and better than screen. Then I remembered hey it was kind of useful to use a screen multiplexer (BTW, I love saying multiplexer. I feel smart, like I know what I am talking about, hehe). I got home from my walk and installed it and started with the links on the changelog show notes and not wanting to get overwhemed by just copying someone conf file, I started simple and added a few things:
<!--more-->

``` bash
# change default command sequence·
unbind C-b 
set-option -g prefix C-a 

# Start Window Numbering at 1
set -g base-index 1

# Faster Command Sequences
set -s escape-time 0

# force a reload of the config file
unbind r
bind r source-file ~/.tmux.conf

# quick pane cycling
unbind ^A
bind ^A select-pane -t :.+ 
```

I used the default from screen, ctrl-a to go into command mode. Since that the combo used for screen, it seemed like a good idea. The second setting sets the numbering at 1, arguement for that is the 1 is probably closer to where you are typing and certainly closer to ctrl-a

The next 3 were just something I found, seemed like they'd be ok. Then I stopped adding to the conf and started trying it out.

I often alternate between textmate and macvim. I remembered from the podcast that one of the guys now just does entirely vim. Meh ok, I only like a few of the things in macvim, mostly scrolling and clicking to change panes. So i opened my project up and started working away. 

I had a hard time figuring out how to get the split that one of the guys talked about, so once I found it made note of it:

``` bash
ctrl-a :
 split-window -p 25
```

I used this page alot and copy and pasted some things to my own cheatsheet

<a href="http://robots.thoughtbot.com/post/2641409235/a-tmux-crash-course">Thoughtbot - A tmux crash course</a>

One thing I really like is named sessions so I can have my own workspaces

``` bash
tmux list-sessions
fun: 3 windows (created Sat Feb 18 16:10:58 2012) [156x37]
koans: 1 windows (created Wed Feb 22 11:06:36 2012) [156x36]
work: 3 windows (created Sat Feb 18 16:38:46 2012) [156x37]
```

Great to remember what you were doing last! 

Fun is where I work on something fun. I was digging into rails activesupport source code and reading the tests. koans is where I work through rubykoans (having it in a session makes it quick to switch and work on a few at a time). Its split down the middle with my console on one side and the code on the other. Work is well work :) I have it split 75/25 with vim at the top, and bottom is just the shell where i type my git commands, ack/grep. I have a second window with my rails server log going. 

I even found a <a href="https://github.com/squil/pomodoro.rb">pomodoro timer</a> that runs in the console.

Its working out good. I've started to work fullscreen  and keep clutter out of slight.  I think its a smoother flow than screen and I wish I had dived in sooner. 

If you haven't tried a screen multiplexer give it a shot! Remember to start with a few commands at a time so you dont get overwhelmed! 

Edited to add:

Tmux remote pairing scripts

<a href="https://github.com/mikehowson/tmux-remote-pairing-scripts">https://github.com/mikehowson/tmux-remote-pairing-scripts</a>

