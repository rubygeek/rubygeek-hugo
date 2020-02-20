---
categories: null
comments: true
date: "2013-03-10T00:00:00Z"
published: false
title: 'Learning a new Language: Scala'
---

I've played around with Scala a bit here and there and it seems cool, some of the things I like in ruby are in scala. So for the next month I am going to see how much scala I an do what I accomplish.

I've read the scala chapter in 7 Languages in 7 Weeks, didn't work through exercises (yet). I really like how the author compares it to ruby a little and to Prolog (the previous chapter, I supposed I should have read the chapters in order!).

When learning something new it is a good idea to have a result in mind and somewhere to start. I looked on github for scala projects thinking maybe I can find something I can ready/study or even contribute to once I got the hang of it. I found a handful of projects that seem interesting and while doing that I found Twitter's [http://twitter.github.com/scala_school/](Scala School) which is a resource and I will read through before diving into the other scala projects I found.


Interesting observation that I didn't catch in my previous dabblings in scala: 

# Scala has a REPL
Start it with
``` scala
sbt console

scala> 1 + 1
res0: Int = 2

scala> res0 + 5
res1: Int = 7
```

Right off the bat the REPL assigns the result of your expression to a variable. Nice! In Ruby IRB you have to remember to do that yourself or use _ which holds the result of the most recently evaluated expression.


``` scala
scala> val two = 0 to 20 by 5
two: scala.collection.immutable.Range = Range(0, 5, 10, 15, 20)

scala> two.foreach { o => println(o) }
0
5
10
15
20

scala> val two = 0 until 20 by 5
two: scala.collection.immutable.Range = Range(0, 5, 10, 15)
```

Now thats not confusing.  0 to 20 by 5 is inclusive, meaning it doesn't include the end bit ...  0 to 20 by 5 includes the end number? strange 

ranges with letters including skipping?
```
scala> val two = 'a' to 'z' by 3
two: scala.collection.immutable.NumericRange[Char] = NumericRange(a, d, g, j, m, p, s, v, y)
```

Cool, I guess. 

