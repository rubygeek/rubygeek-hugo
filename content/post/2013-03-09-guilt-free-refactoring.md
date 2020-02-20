---
categories:
- refactor
- legacy code
comments: true
date: "2013-03-09T00:00:00Z"
published: false
title: Guilt Free Refactoring
---

I suspect many of you have been in the same boat as myself. You write some awesome code to solve a problem...sometime goes by and then you learn a new, better way to achieve the same outcome. Major facepalm. Or maybe after you are done you remember this great technique that totally didn't cross your mind to use. Yes, nobody is perfect. Maybe you've inherited some legacy code that makes you want to cry yet you don't want to make a big deal about how bad it is. Well, we can still refactor and not feel bad about it. Here's some small techniques that I've recently been thinking are very do-able and you can do in your project now and not feel guilty about.


# Rename Variables, Methods, Classes
I think if you do this carefully enough, you don't need tests before you do this...just good grep/ack skills to find all uses of the variable. Of course I've always heard "make the name sensible" and I was reminded about this when reading the book Clean Code by Bob Martin (@unclebob)

# Moving bits of code into a presenter method



# Call partials with Values
Maybe you haven't done this but I have. /facepalm

``` ruby
render partial: sidebar
```
I think for some versions of rails you don't need to put the keyword partial, but when I've tried it recently it didn't work. I had to have the keyword partial.

``` ruby
<div id="sidebar">
  <div class="links">
    <% @links.each do |link| %>
      <li><%= link.name %></li>
    <% end %> 
  </div>
  <div class="links">
    <% @recent_posts.each do |post| %>
      <li><%= post.name %></li>
    <% end %> 
  </div>
</div>
```

instead of referencing the values passed into the partial, like this;

``` ruby
render partial: sidebar, links: @links, recent_posts: @recent_posts
```

``` ruby
<div id="sidebar">
  <div class="links">
    <% links.each do |link| %>
      <li><%= link.name %></li>
    <% end %> 
  </div>
  <div class="links">
    <% recent_posts.each do |post| %>
      <li><%= post.name %></li>
    <% end %> 
  </div>
</div>
```

I think this is the intended way to access data in partials and it makes it very clear when looking at the use of the partial what data it is accessing. And the partial code looks cleaner without the @ and if you decide to rename the instance variable, you only need to rename the reference in the partial not every use of it inside the partial. Win win!!
