---
categories:
- ruby
- practice
comments: true
date: "2012-09-06T00:00:00Z"
title: Playing with RubyKoans
---

From time to time I run through the rubykoans...each time remembering or picking up something new. I'm such a dork, I keep a log of the dates in a spreadsheet. I first did these in Dec 2009, then again in July 2010, May 2011 and now I am going through them again. I remember the first time I did a few, I was amazed "how did they do that!?!?!" and dug through the source to see how it works. I immediately emailed Joe O'Brien and Jim Weirich at [edgecase.com](http://www.edgecase.com) and thanked them! Joe thanked me for my kind words and Jim said they were inspired by the book Little Lisper. 

When I first did these in 2009, it was a github repo that you cloned. Now you can go to [www.rubykoans.com](http://www.rubykoans.com) and download a zipfile or work on them through the browser! Few months ago I was bored with no real computer, so I attempted the browser based koans on my Samsung Tablet. It was kinda hard to do the special symbols so I don't really recommend doing it on a tablet unless you merely wanted to kill time like I did. :)
<!--more-->

I sometimes complete each test, then run in true TDD. Or I may try to complete the whole file and try to get it right the first time. Or sometimes I run autotest.

Things that stood out in this time around:

If two symbols are the same, then they have the same object id. See this in irb:
```ruby
>> "nola".object_id
=> 70210069961800
>> "nola".object_id
=> 70210069955500
>> :nola.object_id
=> 462408
>> :nola.object_id
=> 462408
```

Two strings, though they have the same value, have different ids because they are separate objects ... symbols are the same since Ruby doesn't make another instance of the symbol. Cool, that makes more sense and another reason to use symbols over strings for things like constants etc.

I always forget you can do parallel assignment with arrays: 
```ruby
first_name, last_name = ["John", "Smith"]    
```

And these bits on nil are always good to refresh on, since nil is an object unlike most other languages and use the .nil? method if you want nil!

```ruby
>> nil.is_a?(Object)
=> true
>> "".nil?
=> false
>> nil.nil?
=> true
```

If you haven't tried [rubykoans](http://www.rubykoans.com) yet, what are you waiting for!!