---
categories:
- elixir
- clojure
comments: true
date: "2017-12-21T00:00:00Z"
title: 'Data Structures in Clojure and Elixir: Maps'
---

### Elixir

Tuples used `{ }` .. but maps use `%{ }`. You can use anything for a key..  here are string keys:

```elixir
iex(1)> data = %{"name" => "nola", "color" => "red"}
%{"color" => "red", "name" => "nola"}

iex(4)> data["name"]
"nola"
```

If you use a string to set the value, obviously you need to use that to get the value.

Using atoms for keys is alot nicer for maps, the syntax is shorter and you can use  `.name` to access the value

```elixir
iex(2)> more_data = %{city: "Austin", state: "TX"}
%{city: "Austin", state: "TX"}

iex(5)> more_data.city
"Austin"
```

I don't know why you would, but you can use integers as keys too: 

```elixir
iex(3)> yet_more = %{1 => "user 1", 2 => "user 2"}
%{1 => "user 1", 2 => "user 2"}

iex(12)> yet_more[1]
"user 1"
```

And, even a list as a key:  

```elixir
iex(8)> weird_keys = %{ [1, 2] => "one two", [3, 4] => "three four"}
%{[1, 2] => "one two", [3, 4] => "three four"}

iex(9)> weird_keys[[1,2]]
"one two"
```

I can't think of a use case for that, but if you need it, then you can do it.

Some methods for maps: 

```elixir
iex(6)> Map.keys(more_data)
[:city, :state]

iex(7)> Map.keys(data)
["color", "name"]

iex(13)> Map.keys(weird_keys)
[[1, 2], [3, 4]]

iex(14)> Map.values(weird_keys)
["one two", "three four"]

iex(25)> Enum.count(weird_keys)
2
```

#### Updating a value in a map

```elixir
iex(15)> data = %{color: "red", name: "nola"}

(16)> updated = %{data | color: "pink" }
%{color: "pink", name: "nola"}
```

Using the | operator with either syntax for strings or atom keys (whichever you are using) ie  `"color" => "pink"'. 

But if the key does not exist, you will get an exception!


To add a new key to a map, you use `Map.put_new`: 

```elixir
iex(1)> data = %{color: "red", name: "nola"}

iex(2)> Map.put_new(data, :state, "TX")
%{color: "red", name: "nola", state: "TX"}
```
If the key was already there, the change is ignored.

To add a new key to a map reguardless if it was there before or not use: 

```elixir
iex(10)> data = %{color: "red", name: "nola"}
%{color: "red", name: "nola"}

iex(11)> more = Map.put(data, :color, "green")
%{color: "green", name: "nola"}

iex(12)> Map.put(more, :state, "TX") 
%{color: "green", name: "nola", state: "TX"}
```


Still, the data is not actually changed but a copy is returned. The original is unchanged.


### Clojure

```clojure
user=> (hash-map :name "nola" :state "TX" :color "red")
{:color "red", :name "nola", :state "TX"}

user=> (array-map :name "nola" :state "TX" :color "red")
{:name "nola", :state "TX", :color "red"}
```

I was trying to figure out what the difference was and found this on [stackoverflow](https://stackoverflow.com/questions/26050540/what-is-the-difference-between-the-hash-map-and-array-map-in-clojure)

<blockquote>
    Array maps and hash maps have the same interface, but array maps have O(N) lookup complexity (i.e. it is implemented as an simple array of entries), while hash maps have O(1) lookup complexity.
</blockquote>

If you notice in my code above, the hash-map has the keys in a different order than I created them in.

So, thats the difference. 

Using the literal `{ }` it produces an array-map. 


```clojure
user=> (class {:name "Nola" :state "tx"})
clojure.lang.PersistentArrayMap
```

Some functions for maps: 

```clojure
user=> (def data {:color "red", :name "nola", :state "TX"})
#'user/data

user=> (keys data)
(:color :name :state)

user=> (vals data)
("red" "nola" "TX")

user=> (count data)
3
```

Editing or adding new keys to a map:

```clojure
user=> (assoc data :state "IL")
{:color "red", :name "nola", :state "IL"}

user=> (assoc data :food "tacos")
{:color "red", :name "nola", :state "TX", :food "tacos"}
```

Unlike Elixir, Clojure doesn't care if this value is new or not when updating a map. It also doesn't change the original data structure like Elixir.

Let me know if I am understanding these languages right. Next post will talk about [Elixir Structs and Clojure Records](/2017/12/26/data-structures-in-clojure-and-elixir-structs-records/).

See my post on [Lists, Tuples, Vectors](/2017/12/13/data-structures-in-clojure-and-elixir-lists-tuples-vectors/) and [Sets](/2017/12/17/data-structures-in-clojure-and-elixir-sets/).