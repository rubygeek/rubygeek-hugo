---
categories:
- refactor
- legacy code
comments: true
date: "2013-03-02T00:00:00Z"
published: false
title: Refactoring Legacy
---

Refactoring a legacy codebase is a daunting task, because you may have nothing visual to show for it after you are done. The code works the same as before. Its particularly confusing to convince management to let us take the time to refactor. "Why did you not do it right the first time?" they might say! I always advocate refactoring as you add new features but some are uncomfortable with that and only wanting to change one thing at a time. Accept the fact that no scenario is perfect and you have to start somewhere. Lets write some basic tests, refactor a bit, run the tests and then add new feature.



Is there times when you stop refactoring? Yes, there is. You come to a point where it is "Good Enough" and it is time to move on. What is good enough? one persons good enough is not another persons good enough. 

You have to start somewhere, so here's some tips.

If you don't have tests you can even do this one:

naming variables and methods - For whatever reason, the previous programmer named the method a cute name then left. Say you have a recipe application and the attribute of the recipe for ingredients is called "fixins". Cute, but the next person has to figure it out by context what "fixins" is. Rename that variable! Run ack or grep on your project to make sure all occurrences of "fixins" is gone.

The very nature of programming is a constant refactor process. We learn more about the project and requirements, we find that what we thought was good at one time turned out to not be so good. As programmers we are constantly learning more about our chosen language and learn more techniques. Don't feel guilty for refactorying code! :) 


