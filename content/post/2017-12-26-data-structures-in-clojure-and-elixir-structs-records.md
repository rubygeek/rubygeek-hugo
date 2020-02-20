---
categories:
- elixir
- clojure
comments: true
date: "2017-12-26T00:00:00Z"
title: 'Data Structures in Clojure And Elixir: Structs and Records'
---
Both Elixir and Clojure have a way to make a more "organized" entity, like a map but more structure. Elixir can easily have default values whereas with Clojure, it is not built in but you can create a helper functions to set some default values and create a record.

### Elixir

A `defstruct` takes on the name of the `defmodule` it is in:

```elixir
iex(1)> defmodule User do
...(1)>   defstruct name: "", state: "", color: "green", size: "M"
...(1)> end
{:module, User,
 <<70, 79, 82, 49, 0, 0, 8, 60, 66, 69, 65, 77, 65, 116, 85, 56, 0, 0, 0, 232,
   0, 0, 0, 22, 11, 69, 108, 105, 120, 105, 114, 46, 85, 115, 101, 114, 8, 95,
   95, 105, 110, 102, 111, 95, 95, 9, 102, ...>>,
 %User{color: "green", name: "", size: "M", state: ""}}

iex(2)> %User{name: "nola", state: "TX"}
%User{color: "green", name: "nola", size: "M", state: "TX"}

iex(30)> user.name
"nola"

iex(30)> user.color
"red"

```

Using an attribute that doesn't exist: 

```elixir
iex(32)> user["country"]
** (UndefinedFunctionError) function User.fetch/2 is undefined (User does not implement the Access behaviour)
    User.fetch(%User{color: "", name: "nola", state: "TX"}, "country")
    (elixir) lib/access.ex:304: Access.get/3

```

Yeah don't do that :) Accessing a key not defined is not good. 


### Clojure

First define a record with `defrecord` and a vector for the names of the attributes:

```clojure
user=> (defrecord DataRecord [name state])
user.DataRecord
```

Then create a record

```clojure
user=> (def data (->DataRecord "Nola" "TX"))
#'user/data
```

Access the values just like a map:
 
```clojure
user=> (:name data)
"Nola"

user=> (:state data)
"TX" 
```

Or create using a map: 

```clojure
user=> (map->DataRecord {:name "bob" :state "IL"})
#user.DataRecord{:name "bob", :state "IL"}
```

To create a Record that may have default values, create a helper function to build it. 

```clojure
user=> (defrecord Person [name state color size])
user.Person

user=> (defn create-person [{:keys [name state color size] :or {color "green" size "M" }}] (->Person name state color size))
#'user/create-person

user=> (def data (create-person {:name "bob" :state "TX"}))
#user.Person{:name "bob", :state "TX", :color "green", :size "M"}

user=> (:name data)
"bob"

user=> (:state data)
"TX"

user=>
```

I wrote about Clojure records awhile back, See [more on records](/2016/03/13/records-in-clojure/).



See my previous posts on [Maps](/2017/12/21/data-structures-in-clojure-and-elixir-maps/), [Lists, Tuples, Vectors](/2017/12/13/data-structures-in-clojure-and-elixir-lists-tuples-vectors/) and [Sets](/2017/12/17/data-structures-in-clojure-and-elixir-sets/).

