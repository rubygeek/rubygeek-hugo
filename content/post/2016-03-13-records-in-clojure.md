---
categories:
- clojure
comments: true
date: "2016-03-13T08:22:44Z"
title: Records in Clojure
---

I haven't used records before in a project and when reading some code that used it, realize I really need to learn about records. It's not complicated and actually kind of cool! Here's my experimentation.

To start off, I re-read the first chapter in [ClojureApplied](http://blog.clojuregeek.com/2015/08/30/book-review-clojure-applied/) where it talks about Modeling your Entities.

I took a familar problem, my [recipe-api](https://github.com/rubygeek/recipe-api). This is a recipe api that uses liberator api reading recipes from a database. But, I can use the concept here. 

```clojure
user=> (defrecord Recipe [name link source])
user.Recipe
```

When you create a record, you get two factory functions for free to create instances of it. The first, a positional function:

```clojure
user=> (def tacos (->Recipe "Tacos" "www.tacorecipes.com" "mom"))
#'user/tacos
```


You must put values for all the fields in or you get
```clojure
user=> (def tacos (->Recipe "Tacos" "www.tacorecipes.com"))

CompilerException clojure.lang.ArityException: Wrong number of args (2) passed to: user/eval10000/->Recipe--10017, compiling:(form-init4856550892047924668.clj:1:12)
```

The other way you can create an instance of a record is using a map.

```clojure
user=> (def pizza (map->Recipe {:name "Pizza" :source "dad"}))
#'user/pizza

user=> pizza
#user.Recipe{:name "Pizza", :link nil, :source "dad"}
```

We didn't have a value for `link` in the map so it still let us create the instance and automatically assigned the value of nil. This might be a reason to use the `map->` version over the positional version, if you don't know all the values at that time. They work like maps, you can add in missing values later with `assoc`:

```clojure
user=> (assoc pizza :link "www.pizzarecipes.com")
#user.Recipe{:name "Pizza", :link "www.pizzarecipes.com", :source "dad"}
```

Accessing the the values in a record is just like a map:

```clojure
user=> (:name tacos)
"Tacos"
user=> (:source tacos)
"mom"
```

The chapter in [Clojure Applied](http://amzn.to/2Djnp7L) has a great section on why you would want to choose Records over Maps you should read! I won't spoil the fun here, but there are some compelling reasons when to use records over a map.
