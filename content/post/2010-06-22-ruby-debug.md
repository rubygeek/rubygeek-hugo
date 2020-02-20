---
categories: ruby
comments: true
date: "2010-06-22T00:00:00Z"
title: Ruby-Debug
---

I never really used a debugger until I did Java. Before that I did what was called "PHP Debugging" which is littering your code with print statements until you figure out what you need to know, then going back and making sure you deleted them! In php I sometimes would put a unique string like 

print "QWE" . $myawesomevar 

Or something like that, then going back and searching your code for QWE (just picked something that was easy to type!). But all that gets rather annoying. They might have nice debuggers for PHP now, I haven't really done php in 4-5 years. I really didn't understand the power of breakpoints until I did java and I thought that was pretty cool. Now that I do all ruby, I sort of miss eclipse (I didn't actually think I'd ever say that...) since all I use right now is textmate and vi which don't have the built debugger support that eclipse does. Some of the newer ruby editors might, I haven't spent any research on them.

I found ruby-debug and it works great! You can set a breakpoint and then climb inside your application and peek around and see what is going on. I've done this in controllers and even in test mode. 

If you use passenger, you can still use it but its a little tricky to setup. This <a href="http://duckpunching.com/passenger-mod_rails-for-development-now-with-debugger">blog post</a> from duckpunching.com  has some neat tips and a rake task to make it easier. Be sure to start the rdebug -c  from the console after you have set a breakpoint and reloaded your page.

If you are using passenger, you can't use the irb command inside of the ruby-debug console, but I found that most of the time just being able to poke around the variable space inside my app has been enough for me to understand what is going on.

There are quite a few commands available in the debug console, but these are the ones I've used:

* help - view the list of commands
* l list  -  this shows context around the current line the debugger is on
* n next - moves to the next line
* p print - print a variable
* irb  jump to console - I dont use this very often and it doesn't work if you are using passenger
* c cont - continue to end of breakpoint or end of page load, after I have seen what I want to see I do this to get out of the console

Resources
<ul>
  <li><a href="http://railscasts.com/episodes/54-debugging-with-ruby-debug">Railscasts Episode: Debugging with ruby-debug</a></li>
  <li><a href="http://duckpunching.com/passenger-mod_rails-for-development-now-with-debugger">Duckpunching.com how to use ruby-debug with passenger</a></li>
</ul>

Anyways, hope some of you find it useful and happy debugging :) 

Update: I wrote this in preparation for a short talk at <a href="http://austinonrails.org">AustinOnRails</a> while giving the talk, the group had some suggestions:

put in a ~/.rdebugrc
```
set autoeval
set autolist
```

autolist - will run list command when you enter
autoeval - will eval each line, so instead of "eval @profile" to get it to print, it will automatically eval

doing  m [object] can show methods in a nice table format. Thats cool, i have often gone to irb and did String.methods.sort but that is kinda hard to read. 

Anyways, good stuff! 
