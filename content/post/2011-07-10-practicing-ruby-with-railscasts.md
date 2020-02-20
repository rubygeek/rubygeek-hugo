---
categories:
- ruby
- practice
date: "2011-07-10T00:00:00Z"
description: One of the techniques I use to practice Ruby
title: Practicing Ruby with RailsCasts
---

I recently went on a small vacation and finally had some time to catch up on some [RailsCasts](http://www.railscasts.com)! I like to setup a project and "play along" with Ryan Bates and pause/play and work through the examples. With all my experimentation/playing I keep a git repo at http://github.com/rubygeek/rubygeek where I keep code for reference later. Sometimes when trying to remember if i did something, I grep a keyword or two on the root directory and see if I have done something with it before. I can put notes in the readme file for each project, so I can remember what resources I used.

[Guard](http://www.railscasts.com/episodes/264-guard)
Setups up monitoring for files to run something like tests, compile coffee script or sass. The cast shows how to use it to run your rspec tests. It shows liverefresh, with that and a plugin it will automatically refresh your browser\!

[Ancestory](http://www.railscasts.com/episodes/262-trees-with-ancestry)
I spent a good deal of time using acts_as_commentable_with_threading and although its pretty good, I think now I will change to using this, it seems like it will be more efficent on queries and has more scopes defined.
<!--more-->
[Kaminari](http://www.railscasts.com/episodes/254-pagination-with-kaminari)
This is totally awesome! I did have to do some digging to figure out will_paginate for rails3, and this is way better as it is a rails engine, so its soooper easer to customize. The labels for the next/previous are stored in the language file \( ie.  en.yml \) so if you support multiple languages, this is awesome. If you need to customize beyond modifying css, you can run the rake command to generate the views. 

[Resque](http://www.railscasts.com/episodes/271-resque)
I might need to do something like this soon for a project, so I was happy to see this. It was pretty easy to do and I like the admin interface to see what is going on. Even more cool, is it is written in sinatra and you can hang it of your rails app if you want and assign a route to it. Definitely a fun screencast to follow along with\!

[Template Inheritance](http://www.railscasts.com/episodes/269-template-inheritance)
Finally, this makes perfect sense. Available in Rails 3.1 you can have an Application directory in the views directory and there you can place things common to all views, like header, footer, sidebar etc then if you need it customized footer for say the Products page, you can put a footer partial there and that is used on product pages. Previously you had to make a directory like common, shared. etc etc and it always seemed hackish to me.

Recently Ryan has put before and after version of the code in his gitrepo, and though I usually try to do it all myself, I did grab his before code for the Template Inheritance.


Thats about all for now, give those a try and practice ruby!

