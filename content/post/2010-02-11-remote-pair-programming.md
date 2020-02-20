---
categories:
- pairs
- productivity
date: "2010-02-11T00:00:00Z"
description: Methods and techniques for pair programming
title: Remote Pair Programming
---

Seems like Pair Programming is "all the rage" lately in my circles. I haven't exactly done it before but after hearing about the success and rapid knowledge growth amongst those that pair program...I was almost dying to try it! Especially after i saw <a href="http://davidchelimsky.net/">David Chelimsky</a> and <a href="http://coreyhaines.com">Corey Haines</a> at <a href="http://www.windycityrails.com">WindyCityRails</a> in Sept 2009. I saw them pair and do BDD with Rspec/Cucumber and it was so fascinating, It was like I was watching a ballet as they hopped from RSpec to Cucumber and back and forth. I was like, wow...I wish I was that good! I would have paid good money for a recording of that so I could watch it again and again! I  see <a href="http://coreyhaines.com">Corey Haines</a> traveling around pairing with people too. Some people get together and play cards, but Corey gets together to code!

So ok, I like code, I like people, I want to try it! I live a little south of Chicago so its a long commute and it seemed everyone was so busy to pair in person when I asked. I asked on Devchix mailing list for suggestions on how to do pairing online. I had found a few, and the group had some good suggestions. I even had a volunteer to try it with me! This week <a href="http://edendevelopment.co.uk/blogs/aimee/">aimee</a> and I set a few hours aside to try it and see if we could do it!

This article was also sort of "paired" as it was written from my perspective with input and suggestions from <a href="http://edendevelopment.co.uk/blogs/aimee/">aimee</a>!

We asked on Devchix mailing list for suggestions on how to do pairing online. I had found a few, and the group had some good suggestions. I even had a volunteer to try it with me! This week <a href="http://edendevelopment.co.uk/blogs/aimee/">aimee</a> and I set a few hours aside to try it and see if we could do it!

After introductions on Skype we set about getting a shared environment in which to code together. Ideally, we wanted some kind of desktop sharing so we could run tests, console and editor.

We had heard of a few tools and got suggestions from the devchix list:

* IChat desktop sharing - we couldn't get this to work, we did different things and it would appear to connect but then it failed. I tried to mess with settings for Sharing on mac, but nothing doing.
* [Rio](http://www.ordcamp.com/Home/session-ideas-2010/real-time-collaborative-apps-with-rio) seems to be a library to make collaborative apps, not to use in a pair programming environment.
* [BeSpin](http://bespin.mozilla.com) was hard to use.. we couldn't figure out exactly how to use it. It almost seemed to offer to import the git repository we were working on, but then it said it only supports Subversion and Mercurial, not git.
* [SubEthaEdit](http://www.subethaedit.net) worked but we would have to open each file individually and share each file...  unless I was missing something. This would be fine for collaborating on a single file but then we could not share the test runs, terminal commands or view the browser together.
* [Etherpad](http://etherpad.com) - we didn't end up trying this but I have used it before to debug some code or try out ideas with a friend. They recently got bought by Google, so it would be interesting to see what they do with it. This would suffer the same limitations as SubEthaEdit in that it's just a text editor.
* [GoToMeeting](http://gotomeeting.com) (which is $40-50/month) its a little steep for the open source work I want to do. But people say it works really well.

Then we came to <a href="http://www.teamviewer.com">TeamViewer</a> which worked brilliantly! We shared desktop and I could type in aimee's console window, see the tests running and type in textmate. Even with aimee on her Dvorak keyboard and I on Qwerty! I could type fine but couldn't copy/paste with keyboard shortcuts so I used the mouse to copy/paste and it worked fine.

All in all, it was an awesome experience and I picked up on a few tidbits of knowledge from aimee on git, and rake! I had some bits of code from another project i was able to quickly copy/paste and get us rolling. We had a few discussions about coding style as we went.

Since  <a href="http://edendevelopment.co.uk/blogs/aimee/">aimee</a> was more familiar with the codebase, she mainly wrote the behavioral specs and I wrote the code to satisfy them. We plan to switch around next time, when we pair on a different project that I've been developing for a while.

