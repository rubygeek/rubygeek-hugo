---
categories:
- clojure
comments: true
date: "2015-12-12T14:43:49Z"
title: Two Cool Tools for Clojure Development
---

I've recently used a couple tools that I want to write about because I think they are pretty useful.

## 1. Eastwood
[Eastwood](https://github.com/jonase/eastwood) is a linter, which will check your syntax to see if it is valid. BTW I have actually made this mistake for real! The github page lists all the things it checks for and a lot of how to configure eastwood.

To get started, Install into your lein profiles.clj:

```
{:user {:plugins [[jonase/eastwood "0.2.1"]]}}
```

My function, which by the way, is awesome:

```clj
(defn my-awesome-function [x y z]
"My function is awesome"
(+ x y z))
```

Running `lein eastwood` in the project shows me:

```
lein eastwood
== Eastwood 0.2.1 Clojure 1.7.0 JVM 1.8.0_60
Directories scanned for source files:
src test
== Linting blog-post.core ==
Entering directory `/Users/nolastowe/practicing/clojure/blog-post'
src/blog_post/core.clj:5:7: misplaced-docstrings: Possibly misplaced docstring, my-awesome-function
src/blog_post/core.clj:5:1: unused-ret-vals: Constant value is discarded: "My function is awesome"
== Linting blog-post.core-test ==
== Warnings: 2 (not including reflection warnings)  Exceptions thrown: 0
Subprocess failed
```

Doh. The docstring is in the wrong place, correcting that and eastwood is happy. That is an easy mistake to make!

```
lein eastwood
== Eastwood 0.2.1 Clojure 1.7.0 JVM 1.8.0_60
Directories scanned for source files:
src test
== Linting blog-post.core ==
== Linting blog-post.core-test ==
== Warnings: 0 (not including reflection warnings)  Exceptions thrown: 0
```

I've used eastwood a lot when I was developing amazon lambda functions, when I couldn't see the result till I've uploaded to amazon. At first was frustrated only after all that to find an error which required me to do another compile to jar and upload. Now I use it before commit or when i've completed a unit of work or when I just can't figure why something is not working..always a chance I did something stupid with syntax.

## 2. Criterium
This library is for benchmarking clojure and I was using it to compare different implementations when I was working on adding some string functions to Clojure. Initially I added it to my lein profiles (similar to how i did with eastwood) which works fine for testing in the repl. [Ghadi](https://twitter.com/smashthepast/) saw me talking about using it in Clojurians Slack and gave me a bash script to start any version of clojure and criterium with a repl:

```bash
#!/bin/bash
M2=$HOME/.m2/repository
CLASSPATH=${M2}/criterium/criterium/0.4.3/criterium-0.4.3.jar:src
JOPTS='-Xmx4G -XX:+UseG1GC'
CLOJURE_JAR=$M2/org/clojure/clojure/${CLOJURE_VERSION}/clojure-${CLOJURE_VERSION}.jar
java $JOPTS -cp ${CLASSPATH}:${CLOJURE_JAR} clojure.main
```

Then save it as `repl-version.sh` and run with `CLOJURE_VERSION=1.7.0 repl-version.sh` to open a repl, then you can test away. I see this being useful when you are working on clojure itself or want to compare things in different versions of clojure. I used it when I was working on a patch for Clojure.

One of the things I used it for was figuring  wanted to compare char? and instance? which should I use.. this is a true story! I was trying to figure this out one day..

```
user=> (quick-bench (char? \c))
Evaluation count : 39012534 in 6 samples of 6502089 calls.
Execution time mean : 5.188821 ns
Execution time std-deviation : 0.351751 ns
Execution time lower quantile : 4.755146 ns ( 2.5%)
Execution time upper quantile : 5.544008 ns (97.5%)
Overhead used : 10.020671 ns

user=> (quick-bench (instance? Character  \c))
Evaluation count : 51496848 in 6 samples of 8582808 calls.
Execution time mean : 2.510016 ns
Execution time std-deviation : 0.701679 ns
Execution time lower quantile : 1.693617 ns ( 2.5%)
Execution time upper quantile : 3.375066 ns (97.5%)
Overhead used : 9.812462 ns
nil
```

I wondered, how does `char?`  work? So I looked at the source:

```
user=> (source char?)
(def
^{:arglists '([x])
:doc "Return true if x is a Character"
:added "1.0"
:static true}
char? (fn ^:static char? [x] (instance? Character x)))
nil
```

Ahh, I see, it just checked the `instance?` .. so I look what that function does:

```
user=> (source instance?)
(def
^{:arglists '([^Class c x])
:doc "Evaluates x and tests if it is an instance of the class
c. Returns true or false"
:added "1.0"}
instance? (fn instance? [^Class c x] (. c (isInstance x))))
nil
```

So of course, instance will be faster than char .... If you know your class of object, it looks like instance?  will always be a little faster.  But there ya have benchmarking in Clojure with Criterium. :)

When reviewing this post, Ghadi said:
<blockquote>
One small note, instance? is intrinsified by the compiler. It turns into a bytecode instruction. Which you can see in this <a href="https://github.com/clojure/clojure/blob/master/src/jvm/clojure/lang/Compiler.java#L3767-L3778">clojure source</a>. It's faster than a function call.
</blockquote>
