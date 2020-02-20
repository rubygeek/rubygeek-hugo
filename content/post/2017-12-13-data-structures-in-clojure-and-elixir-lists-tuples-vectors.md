---
categories:
- elixir
- clojure
comments: true
date: "2017-12-13T00:00:00Z"
title: 'Data Structures in Clojure and Elixir: Lists, Tuples, Vectors'
---

In my free time I've been learning more [Elixir](https://elixir-lang.org/), and I've already been doing [Clojure](https://clojure.org/) for awhile now. I am going to write this blog post to compare/contrast the data structures available in both languages as well as a few functions you can use. Just for fun and so I can keep it in my head.

## List / Vector
This is aslso known as an array in some languages. This is a collection of items, in which case order is generally depended on to access items. When I am explaining this to new developers I describe it as a bookshelf, each book on the shelf is an element. 

### Elixir
We have two kinds of these in Elixir, a Tuple and a List. A Tuple is fast and intended to be for short (<= 4 items) and a List is stored as Linked List. It is much slower than a Tuple because it is stored as a Linked List (if you are not familiar, under the hood, each item in the list has a pointer to the next item).

Lists are defined using `[]` Here's what it looks like:

```elixir
iex(1)> favorites = ["bbq", "tacos", "pizza"]
["bbq", "tacos", "pizza"]
```

Then some methods to access items in a list:

```elixir
iex(2)> Enum.at(favorites, 0)
"bbq"

iex(4)> Enum.at(favorites, 2)
"pizza"

iex(5)> List.first(favorites)
"bbq"

iex(6)> List.last(favorites)
"pizza"

iex(7)> length(favorites)
3
```

Notice that length is not part of any namespace, it's part of Kernel which doesn't need prefix of module name (as you might expect) ... it is the same place that `def`, `defmodule`, `if` and `hd` are defined.  

A tuple is define with `{ }`:

```elixir
iex(7)> not_favorites = {"shrimp", "squid", "octopus"}
{"shrimp", "squid", "octopus"}
```

Then accessing elements is like this:

```elixir
iex(7)> not_favorites = {"shrimp", "squid", "octopus"}
{"shrimp", "squid", "octopus"}

iex(8)> elem(not_favorites, 0)  # index of first item is 0
"shrimp"

iex(9)> elem(not_favorites, 2)  # index of third item is 2
"octopus"

iex(10)> tuple_size(not_favorites)
3
```

Tuples are often used as return values from functions and for pattern matching in case statements. I dont often see the elements of a tuple accessed with elem, most of the time I've seen it decontructed with pattern matching. But if you need it, you that is how you do it. 

### Clojure
Clojure has two kinds of sequential containers ... Vectors and Lists. A vector is more efficient adding to the end, where as a list (think single linked lists) is more efficient adding to the front.

```clojure
user=> (def favorites ["pizza" "tacos" "cake"])
#'user/favorites

user=> (def not-favorites '("squid" "shrimp" "octopus"))
#'user/not-favorites

user=> (first favorites)
"pizza"

user=> (rest not-favorites)
("shrimp" "octopus")
```

The ' in front of a list tells clojure don't evaluate this right now.. which you'll see why next.

Take this code:
```clojure
(def name "nola")
```

It defines a value of "nola" which you can refer to as "name". It's not exactly like a variable that you might thing of from other languages, but for now lets say it is close.

However notice something about a list and code:

```clojure
user=> (count '(1 2 3))
3

user=> (count '(def name "nola"))
3

user=> (first '(def name "nola"))
def

user=> (rest '(def name "nola"))
(name "nola")
```

This code is defining a value called `name` to be `nola` ... is itself a list. So you may hear Clojure people chant "code is data. data is code." and this is what they are talking about. It took me awhile to get it then it seemed so simple.

Next post I will talk about [Sets](/2017/12/17/data-structures-in-clojure-and-elixir-sets/) in Elixir and Clojure. 

