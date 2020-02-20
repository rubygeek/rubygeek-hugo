---
categories: git
comments: true
date: "2010-02-17T00:00:00Z"
title: Gitting better with Git
---

In the past few weeks I've been doing alot of Git and alot more collaboration with people on projects! So I wanted to point out some of the things i've learned and some handy tips I picked up.

I was [pairing](/2010/02/11/remote-pair-programming/) with <a href="http://edendevelopment.co.uk/blogs/aimee/">aimee</a> remotely and she showed me how to "stage" a commit. Which is a way to indicate "HEY this is going to be committed when you do git commit!" I used to always do commit -a  which adds all the files not currently in the staging. I used perforce for awhile and learned how nice it was to have a "change list" and how you can commit only certain files at a time. Git seems like you can only have one "change list" at a time but that is fine in most cases.
<code>
git add -p
</code>
It will go through each changed file and ask if you want to stage this hunk. We were able to see what files we had changed as it showed a diff. You can confirm or deny chunk by chunk. today I like to run it to make sure I didn't accidentally change a file. I have a habit of leaving a file open, walking away and coming back and adjusting the spacing. Probably not something I need to in a commit.

```
git status
```

This is really cool command, not only does it show you what is in staging it tells you what to do with the other files to either add/remove etc to get the commit in the shape you want.  After working with git more, i went back and re-watched the [peepcode screencast](http://peepcode.com/products/git) on git. I understand it better now!

After we committed, we realized we made a mistake in log message, so we did
```
git commit --amend
```

It will amend the last message in your git repo. I recently tried this with one of my commits, but I had already pushed to origin. I was kind of confused, so I asked aimee, she said

<blockquote>
no If you have already pushed, you can git push --force to update, but be careful because it destroys references, so if there's a chance someone else may have pulled the commit in the meantime you won't want to go pulling the rug from under their feet, y'know! ;)
</blockquote>


Another sticky point for me was -- what is inverse of git add? git rm? I keep a [git repo](http://github.com/rubygeek/rubygeek) of code I am working on as I to learn stuff, practice etc. Most if it is probably not really worth browsing, but i like to point my friends there when I talk about what I am doing.  I wanted to clean up some stuff that I don't want to keep and mistakenly thought if I git rm it would remove it from the staging. Nope, it removes it! Well, to revert a file back to what you use this:

```
git checkout -- file.txt
```

more on this [Why is "git rm" not the inverse of "git add"?](https://git.wiki.kernel.org/index.php/GitFaq#Why_is_.22git_rm.22_not_the_inverse_of_.22git_add.22.3F)


Anyways, a few tidbits of information I've learned recently, hope this can help you git better at git :)

sources  I found helpful
* [Git FAQ](ttp://git.wiki.kernel.org/index.php/GitFaq)
* [Git Community Book](http://book.git-scm.com/index.html)
* [Pro Git, book by APress, content free online](http://progit.com/book)
