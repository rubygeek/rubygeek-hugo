---
categories: null
comments: true
date: "2015-06-06T00:00:00Z"
title: Exploring Arrays Part 6
---

Continuing on through the array methods... now using `ruby 2.2.0preview1` :)

## eql?

    eql?(other) → true or false
    Returns true if self and other are the same object, or are both arrays with the same content (according to Object#eql?).

Lets try it on structs first. A struct is an easy way to make an object in ruby. First I define a struct of a User with two attributes:

```ruby
irb(main):001:0>  User = Struct.new(:name, :age)
=> User
```

Ok now lets create some objects

```ruby
irb(main):002:0> bob = User.new("Bob", 35)
=> #<struct User name="Bob", age=35>

irb(main):005:0> bob2 = User.new("Bob", 35)
=> #<struct User name="Bob", age=35>

irb(main):006:0> bob.eql?(bob2)
=> true
```

Ok we have two objects, same data name and age and eql? returns true.


```ruby
irb(main):007:0> bob3 = User.new("Bob", 32)
=> #<struct User name="Bob", age=32>
irb(main):008:0> bob.eql?(bob3)
=> false
```

Making a third object with *different* data returns false as advertised. Lets look at the object_ids for each of these objects:


```ruby
irb(main):011:0> bob.object_id
=> 70351123976940
irb(main):009:0> bob2.object_id
=> 70351128238140
irb(main):010:0> bob3.object_id
=> 70351124267600
```

I was curious if the items containing the same values had the same id (probably not..) and it looks like it is not the same object. It looks like the `eql?` method does compare the values in the attributes of the objects. Cool!

Let's see what else `eql?` works on ... How about strings:

```ruby
irb(main):012:0> s = "what up internet"
=> "what up internet"

irb(main):013:0> s.eql?("blah")
=> false

irb(main):014:0> s.eql?("what up internet")
=> true

irb(main):015:0> s == "what up internet"
=> true
```

Looks like you can use eql? in place of == if you are doing string equality. Not sure I would do it.. it does look kinda nice but also kind of like java :) Probably I would just use ==. But now I know that if I see it used that way it works as it appears.

Lets try arrays

```ruby
irb(main):018:0> colors = %w(red green blue)
=> ["red", "green", "blue"]

irb(main):019:0> colors.eql?(["red", "green", "blue"])
=> true

irb(main):020:0> colors.eql?(["red"])
=> false

irb(main):022:0> colors.eql?(["blue", "red", "green"])
=> false
```

It works, if the array is an *exact* match, values and order. That leaves numbers which is pretty obvious, only exact matches since numbers are objects too!

```ruby
"1".eql?(1)
```

Compare to javascript

```javascript
> 1 == "1"
true
```

:) It's kinda funny.


## fetch

    fetch(index) → obj
    fetch(index, default) → obj
    fetch(index) { |index| block } → obj

I've used this method with hashes..ALOT. But looks like it works as advertised. The only reason I can think of needing to use a block to get the default is if maybe I needed to retrieve it from a file or webservice.

```ruby
irb(main):001:0> colors = %w(red white blue)
=> ["red", "white", "blue"]

irb(main):002:0> colors.fetch(0)
=> "red"

irb(main):003:0> colors.fetch(4, :IDUNNO)
=> :IDUNNO
irb(main):004:0>
```

Interesting though, if you use a negative number it counts from the end.

```ruby
irb(main):004:0> colors.fetch(-1)
=> "blue"

irb(main):005:0> colors.fetch(-2)
=> "white"
```

Another way I have set a default with getting values is to use the || (pipe pipe) operator, let me try it here:

```ruby
irb(main):006:0> colors.fetch(4) || :IDUNNO
IndexError: index 4 outside of array bounds: -3...3
from (irb):6:in `fetch'
from (irb):6
from /Users/nolastowe/.rbenv/versions/2.2.0-preview1/bin/irb:11:in `<main>'

irb(main):007:0> colors[4] || :IDUNNO
=> :IDUNNO
```

Yikes, so if you want to use fetch and set a default you better pass it as the second parameter :) Alternatively, you could check to see if a value exists and return or default with ||. Probably that is what I would use for an array, but now that I think of it, fetch does make it read better and the default is handy. I should change my thinking on this!

## fill

    fill(obj) → ary
    fill(obj, start [, length]) → ary
    fill(obj, range ) → ary
    fill { |index| block } → ary
    fill(start [, length] ) { |index| block } → ary
    fill(range) { |index| block } → ary

This method will set the values in an array to a value. It can take a value to set all to or it can take both a value and index(start end) or range. It can set to a value or via a block. Here's my experimentation.

```ruby
irb(main):009:0> weekly_prices = Array.new(7, 49.99)
=> [49.99, 49.99, 49.99, 49.99, 49.99, 49.99, 49.99]

irb(main):012:0> weekly_prices.fill(29.99, 1..6)
=> [49.99, 29.99, 29.99, 29.99, 29.99, 29.99, 29.99]

irb(main):013:0> weekly_prices = Array.new(7, 49.99)
=> [49.99, 49.99, 49.99, 49.99, 49.99, 49.99, 49.99]

irb(main):014:0> weekly_prices.fill(29.99, 1..5)
=> [49.99, 29.99, 29.99, 29.99, 29.99, 29.99, 49.99]

irb(main):015:0> weekly_prices.fill(29.99, 1, 5)
=> [49.99, 29.99, 29.99, 29.99, 29.99, 29.99, 49.99]

irb(main):016:0> weekly_prices.fill(1..5) { 9.99 }
=> [49.99, 9.99, 9.99, 9.99, 9.99, 9.99, 49.99]

irb(main):017:0> weekly_prices.fill(4.99)
=> [4.99, 4.99, 4.99, 4.99, 4.99, 4.99, 4.99]
```

Probably I would not go looking for such a method on arrays, but I think its a good one to keep in mind. Might be handy to reset an array to default values like for score keeping.

## find_index

    find_index(obj) → int or nil
    find_index { |item| block } → int or nil
    find_index → Enumerator

Hmm say you had an array of your books arranged in order of most favorite to favorite..

```ruby
irb(main):032:0* favorite_books = ["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"]
=> ["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"]

irb(main):033:0> favorite_books.find_index("Design Patterns in Ruby")
=> 1
```

You could say "Design Patterns in Ruby" was your second favorite with an index of 1 :)


## first

    first → obj or nil
    first(n) → new_ary

This one is easy, returns the first item in the list or nil if the array is empty.

```ruby
irb(main):032:0* favorite_books = ["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"]
=> ["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"]
irb(main):034:0> favorite_books.first
=> "Eloquent Ruby"
```

And by the way, "Eloquent Ruby by Russ Olsen" is the bestest book ever about ruby and you should run now and get it.

## flatten

    flatten → new_ary
    flatten(level) → new_ary

I've used this one before... let's try it out:

```ruby
irb(main):035:0> ruby_books = ["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"]
=> ["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"]

irb(main):036:0> clojure_books = ["Clojure for the Brave and True", "Clojure Cookbook", "Clojure Applied", "Living Clojure"]
=> ["Clojure for the Brave and True", "Clojure Cookbook", "Clojure Applied", "Living Clojure"]
```

There are my two arrays containing the names of my favorite books

```ruby
irb(main):037:0> favorite_books = [ruby_books, clojure_books]
=> [["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"], ["Clojure for the Brave and True", "Clojure Cookbook", "Clojure Applied", "Living Clojure"]]
```

Combining both lists into one array results in an array containing two arrays..

```ruby
irb(main):038:0> favorite_books.flatten
=> ["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice", "Clojure for the Brave and True", "Clojure Cookbook", "Clojure Applied", "Living Clojure"]

irb(main):039:0> favorite_books
=> [["Eloquent Ruby", "Design Patterns in Ruby", "Growing Rails Applications in Practice"], ["Clojure for the Brave and True", "Clojure Cookbook", "Clojure Applied", "Living Clojure"]]
```

You can see that flatten combines the two arrays and squashes into one array but doesn't change the actual array. The `favorite_books` array is still a combination.

However... I did not know about the parameter of `level` ...

```ruby
irb(main):040:0> one = [1, 2, 3, ['a', 'b']]
=> [1, 2, 3, ["a", "b"]]

irb(main):041:0> two = [5, 6, 7, ['c','d']]
=> [5, 6, 7, ["c", "d"]]

irb(main):042:0> both = [one, two]
=> [[1, 2, 3, ["a", "b"]], [5, 6, 7, ["c", "d"]]]

irb(main):043:0> both.flatten(1)
=> [1, 2, 3, ["a", "b"], 5, 6, 7, ["c", "d"]]
```

Level allows you to indicate how deep to flatten it, that is cool. I did not know that before now!

## flatten!

    flatten! → ary
    flatten!(level) → ary or nil

Now this, as with all ! (bang) methods, it changes the object instead of only returning the results.

Well, I did learn a few things writing this up and I hope you did too! It really is amazing to find out what you did not know :) Hopefully these tidbits will come in handy in future array manipulations :) :)

Next post on arrays will continue with our journey though arrays and start with Frozen.

