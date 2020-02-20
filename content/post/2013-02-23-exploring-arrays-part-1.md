---
categories:
- ruby
- learning
comments: true
date: "2013-02-23T00:00:00Z"
title: Exploring Arrays - Part 1
---
Everytime that I have to lookup something with hashes or arrays, I find lots of methods that I didn't know about, so I thought I'd do an experiment where I will learn about each one and write some examples and try to understand how to use it. I kind of picture someone sitting next to me as I do this, reading the docs and trying them out with me. 

[Ruby 1.9.3 Array Documentation](http://www.ruby-doc.org/core-1.9.3/Array.html)

## Class Methods


Class methods always start with the name of the class, dot, followed by method name. For example, we can
call Time.now as a class method on the Time class, which returns a new Time object initialized with the current time.


Docs say:
## \[\]\(*args\)
    [](*args)
    Returns a new array populated with the given objects.

There are three usage examples in the docs, lets try them out and see what they do, first:

``` ruby
>> Array.[](1)
=> [1]
>> Array.[](1,2,3)
=> [1, 2, 3]
```
The method name is [] and the params is *args which is the splat operator which takes any number of objects. Interesting I've never seen this used in the real world. I am sure its used cleverly, I just don't see it! I showed this to a friend [Danielle Sucher](http://daniellesucher.com) and she said:

<blockquote>
  I actually always think it's nifty that something like
  a[0] can be called with a.send(:[], 0) instead. Makes it possible to
  do stuff like a.try(:[], 0) or a.tap{}.send(:[], 0).  
</blockquote>

Thanks!! Moving on..

``` ruby
>> Array['a','b','c']
=> ["a", "b", "c"]
```
Returns array with the contents of *args just as it says. This is one way you can be "confident" of your code as Avdi talked about in [Confident Coding](http://www.youtube.com/watch?feature=player_embedded&v=T8J0j2xJFgQ#!) . If you want an array, make sure its an array! if its not an array it will put it inside an array like this:

``` ruby
>> Array(1)
=> [1]
>> Array([1])
=> [1]
```

Parentheses is optional in ruby so these are the same:

``` ruby
>> Array 1
=> [1]
>> Array [1]
=> [1]
>> Array([1,2,3]) == Array[1,2,3]
=> true
```
Looks funny without the () doesn't? I almost always use () for methods. Helps later on if you decided to add more things on that line you don't have to go back and add them. See [Avdi's RubyTapas Episode 024 Incidental Change](http://devblog.avdi.org/rubytapas-episode-list/) $9 a month screencasts that come out 3x week, so good!! Sign up now! :)

And lastly, the way you might normally create arrays, with [ *args ]. I tried to dig through Kernel to see if [] was a method on kernel that would call Array.new but I couldn't find anything. So I am still puzzled about how this actually works. Maybe a reader has an insight?

``` ruby
>> [1,2,3]
=> [1, 2, 3]
```


## Array.new

    new(size=0, obj=nil)
    new(array)
    new(size) {|index| block }
    Returns a new array.


So if no params, default is size 0 and value of index 0 is nil

``` ruby
>> a = Array.new 
=> []
>> a[0]
=> nil
```

Creates an empty array. Default value is nil.

If you pass a single param, it makes an array of that size. 

``` ruby
>> a = Array.new(5)
=> [nil, nil, nil, nil, nil]
>> a.size
=> 5
```
Creates an array with 5 elements with the default value is nil

If we pass two params, the second is the default value of the empty elements

``` ruby
>> a = Array.new(5, "-empty-")
=> ["-empty-", "-empty-", "-empty-", "-empty-", "-empty-"]
>> a[0]
=> "-empty-"
>> a[1]
=> "-empty-"
>> a[0].object_id == a[1].object_id
=> true
```

So the default value of each element is the string "-empty-" and they are in fact the same object. 

Lets create an array of basic family members, adding it to her_side and his_side. Then I added another family member to his_side and prove they are copies of the parents and not the same object. 

Second version is passing an array to Array
``` ruby
>> parents = ["mom", "dad"]
=> ["mom", "dad"]
>> her_side = Array.new(parents)
=> ["mom", "dad"]
>> his_side = Array.new(parents)
=> ["mom", "dad"]
>> her_side == his_side
=> true
>> his_side << "grandma"
=> ["mom", "dad", "grandma"]
>> her_side == his_side
=> false
```


And finally the third form uses a block:

``` ruby
>> b = Array.new(5) { |index| "-#{index}-" }
=> ["-0-", "-1-", "-2-", "-3-", "-4-"]
```

Interesting, maybe if you wanted an array like this


``` ruby
>> b = Array.new(5) { |index| index* 5 }
=> [0, 5, 10, 15, 20]

```

Cool!

## try_convert(obj) -> array or nil
    Tries to convert obj into an array, using to_ary method. Returns the converted array or nil if obj cannot be converted for any reason. This method can be used to check if an argument is an array.

``` ruby
>> Array.try_convert(1)
=> nil
>> Array.try_convert([1,2,3])
=> [1, 2, 3]
```
Hmm, so I guess this would be good if you only want to return array if its an array and nil otherwise. Never seen it used myself but it be a way to write confident code if you only want to respond to nil or array and use a bunch of if statements.
    
Cool, I had fun poking at ruby array class methods and trying things out.  Be sure and checkout the gotchas in the docs. I hope I was able to shed some light or at least make you think about some of the public methods of arrays. If you have some insight on how some of these work, please leave a note in the comments.

Thank you to [Danielle Sucher](http://daniellesucher.com) [@DanielleSucher](https://twitter.com/@DanielleSucher) for helping by proof-reading this post as I am working on improving my writing. 

Next post in this series will be on instance methods!





