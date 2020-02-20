---
categories:
- ruby
- learning
comments: true
date: "2014-08-28T00:00:00Z"
title: Exploring Arrays Part 5
---

BTW Going to use Ruby 2.1.2 from here on out

Yes, it has been awhile but (who has time to dwell on the past?)  I haven't forgotten I was going through each array class! My [last post](/2013/09/02/exploring-arrays-part-4/) said next was delete, so let's get to it!

## delete

    delete(obj) → item or nil 
    delete(obj) { block } → item or result of block


Ok first one looks pretty straight forward, it returns item if it found it and nil if it doesn't. Lets try it out:

```ruby
>> colors = %w(red blue green yellow)
=> ["red", "blue", "green", "yellow"]
>> colors.delete("blue")
=> "blue"
>> colors
=> ["red", "green", "yellow"]
>> colors.delete("can't find me")
=> nil
>> colors
=> ["red", "green", "yellow"]
```

I deleted "blue" and it found it because it returned "blue". The array is now changed, contains no "blue". When you try to delete a value which is not there, "can't find me" it doesn't throw an exception, just returns nil and does not change the array. What happens if the array is empty? Lets say I made myself a todo list and deleted thing as I finished them:

```ruby
>> todo = ["get up", "drink coffee", "study ruby"]
=> ["get up", "drink coffee", "study ruby"]
>> todo.delete("get up")
=> "get up"
>> todo.delete("drink coffee")
=> "drink coffee"
>> todo.delete("study ruby")
=> "study ruby"
>> todo.delete("do it again")
=> nil
>> todo
=> []
```

So nil is the result when the item is not found OR the array is empty. So, don't depend on the return value too much since it can be nil for multiple reasons. Delete is not a great method for checking off things in your todo list!


## delete_at


    delete_at(index) → obj or nil

Where as delete would delete from the array at the value, delete_at will delete at the specified index.

```ruby
>> colors = %w(red orange yellow green blue)
=> ["red", "orange", "yellow", "green", "blue"]
>> colors.delete_at(3)
=> "green"
>> colors
=> ["red", "orange", "yellow", "blue"]
```

Pretty simple, let see what happens when you try to delete at an invalid index:

```ruby
>> colors.delete_at(100)
=> nil
```

So once again, if its an invalid index you get nil just like if there is an invalid value, you get nil. Lets move on!


## delete_if


    delete_if { |item| block } → ary
    delete_if → Enumerator


So looks like if takes a block that tells us what items will be deleted from the array. Lets delete all color names that are greater then a length of 5 characters.

```ruby
>> colors = %w(red orange yellow green blue)
=> ["red", "orange", "yellow", "green", "blue"]
>> colors.delete_if { |color| color.length > 5 }
> ["red", "green", "blue"]
```

So it deletes from the array, every element for which the block is true. And its true for values orange, yellow and green. Leaving us with red, green and blue. The docs say its the same as reject! ... yay for being like php! 


## drop

    drop(n) → new_ary

So it takes the array and removes the first N elements and returns the rest.

```ruby
?> colors = %w(red orange yellow green blue)
=> ["red", "orange", "yellow", "green", "blue"]
>> colors.drop(2)
=> ["yellow", "green", "blue"]
```

We drop the red and orange elements, and return the rest which is yellow, green and blue. Pretty straightforward, Ok, next method!

## drop_while

    drop_while { |arr| block } → new_ary
    drop_while → Enumerator

```ruby
=> ["red", "orange", "yellow", "green", "blue"]
>> colors.drop_while { |color| color.length != 4 }
=> ["blue"]
```

This one drops all elements for which the block is NIL or False. So if we wanted all the colors whose length was NOT 4, we would only get one element of blue.

## each


    each { |item| block } → ary click to toggle source
    each → Enumerator


This one is easy, we use it all the time..

```ruby
>> colors = %w(red orange yellow green blue)
=> ["red", "orange", "yellow", "green", "blue"]
>> colors.each { |color| puts color }
red
orange
yellow
green
blue
```

## each_index

    each_index { |index| block } → ary click to toggle source
    each_index → Enumerator


Whereas ```each``` gives you the value each time, this gives you the index. 

```ruby
>> colors = %w(red orange yellow green blue)
=> ["red", "orange", "yellow", "green", "blue"]
>> colors.each_index { |color_index| puts color_index }
0
1
2
3
4
```


## empty?

    empty? → true or false


This checks if there are any elements in the array, I experimented with nil, empty or whitespace and the only thing that returns true is no elements. 

```ruby
?> [nil,nil].empty?
=> false
>> ["",""].empty?
=> false
>> [" "," "].empty?
=> false
>> [].empty?
=> true
```

This seems like a good place to stop, next blog post will start with eql? 




