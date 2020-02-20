---
categories: javascript
comments: true
date: "2017-03-16T00:00:00Z"
title: using jshint
---

Javascript. It's a language that some of us love to hate, me occasionally. Here's a few notes to myself to make JS a little better :) 

Use these settings in JSHint to avoid instanity:

```
file: .jshintrc
{
  "flagname" : true
}
```

If you have settings that apply to only your test environment you can put those in `test/.jshintrc` (maybe allowing global var for your test function) .. you can extend a confiuration in another file with this option `"extends" : "../.jshintrc" 

Sometimes you just can't please everyone, including the linter. Here's how to ignore blocks or lines from the linter:

To ignore a block of code use 

```
/* jshint ignore:start */
myBadJsHere
/* jshint ignore:end */
```

to just ignore a single line:

```
myBadJsHere // jshint ignore:line
```

Obviously you shouldn't let the linter ignore code but you may have a good reason to do so. 


Handy thing to have in your .jshintrc file:

`eqeqeq`

1 != "1" right? comparing a number to a string? no, JS says sure its the same using automatic type conversion. Use this flag to warn you and remind you to use triple equals to not do automagic type conversion. 



Switch statements

This was just a neat thing I learned today,  in JS switch/case statements you need to do a return or break or you will fall through to the next option. Leave a note for the next guy that says you did mean to fall through with a comment `/* falls through */` and you will silence the warning from jslint. Kinda cool, huh?


Use Strict

`"use strict";` 

This tells javascript to cut the funny business and not be so helpful and error when stuff should error. 

I picked up some of these tips watching [Pluralsight: JavaScript Best Practices](https://app.pluralsight.com/library/courses/javascript-best-practices/table-of-contents)

Thanks for humoring me as I write these notes to myself :)
