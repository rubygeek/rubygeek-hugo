---
categories:
- practice
- legacy code
comments: true
date: "2012-10-16T00:00:00Z"
title: Better code through Code Reviews
---

For one of my projects we have started doing code reviews. We use github, work in a branch, then submit a pull request for review. Looking at github you can see a nice diff of the changes and you are able to add comments. So github is one way. You could also just use 
```
git diff  master..myawesomebranch 
```
And get a decent diff.

I prefer to code review live if the other person is available in person (screensharing would work too). It makes it easy to ask why this, and did you consider this and then after discussion you can make a note of what to change. 

Its super easy to nit pick at someones code, after all programming is alot of your personal style. But there are sometimes where you say "umm ok thats different, cool lets move on" and sometimes where you can say "hmm, what do you think of this instead" like for example

```
  unless something == "something" && !this.method(that) do
     // this awesome thing
  end
```

yes it works (but I didn't question if it works or not, I'm thinking of humans reading this code later!), but lets consider:

```
  if something != "something" && this.method(that) do
     // this awesome thing
  end
```

I think its a little more readable, but when I make suggestions I like to open the door for discussion, and perhaps I don't understand all the code and it gives the other person to explain it. Sometimes they answer with "oh well i didn't write that part" .. That is OK!! when I do a code review I am taking it as you saying "hey this code was my responsibility, I added some code, fixed some confusing parts and submit it you for review."

I rarely do git blame to see who did something because it doesn't matter most of the time who did that weird bit of code. Most likely it was a build up of code and the author didn't stop to think about refactoring, or he/she was under time constraints. That's the normal evolution of programming and we are trying to break that habit with reviews. But if I find a confusing bit of code that I can't figure out, then maybe I'll do git blame to see who last edited that so I can go to them and ask for some help in understanding it. Coding is never a perfect process. This is why code reviews are good. 

I like code reviews alot and have done them at a few different companies and with people having the right attitudes it goes a long way to making better code.
