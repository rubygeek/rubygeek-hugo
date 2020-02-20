---
categories:
- ruby
comments: true
date: "2015-08-02T00:00:00Z"
title: 'Enumerable: any?'
---

This is about the handy `any?` method in [Enumerable](http://ruby-doc.org/core-2.2.2/Enumerable.html)

The documentation says

<blockquote>
Passes each element of the collection to the given block. The method returns true if the block ever returns a value other than false or nil. If the block is not given, Ruby adds an implicit block of { |obj| obj } that will cause any? to return true if at least one of the collection members is not false or nil.
</blockquote>


```ruby
class Taco
  attr_accessor :meat

  def initialize(meat)
    @meat = meat
  end

  def beef?
    @meat == :beef
  end

  def chicken?
    @meat == :chicken
  end

end
```

Lets test it out
```ruby

irb(main):018:0> taco = Taco.new(:beef)
=> #<Taco:0x007fa374070278 @meat=:beef>

irb(main)> taco.beef?
=> true

irb(main)> taco.chicken?
=> false
```

Lets create two instances
```ruby
tacos = []
tacos.push(Taco.new(:beef))
tacos.push(Taco.new(:chicken))
```

Now since the array has enumerable and we made some handy methods to test the meat of our tacos we can easily check our array to see if we have any chicken tacos:

```ruby
tacos.any? { |taco| taco.chicken? }
```

If all the elements of the array are booleans as in this case we don't need the block version:

```ruby
irb(main)> possible_chicken_tacos = [tacos[0].chicken?, tacos[1].chicken?]
=> [true, false]

irb(main)> possible_chicken_tacos.any?
=> true
```

Yep, today was definately a chicken taco day. :)
