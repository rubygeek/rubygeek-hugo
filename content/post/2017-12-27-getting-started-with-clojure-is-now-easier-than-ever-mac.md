---
categories: clojure
comments: true
date: "2017-12-27T00:00:00Z"
title: Getting Started with Clojure is now easier than ever - On a Mac
---

As of Dec 8, 2017 a [brew recipe](https://github.com/Homebrew/homebrew-core/blob/master/Formula/clojure.rb) has been added to install Clojure with `brew install clojure` and it gives you commands `clj` and `clojure`. 

To try it out, I created a file, `test.clj` with the following:

```clojure
#!/usr/bin/env clojure

(println "Hello World")

(println (+ 1 2 3 4 5))

(println (clojure.string/upper-case "hello world"))
```

After you make it executable with: 

```bash
chmod u+x test.clj
```

You can execute it:

```bash
▶ ./test.clj
Hello World
15
HELLO WORLD
```

A popular way to play with Clojure is to use a repl. You can start a repl, by using either `clj` or `clojure` without a file name

```clojure
▶ clojure
Clojure 1.9.0
user=> (+ 1 2 3 4)
10
user=> (apply str [1 2 3 4])
"1234"
user=> (map #(* 2 %) [1 2 3 4])
(2 4 6 8)
```

I was curious about commands `clojure` and `clj` and found an explaination of how it [works](https://clojure.org/reference/deps_and_cli#_usage).

You can have a `deps.edn` file to include dependencies. At first I was confused on `:mvn/ prefix`, and thought how to include a library from `clojars`? So I looked farther and found that `:mvn` includes this group of locations (which includes clojars), as shown [here](https://clojure.org/reference/deps_and_cli#_providers).

```clojure
{:mvn/repos
 {"central" {:url "https://repo1.maven.org/maven2/"}
  "clojars" {:url "https://repo.clojars.org/"}}}
```

to test this out, I created a directory and put a file `deps.edn` file with the following: 

```clojure
{:deps
 {anagrams {:mvn/version "4" }}}
```

Anagrams is a fun little library I found browsing [clojars](https://www.clojars.com). It takes a list of words separated with `\n` and finds words that are anagrams of a word you give it. My example is a little contrived, but see it working: 

Start the repl in that same directory, if needed it will download and install dependencies: 

```clojure
▶ clj
Clojure 1.9.0
user=> (require '[anagrams.core :as a])
nil

user=> (a/set-word-list! "cat\ntac\natc\ndog\ngod\ntaco")
"done"

user=> (a/anagrams-of "cat")
#{"tac" "atc" "cat"}

user=> (a/anagrams-of "dog")
#{"dog" "god"}
```

Cool, that's fun :) 

A new way to try out Clojure.. (on a mac at least). Pretty easy!