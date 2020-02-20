---
categories:
- clojure
- ruby
comments: true
date: "2018-01-03T00:00:00Z"
title: One-Liners in Clojure and Ruby
---

A quick post to start off the new year. 

You and your co-workers can't decide where to go for lunch? 

```ruby
▶ ruby -e "puts [:tacos, :chicken, :bbq].sample"
chicken
```

```clojure
▶ clj -e "(rand-nth [:tacos :chicken :bbq])"
:tacos
```

Observations: 

Ruby I had to tell it what to do with the output, to `puts` it. It didn't automatically display like if I had done that line in the irb repl. Oh and don't do what I did at first and forget the `,` (comma) :) 

Clojure, since everything returns a value, I just had to call the function.

Have fun using code to make important decisions, like .. Lunch :) 