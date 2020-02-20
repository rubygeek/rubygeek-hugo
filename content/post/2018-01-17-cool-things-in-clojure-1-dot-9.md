---
categories: clojure
comments: true
date: "2018-01-17T00:00:00Z"
title: Cool Things In Clojure 1.9
---

I was glad to see Clojure 1.9 out because of [Spec](https://clojure.org/guides/spec) and some more things. But, was sad, because I can no longer say "Hey! I have a [commit](https://dev.clojure.org/jira/browse/CLJ-1449) in the latest version of clojure!". Course, that just gives me some motivation to find ways to help out in .. Clojure 2.0 ?? OMG. I don't like to brag, but it was fun when this guy in my community (mostly he didn't do Clojure) when he heard I had a patch in 1.8, come up to me and say "BADASS!!! WAY TO GO!!!". I did have the help of [Alex Miller](https://twitter.com/puredanger) and others and it was partly done when I took it over. It was really fun to be a small part of 1.8

Here are the things I think are cool (besides Spec) in Clojure 1.9:

## Reader Syntax for Namespaced Maps

```clojure
user=> (def data #:person{:name "Nola", :color "purple", :state "TX"})
#'user/data

user=> (keys data)
(:person/name :person/color :person/state)
```

Using #:ns-name{ key value key value } gives you an easy way to write out your map without prefixing each key. COOL.

## Predicate Functions 

This were put in mainly (I think) to support Spec, but in short, predicate functions (ending in ?) return true or false. 

Some of them [added in this patch](https://github.com/clojure/clojure/blob/master/changes.md#13-new-predicates) are: 

  * boolean?
  * int? pos-int? nat-int?
  * double? 
  * ident?  
  * qualified-keyword?

For example, Using [remove](https://clojuredocs.org/clojure.core/remove) with a collection will remove all values for which the predicate is true. So to remove all the postive integers from a vector: 

```clojure
user=> (remove pos-int? [-4 -2 4 1 0 -2 5])
(-4 -2 0 -2)
```

Then of course, in a spec: 

```clojure
user=> (s/def ::age pos-int?)
:user/age

user=> (s/conform ::age -16)
:clojure.spec.alpha/invalid

user=> (s/conform ::age 13)
13
```

Then lets remove everything not a qualified keyword, using in an anonymous function:: 

```clojure
user=> (remove #(not (qualified-keyword? %))  [:name/nola :color/purple 1 5 0])
(:name/nola :color/purple)
```

Previous you could do the samething of course, but you'd have to write more code. I don't know if you would use them much outside of spec, but if you do there they are there. Nice.

##  Atoms get new functions! 

Ever wanted to give an atom a new value but also get back what the value *was* too? 

```clojure
user=> (def name (atom "Nola"))

user=> (reset-vals! name "Nick")
["Nola" "Nick"]
```

Instead of just [reset!](https://clojuredocs.org/clojure.core/reset!) you now have reset-vals! returning the old value followed by the new (current) value. And this funtion's cousin swap-vals!:

```clojure
user=> (def size (atom 10))
#'user/size

user=> (swap-vals! size inc)
[10 11]
```

This function, `swap-vals!` takes an atom and a function and returns the new and the old value. 

And before you [bikeshed](https://en.wiktionary.org/wiki/bikeshedding) on the names of these functions, don't bother, it has [been done](https://dev.clojure.org/jira/browse/CLJ-1454). I think they are good names :) 

## Brew Install

Not exactly a part of the [Clojure 1.9 Release Notes](https://clojure.org/news/2017/12/08/clojure19) but I think it bears mention along with this release is that for you Mac Users (like me!), you can now `brew install clojure`. I wrote [about it](http://www.rubygeek.com/2017/12/27/getting-started-with-clojure-is-now-easier-than-ever-mac/) early because I was so excited. Read the post for more details which also links to more documentation on it.

And also if you are an new Clojure dev, with Atom you can setup a decent [Clojure environment with Atom](https://medium.com/@jacekschae/slick-clojure-editor-setup-with-atom-a3c1b528b722?__s=gbwqypueretgonjwasja). I recommend looking over the docs for [parinfer](https://shaunlebron.github.io/parinfer/) to learn its magics, but once you get it you are good for life.

I hope you are excited about Clojure 1.9 and are using it in your projets :) It was a long time coming, but the best things are worth waiting for, right? :)



 

 

