---
categories:
- testing
- capybara
- cucumber
- books
comments: true
date: "2014-03-09T00:00:00Z"
title: 'Book Review:  Application Testing with Capybara'
---

If you know me, you know I love testing. Model tests, library tests (typically when poro objects) are the most awesome to test and I love it. Controller tests, well, are less fun and testing routes is not bad either. But my least favorite are integration tests.

Cucumber tests seemed awesome in theory and I remember early days in Chicago ruby group there was development on rspec story runner which I believed was written by Dan North, later rewritten as Cucumber. Wow!! Project owners could write their own tests!! Right... Or maybe they could read the tests and say yeah that's what I want. Yes, that was 2007. I've toyed with cucumber a few times and in frustration gave up. Even when using capybara, it was hard to get the right syntax and find accurate documentation. 

Last fall there was a few talks I watched Capybara and Cucumber, Rspec ... One project I work on they use Selenium. Ahh so many choices. How do I know what exactly is the difference between each of them and when do I use what? 

I was pretty excited to get a review copy of [Application Testing with Capybara](http://amzn.to/2rokPtT) ... ok, lets give this another shot. 

The book starts out with installation, then proceeds into an example of using cucumber to run a test on Youtube, I think thats a great start. Lets focus on writing a test for something we didn't build and get our setup and syntax down. And here is where we see the Capybara uses Selenium as the driver. I was able to follow along and build a test that does a search on youtube. Cool, you know getting something working while you are learning is a great boost!

The second chapter is about Mastering The API ... this is where I got hung up on the most, seeing different tutorials, docs, examples with different versions (most examples don't tell you what version numbers of Capybara is used!) and it is just frustrating. Capybara can use XPath or CSS selectors to locate elements. The author prefers the CSS selector style because he thinks they are more readable, I agree! You can set your preferred style in your env.rb but you can always override it on a case by case basis.

The book continues with examples of all types of selecting items and filling in forms. One thing that I found helpful is it shows the syntax of the example queries and Rspec matchers

```ruby
# example selectors
page.has_selector? '#local_results'

# Rspec matchers
page.should have_selector '#local_results'
```

This is one of the things that I think always tripped me up, I didn't know there was two ways to do the same thing and they were both correct, it is just different styles. This is a great resource! The book also talks what driver to use when and how to tag tests that would have client side code. Great content. 

Today I wrote some Cucumber tests for a project I am working on and I used this book. I got things setup set up correctly and looked up some syntax. I am happy to report success. If you want to work with Capybara be sure to grab a copy of [Application Testing with Capybara](http://amzn.to/2rokPtT) :) YAY!

