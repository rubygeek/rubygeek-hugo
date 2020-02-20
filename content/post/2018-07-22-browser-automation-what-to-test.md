---
categories:
- automation
- robot framework
- jenkins
comments: true
date: "2018-07-22T00:00:00Z"
title: Automation - What to test?
---

Recently I've been working with a QA team and I've learned how to test the browser from them and have came up with some of my own ideas learning from Trial and Error. I have spent close to 1.5 years doing Automation Testing whereas previously I've always been a test obsessed developer with little in the way of browser testing.

This post is focusing on testing the browser and automating the tests.

I've already wrote about how I [setup Robot Framework](http://www.rubygeek.com/2018/07/05/my-robot-framework-setup/). 

### But what to test? 

Here's some questions to think about:

What button or link on my page makes money either directly or indirectly? It could be a Buy Now button or a subscription to a newsletter. Those are candidates to test.

Which thing on the page which didn't work, would make us look stupid? Ok you want to have some fancy dropdown that doesn't look like an HTML dropdown. But then it doesn't work. As a user, I would probably be so mad I would leave the site. I don't really have time to waste on stupid sites.

Things are that easy to test -- why not? Contact us link, about us, FAQ. Make sure those links are going to the right place. 5 minutes of your time gives you the confidence your site is functioning. Test that there is a valid url and when you go to it something shows up to prove you are on the right page, wether a H1 or page title. 

This is easy, test your copyright date if you have it :) 2018 is over half over and I'm still seeing current sites with 2017. Use some scripts to get the current year and check it.

Test the navigation the user might use, make sure each link is properly labeled and goes to the right page.  

### What do you hate testing Manually?

If it is a complex work flow of choosing multiple options and certain buttons to get to a certain page. That is tedious and should be automated. Not only do you make it "repeatable" but you won't forget steps later when you haven't worked on the site for some time.

### What not to test?

Things that don't break often. Sorting a list alphabetically? Probably not going to break. Computers are good at that. If you insist on testing this on the frontend  a good way to do it is to get all the items in a collection and sort it , then compare the order to what is on the site. Sorting by date is another thing that probably can be covered in unit tests and not worth testing. Remember we are trying to test as a user.

Things already implemented in unit tests. Most of the time you don't want to double test. For things like cookies/security tokens, etc you do want to double test on both frontend and backend. Just to be sure. 


### Use Jenkins

Setup Jenkins to run your tests. I [wrote about it](http://www.rubygeek.com/2018/04/09/setting-up-jenkins-for-robot-framework/). If you did all this work to write them set them up to run and emails you on failures or ping a chat room or something. Jenkins makes it really easy.

Write a test you want to maintain in future. If you are doing something insanely complex and have a good reason, comment it to save some stress for the next guy working on your tests. Or better yet, just write tests as simply as you can. Err on the side of maintainability instead of cleverness. Your future self will thank you.


### Use IDs or Descriptive Class Names

For the Love of Coding, please .. if your Devs won't give descriptive IDs (ie `nav`, `main`, `sidebar` etc) then at least get them to add some class names that describe what it is and not "how" .. a "how" classname might be `text-left`. Sure thats great for your designer, but that classname doesn't help the QA person.

If adding IDs or Classnames won't get you what you need, ask them to add custom attributes (allowed now in [react 16](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html)).

### Conclusion

Browser testing is frustrating but it is a much needed type of testing that everyone should have at least in some aspect. Ignoring these tests because they are a PITA is not a great idea. I hope I've given you some ideas on how you might start implementing it on your own sites. 