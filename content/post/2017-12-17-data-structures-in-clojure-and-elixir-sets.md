---
categories:
- elixir
- clojure
comments: true
date: "2017-12-17T00:00:00Z"
title: 'Data Structures in Clojure and Elixir:  Sets'
---

My [last post](/2017/12/13/data-structures-in-clojure-and-elixir-lists-tuples-vectors/) talked about Lists, Tuples and Vectors comparing [Elixir](https://elixir-lang.org/) and [Clojure](https://clojure.org/) ... now lets cover a related topic.. sets.

When you think of set, think of Math Sets.

### Elixir

There are two ways to create a Set in Elixir

Using the Pipe Operator and `put`:

```elixir
set = MapSet.new |> MapSet.put("apple") |> MapSet.put("apple") |> MapSet.put("banana")
#MapSet<["apple", "banana"]>
```

Or use a List and pass that to `MapSet.new`

```elixir
iex(1)> set = MapSet.new(["apple","orange","grape", "grape"])
#MapSet<["apple", "grape", "orange"]>
```

I purposely put duplicates when I created it to see what would happen. It quietly dismissed the duplicate value and made a collection of unique values.

Passing a list to new would be a good way to filter out duplicates in a collection as well as build a set more easily.

```elixir
iex(3)> unique_values = MapSet.new(["apple","orange","grape", "grape"]) |> MapSet.to_list()
["apple", "grape", "orange"]
```

Some more functions for sets

```elixir
iex(4)> fruit = MapSet.new(["apple","orange","grape"])
#MapSet<["apple", "grape", "orange"]>

iex(5)> MapSet.member?(fruit, "apple")
true
```

and subset

```elixir
iex(4)> fruit = MapSet.new(["apple","orange","grape"])
#MapSet<["apple", "grape", "orange"]>

iex(6)> other_fruit = MapSet.new(["apple", "grape"])
#MapSet<["apple", "grape"]>

iex(8)> MapSet.subset?(other_fruit, fruit)
true
```

Read more on [MapSet](https://hexdocs.pm/elixir/MapSet.html)

### Clojure

You have two ways to make a set in Clojure, using the `set` function and the `#{}` literal. 

```clojure
user=> (set [:apple :grape :orange :orange])
#{:orange :apple :grape}

user=> (set '(:apple :grape :orange :orange)
#{:orange :apple :grape}

```

We are converting a vector or a list to a set.

Also testing putting duplicates and as expected, the result is all unique values.

```clojure
user=> (def taco-restaurants #{"torchys" "maudies" "torchys" "taco bell"})

IllegalArgumentException Duplicate key: torchys  clojure.lang.PersistentHashSet.createWithCheck (PersistentHashSet.java:68)
RuntimeException Unmatched delimiter: )  clojure.lang.Util.runtimeException (Util.java:221)
```
No, the error is not calling `taco bell` a restaurant, it is putting `torchys` twice. YIKES... the literal syntax does not like duplicates. If you think your values might have duplicates, put in vector first and pass to `set`.

<blockquote>
    Edit: Alex Miller pointed out, the reason we have an error it because it is an invalid set.
    Now I think it should error and since Elixir doesn't have a literal syntax it is not exactly the same thing to compare. Thanks for pointing that out why it errors Alex :)
</blockquote>

Some useful functions for sets:

```clojure
user=> (def valid-actions #{:get :post})
#'user/valid-actions

user=> (contains? valid-actions :post)
true

user=> (contains? valid-actions :head)
false
```

Use a set when you need to see if a certain value is one of X. It is surprisingly not easy to do the same with checking membership in a list or vector :) Use a set, you don't need duplicates anyways so it makes sense.

There is a Set namespace with functions for sets:

```clojure
user=> (require '[clojure.set :as set])
nil

user=> (set/subset? #{:post} valid-actions)
true

user=> (set/subset? #{:head} valid-actions)
false
```


Next, we'll cover [Maps!](/2017/12/21/data-structures-in-clojure-and-elixir-maps/)
