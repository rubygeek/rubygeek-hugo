---
categories:
- ruby
comments: true
date: "2013-09-02T00:00:00Z"
title: Exploring Arrays - Part 4
---

## combination
    combination(n) { |c| block } → ary 
    combination(n) → an_enumerator

Next method up for discussion is combination, when I saw this my first thought was "OH wow!! I can make games!!". Being able to generate datasets I think is key to making a game. Being able to make a list of combinations, then pick on at random I think is cool. Lets try it 

``` ruby
>> bad_guys = [:ogre, :giant, :zombie]
=> [:ogre, :giant, :zombie]
>> bad_guys.combination(2).to_a
=> [[:ogre, :giant], [:ogre, :zombie], [:giant, :zombie]]
>> bad_guys.combination(2).to_a.sample(1).flatten
=> [:giant, :zombie]
```

Lets build an array of bad guys, generate all the combinations of 2 badguys, choose a set of 2 at random and flatten it. Here is the list of bad guys for our awesome game. Cool huh? Doesn't this just make you want to stop everything and make a game now? Ok, maybe it is just me. :) 

## compact
    compact → new_ary 
    Returns a copy of self with all nil elements removed.


I recently used this on a project (with help from a friend, Thanks Corey!) where I had 1 or 2 items and if I had two I wanted to be $100-200 but if just one I wanted it to be $100 or $200.
``` ruby
>> price_a = "$100"
=> "$100"
>> price_b = "$200"
=> "$200"
>> [price_a, price_b].compact.join("-")
=> "$100-$200"

>> price_a = "$100"
=> "$100"
>> price_b = nil
=> nil
>> [price_a, price_b].compact.join("-")
=> "$100"
```
Cool huh? previously I had something hideous like

``` ruby
if price_a.present? && price_b.present?
 "#{price_a}-#{price_b}"
elsif price_a.present?
  price_a
elsif price_b.present?
  price_b
else
  ""
end
```

Gross right? uh. I hate hate if structures like that just for checking if something has value. Following Avdi Grimm and reading his book "Much Ado about Naught" and his Confident Ruby has given me ideas to get around this type of coding! Using compact is great for this since it throws away any nils. Note this returns a copy of the array not changing it (as it would if it was compact!).

## compact!
    compact! → ary or nil
    Removes nil elements from the array. Returns nil if no changes were made, otherwise returns ary.

Here's what I just said, the ! changes the array and if nothing changed it just returns nil. 

## concat
    concat(other_ary) → ary 
    Appends the elements of other_ary to self.

Concatenates one array into the other. Also could use + ... let me try it.

``` ruby
>> colors = [:red, :green, :blue]
=> [:red, :green, :blue]
>> colors.concat([:yellow, :orange])
=> [:red, :green, :blue, :yellow, :orange]
>> colors
=> [:red, :green, :blue, :yellow, :orange]

>> colors = [:red, :green, :blue] + [:yellow, :orange]
>> colors
=> [:red, :green, :blue, :yellow, :orange]
```
	
So, looks like they both will add arrays and the arrays are identical. Depending on the context, I think i might like the concat better.

## count
    count → int 
    count(obj) → int
    count { |item| block } → int

I think I knew count could count particular items but I forgot. Trying it out:

``` ruby
>> items = [:potion, :food, :food, :weapon, :potion]
=> [:potion, :food, :food, :weapon, :potion]
>> items.count(:potion)
=> 2
>> items.count(:food)
=> 2
```

So imagine if you were writing a game, and showing stats for things in your bag you could do something like this. The last one takes a block.

## cycle 
    cycle(n=nil) {|obj| block } → nil 
    cycle(n=nil) → an_enumerator

Cycle will iterate through an array going through each item n times or forever

``` ruby
>> levels = [:one, :two, :three, :four, :five]
>> levels.cycle(2) { |level| puts "level #{level}" }
level one
level two
level three
level four
level five
level one
level two
level three
level four
level five
```

This cycles through the array 2 times. Hmm that makes me wonder if I can use this as sort of a state machine? 

``` ruby
>> status = [:draft, :review, :publish]
=> [:draft, :review, :publish]
>> status.cycle(1)
=> #<Enumerator: [:draft, :review, :publish]:cycle(1)>
>> states = status.cycle(1)
=> #<Enumerator: [:draft, :review, :publish]:cycle(1)>
>> states.next
=> :draft
>> states.next
=> :review
>> states.next
=> :publish
>> states.next
StopIteration: iteration reached an end
	       from (irb):62:in `next'
	       from (irb):62
	       from /Users/nolastowe/.rvm/rubies/ruby-1.9.3-p392/bin/irb:16:in `<main>'
```

Hmm interesting. Well maybe needs some work. :) Next is a method for dealing with loops... not sure if its great to be using with cycle, its just something that came to my head when I was playing around.

This is enough for now, next blog post will start with delete.




