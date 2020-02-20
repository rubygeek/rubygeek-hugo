---
categories:
- ruby
- python
comments: true
date: "2017-03-10T00:00:00Z"
title: Ruby like Python
---

I did python.. awhile back, it was like ruby in some ways and in some ways not. So I picked one, I went full blown into Ruby. Recently I've seen lots of job postings for Python so I figured I'd give it another go :) 

## Slicing

In Python:

``` python
>>> message = "hello world"

>>> message[0:5]
'hello'

>>> message[6:]
'world'
```

Then in ruby

``` ruby
2.4.0 :001 > message = "hello world"
 => "hello world"

2.4.0 :002 > message[0..5]
 => "hello "

2.4.0 :003 > message[6..-1]
 => "world"
```

Eveything in ruby is an object and objects have methods. String has a method slice, aliased to `[]` (cool!) and the `..` is a range.  You could use a comma but there is so way to tell it to use the end unless using a range. If you look at the params for the slice method it is a `fixnum` or `fixnum, fixnum` or a range. It can also take a string (if you use the method name) but that is a strange use. 

``` ruby
2.4.0 :005 > message
 => "hello world"

2.4.0 :006 > message.slice("ello")
 => "ello"

2.4.0 :007 > message
 => "hello world"

2.4.0 :008 > message.slice!("ello")
 => "ello"

2.4.0 :009 > message
 => "h world"
```

Unless you use `slice!` (bang) does it actually change/do anything remotely interesting. Probably I would find a more clear way to do it than using slice with a string. 

Well this is just one aspect of the differences and I am not sure if there are differences in the versions of python too.


