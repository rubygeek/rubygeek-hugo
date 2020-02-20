---
categories:
- ruby
- learning
comments: true
date: "2013-03-11T00:00:00Z"
title: Exploring Arrays - Part 2
---

My second blog post on the topic. Between the writings of this post and [part 1](/2013/02/23/exploring-arrays-part-1) Ruby 2.0 was released on its 20th birthday! But I am going to stick with Ruby 1.9.3 for this series, maybe after I am done I'll see what changed for 2.0

[Array Documentation](http://ruby-doc.org/core-1.9.3/Array.html)
http://ruby-doc.org/core-1.9.3/Array.html

BTW, to make an array you can use the %w and space separated list of values, I will use that here to save on typing.

## ary & other_ary → new_ary
    Set Intersection—Returns a new array containing elements common to the two arrays, with no duplicates.

Oooh this sounds like math, specifically [Set Theory](http://en.wikipedia.org/wiki/Intersection_\(set_theory\)). I did this in college but didn't remember the 'no duplicates' part so I looked it up. The definition for Intersection from wikipedia is "the intersection of two sets A and B is the set that contains all elements of A that also belong to B (or equivalently, all elements of B that also belong to A), but no other elements." So no room for duplicates there either.

Lets try it:

``` ruby 
>> cookie_ingredients = %w(butter eggs sugar salt)
=> ["butter", "eggs", "sugar", "salt"]

>> pancake_ingredients = %w(vanilla sugar baking_powder, eggs)
=> ["vanilla", "sugar", "baking_powder,", "eggs"]

>> cookie_ingredients & pancake_ingredients
=> ["eggs", "sugar"]
```

If I wanted to know what ingredients I need to be sure to get enough of for baking I could make some arrays like that and get the intersection of them. Whoa I thought & for bitwise operation? Well Yes it is!! But that is for Fixnum objects. With arrays the & is an intersection! Cool .. polymorphism for the win!


Next is the * operator with two usages:

## ary * int → new_ary
## ary * str → new_string
    Repetition—With a String argument, equivalent to self.join(str). Otherwise, returns a new array built by concatenating the int copies of self.

Lets try the * int out first:

``` ruby
>> week = %w(Sunday Monday Tuesday Wednesday Thursday Friday Saturday)
=> ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]

>> week * 4
=> ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
```

I made an array for days of the week, then used * 4 to make a 4 week month. Seems pretty awesome to something like this without having to loop to build arrays.. if I didn't use that I would have to write code like this:

``` ruby
>> four_week = []
=> []
>> 4.times do
?> four_week << week
>> end

>> four_week
=> [["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"], ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"], ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"], ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]]
```

Hmm can't figure out a way to make it a single array, so I call flatten

``` ruby
>> four_week.flatten
=> ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
```

That all being said, if you want to combine an array 4 times, use * and it is easy :)


There's other ways that might be better, but you get the idea. Much shorter to use the * int :) lets try the other version.

``` ruby
>> title = %w(Samuel E. Jones)
=> ["Samuel", "E.", "Jones"]
>> title * " "
=> "Samuel E. Jones"
```
The parameter after the * is the character to join with, basically it is calling join.

``` ruby
>> title.join(" ")
=> "Samuel E. Jones"
```

I probably would use join since that makes it more clear to the reader what you are doing and it is more common. But if you are going for [ruby golf](http://en.wikipedia.org/wiki/Code_golf), then use the <tt>* " "</tt>

## ary + other_ary → new_ary

Array concatenation, simply adding two arrays.

``` ruby
>> meals = %w(breakfast lunch dinner)
=> ["breakfast", "lunch", "dinner"]
>> snacks = %w(snack1 snack2)
=> ["snack1", "snack2"]
>> buy_food = meals + snacks
=> ["breakfast", "lunch", "dinner", "snack1", "snack2"]
```

Easy, next array difference.

## ary - other_ary → new_ary
    Array Difference---Returns a new array that is a copy of the original array, removing any items that also appear in other_ary. (If you need set-like behavior, see the library class Set.)

``` ruby
>> colors = %w(red purple blue green yellow orange)
=> ["red", "purple", "blue", "green", "yellow", "orange"]
>> primary_colors = %w(red blue yellow)
=> ["red", "blue", "yellow"]
>> secondary_colors = colors - primary_colors
=> ["purple", "green", "orange"]
```

Also pretty straight forward. 

## ary << obj → ary
    Append—Pushes the given object on to the end of this array. This expression returns the array itself, so several appends may be chained together.

Commonly called the shovel operator you can add something to the end of the array, and since it returns an array you can do it multiple times

``` ruby
>> colors = []
=> []
>> colors << "red"
=> ["red"]
>> colors << "blue"
=> ["red", "blue"]
>> colors << "green" << "orange"
=> ["red", "blue", "green", "green", "orange"]
```

Chaining << is just like chaining other methods:

``` ruby
>> "first name".gsub(" ","_").upcase
=> "FIRST_NAME"
```

which is basically

``` ruby
>> colors.<<("red").<<("green")
=> ["red", "green"]
```

without the extra parenthesis. Cool, I hadn't realized you could chain << before now!

Next, a method with a funny name. <=>

## ary <=> other_ary → -1, 0, +1 or nil
    Comparison—Returns an integer (-1, 0, or +1) if this array is less than, equal to, or greater than other_ary. Each object in each array is compared (using <=>). If any value isn’t equal, then that inequality is the return value. If all the values found are equal, then the return is based on a comparison of the array lengths. Thus, two arrays are “equal” according to Array#<=> if and only if they have the same length and the value of each element is equal to the value of the corresponding element in the other array.

I've only seen this used when writing custom methods to determine equality, but it seems pretty straight forward. I will play with it when I play around with enumerable. 

## ary == other_ary → bool 
    Equality—Two arrays are equal if they contain the same number of elements and if each element is equal to (according to Object.==) the corresponding element in the other array.

``` ruby
>> [1,2] == [2,1]
=> false
>> [1,2,3] == [1,2]
=> false
>> [1,2] == [1,2]
=> true
```

To be equal they have to have the same number of elements and same ordering of elements. The first and last examples show that.

I didn't learn anything earth shattering this time, but it is a good review of some basic arrays and set operations.



