---
categories:
- ruby
- practice
comments: true
date: "2013-04-27T00:00:00Z"
title: Exploring Arrays - Part 3
---

Continuing going through the array methods, using Ruby 1.9.3

## Element Reference

#### ary[index] → obj or nil
#### ary[start, length] → new_ary or nil
#### ary[range] → new_ary or nil
#### slice(index) → obj or nil
#### slice(start, length) → new_ary or nil
#### slice(range) → new_ary or nil

Ok, so we've got the "normal" way to access an array element, simply using the name of the array, [ index ]. If item is not found at index it returns nil. 

Then we have  passing a start index and a length. A friend was working on [rubykoans.com](http://rubykoans.com) had  question I couldn't answer - Why is ary[4,0] an empty array and not nil?

``` ruby
>> ary = %w(one two three four)
=> ["one", "two", "three", "four"]
>> ary[0,0]    #expected
=> []
>> ary.index(ary.last)    # index of last item is 3
=> 3
>> ary[4,0]    # not expected, no index of 4 exists ?? why return empty array 
=> []
>> ary[5,0]    # expected, no index of 5
=> nil
```

The docs say

```
Returns nil if the index (or starting index) are out of range.
```

So yes, thats very confusing... why is it returning an empty array for last_index+1 of the array??

Moving on .. Passing a range is another way to access elements

I tried accessing an index greater than the end and it works as expected
``` ruby
>> ary[0..3]                        # all the elements
=> ["one", "two", "three", "four"]
>> ary[1..2]                        # just index 1 and 2
=> ["two", "three"]
>> ary[1..5]                        # accessing off the end of array
=> ["two", "three", "four"]
```

It looks as though slice is alias for [ ] ... so everything should work the same.

## Element Assignment

#### ary[index] = obj → obj 
#### ary[start, length] = obj or other_ary or nil → obj or other_ary or nil
#### ary[range] = obj or other_ary or nil → obj or other_ary or nil

``` ruby
>> ary[0,2] = %w(uno dos)
=> ["uno", "dos"]
>> ary
=> ["uno", "dos", "three", "four"]
```

Starting at index 0 and for 2 elements, replace with a new array .. works! Never used that before! Cool

also you can use it to clear out values

``` ruby
>> ary
=> ["uno", "dos", "three", "four"]
>> ary[0,2] = nil
=> nil
>> ary
=> [nil, "three", "four"]
```

## assoc(obj) → new_ary or nil

This works on an array which is a collection of arrays, and this method will find the first array where the first key matches obj. Odd, not sure when I would use this.

## at(index) → obj or nil

Same thing as  ary[index] .. maybe it would read better, but I'm so used the  [ ] syntax, I will probably use that syntax

## clear → ary

Clear, removes all elements in the array. Didn't know this was there.

## collect/map
#### collect {|item| block } → new_ary 
#### map {|item| block } → new_ary
#### collect → an_enumerator
#### map → an_enumerator

Not the same collect that is in enumerable, it runs the block for each item in the array and returns an array of results.

``` ruby
>> %w(red blue green yellow orange).collect { |w| w.upcase }
=> ["RED", "BLUE", "GREEN", "YELLOW", "ORANGE"]

>> %w(red blue green yellow orange).map(&:upcase)
=> ["RED", "BLUE", "GREEN", "YELLOW", "ORANGE"]

>> %w(red blue green yellow orange).map(&:upcase).object_id
=> 70349437828620
>> %w(red blue green yellow orange).object_id
=> 70349434016460
```

You can give it a block or a "symbol to proc", that is the funny &: followed by a method name. When I first saw that I said what the heck is that? and what is it doing? I mean, how do you google for something like that? I finally found out what it was called..."symbol to proc" and now I use it everywhere it makes sense. So Map and Collect appear to be synonyms here. Did not know that... Usually, I use collect over map since that makes more sense to me, you are collecting results. 


That is all for this post, I can replace arrays in to arrays, now I know that collect and map are the same on arrays. Still there is the nagging problem of why on a 4 element array ary[4,0] returns an empty array instead of nil. Anyone know? 
 
Other posts in this series:

* [part 1](/2013/02/23/exploring-arrays-part-1)
* [part 2](/2013/03/11/exploring-arrays-part-2)

