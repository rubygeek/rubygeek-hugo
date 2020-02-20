---
categories:
- clojure
- practice
comments: true
date: "2015-04-18T15:13:10Z"
title: Using lein-try to learn Prismatic Schema
---

When I first heard of `lein-try` on the [Cognitect Podcast](http://blog.cognitect.com/cognicast/050-luke-vanderhart-ryan-neufeld) (and then again in the [Clojure Cookbook](http://shop.oreilly.com/product/0636920029786.do) I thought wow! How cool is that! I tried it with following the example in the [readme](https://github.com/rkneufeld/lein-try) `clj-time` to get familar with using it.

First add lein-try to your ~/.lein/profiles plugins vector, run `lein deps` to get it installed.

```clj
{:user {:plugins [lein-try "0.4.3"]}}
```


I wanted to understand [Prismatic’s Schema](https://github.com/Prismatic/schema) and wanted to try it.  I wanted to start simple so I could understand it.

```clj
lein try “prismatic/schema”
```

A repl starts, then require the library and give it an alias

```clj
(require '[schema.core :as s])
```

Ok now ready to play!

First we need to define a schema:
```clj
user=> (def Recipe "a recipe" {:name s/Str})
#'user/Recipe
```

Then we can use validate on a data structure to make sure it matches
```clj
user=> (s/validate Recipe {:name "Chicken and Rice"})
{:name "Chicken and Rice"}
```

On success we get back the same structure. However, if we pass an invalid structure
```clj
user=> (s/validate Recipe {:name 42})
```

Wait a minute. What is this? an actually helpful error message in Clojure?!?!
```clj
ExceptionInfo Value does not match schema: {:name (not (instance? java.lang.String 42))} schema.core/validate (core.clj:161)
```

It tells you the keyword of the invalid value and what value was passed in. Nice.

We can validate, but what does check do?

```clj
user=> (s/check Recipe {:name "Chicken and Rice"})
nil
user=> (s/check Recipe {:name 42})
{:name (not (instance? java.lang.String 42))}
```

Looks like you’d use check when you are prepared to deal with nil, and validate when a representation of the data structure is needed.

Now lets try a more complex example.
```clj
user=> (def Source {:name s/Str :url s/Str :year s/Int})
#'user/Source
user=> (s/validate Source {:name "Food Network" :url "http://www.foodnetwork.com" :year 2015})
{:name "Food Network", :year 2015, :url "http://www.foodnetwork.com"}
```
This example is Source which consists of 3 elements. Lets try leaving one off:

```clj
user=> (s/validate Source {:name "Food Network" :url "http://www.foodnetwork.com" })

ExceptionInfo Value does not match schema: {:year missing-required-key} schema.core/validate (core.clj:161)
```

This makes me think maybe there is a way to indicate an optional value? Reading down farther on the [documentation](https://github.com/Prismatic/schema) I see how to make keys optional. Lets redefine our Source with year as optional:

```clj
user=> (def Source {:name s/Str :url s/Str (s/optional-key :year) s/Int})
#'user/Source

user=> (s/validate Source {:name "Food Network" :url "http://www.foodnetwork.com" })
{:name "Food Network", :url "http://www.foodnetwork.com"}
```

Sweet! I also read that since Schemas are just data structures they are composable. Lets add Source to Recipe to make a more complex structure.

```clj
user=> (def Recipe "a recipe with source" {:name s/Str :source Source})
#'user/Recipe
user=> Recipe
{:name java.lang.String, :source {:name java.lang.String, :url java.lang.String, #schema.core.OptionalKey{:k :year} Int}}

user=> (s/validate Recipe {:name "Chicken and Rice" :source {:name "FoodNetwork" :url "www.foodnetwork.com"}})
{:name "Chicken and Rice", :source {:name "FoodNetwork", :url "www.foodnetwork.com"}}
```

Nice!! Since I’m a ruby developer by day, I think how could I do this in ruby? Well, in Rails, we have validations on models and once you set the attributes you *could* call model.valid? which would return boolean. If it’s not valid, it populates a errors attribute on the model with a nested hash of key/error messages. Composing two together is not as straightforward. You might be able to with the `accepts_nested_attributes_for` on a Recipe model, but I conclude its not going to be as elegant as using Clojure and Schema :)

Using `lein-try` made it easy to experiement with a library and poke around to practice :)
