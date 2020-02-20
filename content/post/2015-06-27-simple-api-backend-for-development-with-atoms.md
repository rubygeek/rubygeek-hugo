---
categories:
- clojure
comments: true
date: "2015-06-27T20:52:27Z"
title: Simple API Backend for Development with Atoms
---

When playing with liberator I wished I had a simple way to skip over having a database for playing around with data and I also wanted to write about Liberator but not have to get bogged down with Database stuff. I wrote a simple API backend that uses atoms and I'll write about this first, and this will be followed with some posts on liberator and what is the simplest setup and gradually building on.

As you may know, [Atoms](https://clojuredocs.org/clojure.core/atom) are a way to maintain state, a placeholder for changeable data. In my simple-api I defined two atoms, one for the data, an empty map and another for the `auto_increment` number.

```clj models.clj
(defonce id-seq (atom 0))
(defonce recipes (atom (array-map)))
```

Since I only want to define them once (and they will reset everytime this file is reloaded) I used `defonce`. Once I run that, I can see the values:

```clj repl
simple-api.models> @recipes
{}
simple-api.models> @id-seq
0
```

The @ is a reader macro that de-references the atom. Now we need a function to add a new recipe. For fun I used [Prismatic Schema](https://github.com/Prismatic/schema) which I wrote about [here](http://www.clojuregeek.com/2015/04/18/using-lein-try-to-learn-prismatic-schema/).

```clj models.clj
(defn add! [new-recipe]
  (let [id (swap! id-seq inc)
        new-recipe-with-id (assoc new-recipe :id id)
        recipe (coerce! Recipe new-recipe-with-id)]
    (swap! recipes assoc id recipe)
      recipe))

```

Here's what each line of the function is doing:


1: Incoming map of recipe data comes in as `new-recipe`.

2: Change the value of the `id-seq` atom using [swap!](https://clojuredocs.org/clojure.core/swap!). Swap! has always felt a bit strange, you have to pass in a function to change the value... you can't just pass in say `5` (5 is not a function..derp, to just replace the value use [reset!](https://clojuredocs.org/clojure.core/reset!)). In this case [inc](https://clojuredocs.org/clojure.core/inc) works fine and is what we need.

3: Create a `new-recipe-with-id` by adding in the id as a key/value pair by using [assoc](https://clojuredocs.org/clojure.core/assoc).

4: Take the `new-recipe-with-id` and use Coerce! returns a validated data structure or throws an exception if it is not matching structure and type.

5: Finally take the `recipe` and add to our `recipes` atom as a key/value pair of id/recipe.

6: Return the recipe.

Lets see it in action:

```clj repl

simple-api.models> (add! {:name "All American Beef Taco" :url "http://www.foodnetwork.com/recipes/alton-brown/all-american-beef-taco-recipe.html" :source {:name "Food\
Network" :url "http://www.foodnetwork.com"}})
{:source {:url "http://www.foodnetwork.com", :name "FoodNetwork"}, :name "All American Beef Taco", :url "http://www.foodnetwork.com/recipes/alton-brown/all-american-b\
eef-taco-recipe.html", :id 1}

simple-api.models> @recipes
{1 {:source {:url "http://www.foodnetwork.com", :name "FoodNetwork"}, :name "All American Beef Taco", :url "http://www.foodnetwork.com/recipes/alton-brown/all-america\
n-beef-taco-recipe.html", :id 1}}

simple-api.models> @id-seq
1
```

We add a recipe with all valid data. Then examine the two atoms: `@recipes` and `@id-seq` and both are correct.


Lets add another recipe:

```clj repl
simple-api.models> (add! {:name "Mexican Grilled corn" :url "http://www.foodnetwork.com/recipes/tyler-florence/mexican-grilled-corn-recipe.html" :source {:name "FoodN\
etwork" :url "http://www.foodnetwork.com"}})
{:source {:url "http://www.foodnetwork.com", :name "FoodNetwork"}, :name "Mexican Grilled corn", :url "http://www.foodnetwork.com/recipes/tyler-florence/mexican-grill\
ed-corn-recipe.html", :id 2}

simple-api.models> @recipes
{2 {:source {:url "http://www.foodnetwork.com", :name "FoodNetwork"}, :name "Mexican Grilled corn", :url "http://www.foodnetwork.com/recipes/tyler-florence/mexican-gr\
illed-corn-recipe.html", :id 2}, 1 {:source {:url "http://www.foodnetwork.com", :name "FoodNetwork"}, :name "All American Beef Taco", :url "http://www.foodnetwork.com\
/recipes/alton-brown/all-american-beef-taco-recipe.html", :id 1}}

simple-api.models> @id-seq
2
```


Here's the tests I wrote for this function:

```clj models_test
(use-fixtures :each
  (fn [tests]
    (add! {:name "test" :url "test.com" :source {:name "mom" :url "mom.com"}})
  (tests)))


(deftest test-repository
  (testing "adding recipe"
    (let [recipe {:name "test more" :url "testmore.com" :source {:name "mom" :url "mom.com"}}
          r (add! recipe)]
    (is (= 2 (recipe-count))))))

```

The test fixture adds one in, and then my test adds another making it count to be equal to 2. I'm not really liking how I wrote this test...but it works. Suggestions?

BTW, here's the the `recipe-count` function:

```clj models.clj
(defn recipe-count []
  (count @recipes))
```


Now lets write a function to return a vector of recipe maps:

```clj models.clj
(defn get-recipes [] (-> @recipes
                         vals
                         reverse))
```

In the code I found as inspiration for [this](https://github.com/metosin/compojure-api-examples/blob/master/src/compojure/api/examples/domain.clj) the author doesn't use the `@` but passes it to `deref` ... I think I like using the spiral as it results in shorter code. Any reason why one is better than the other?


Now a function to return just one recipe by id:

```clj
(defn get-recipe [id] (@recipes id))
```

Since the key is just the id it is a simple task to look up by id.

Now, the function to update a recipe:

```clj
(defn update! [recipe]
  (let [recipe (coerce! Recipe recipe)]
    (swap! recipes assoc (:id recipe) recipe)
    (get-recipe (:id recipe))))

```

Swap here just takes the recipe given and puts it in place according to id. Not too complicated here.

Finally, deleting a recipe by id:

```clj
(defn delete! [id] (swap! recipes dissoc id) nil)
```

Disassociate the value at id and return nil.

I was pretty happy with how these functions turned out, and I could easily replace them with korma or honeysql when I was ready. I debated calling get-recipe get! and get-recipes get-all! but since those are not desructive like add, update, delete I thought it was fine to leave them.

I'm still learning best and idomatic clojure, so please speak up if you have suggestions! I will be using this [simple-api](https://github.com/rubygeek/simple-api) in future blog post as I start to write about [liberator](http://clojure-liberator.github.io/liberator/) and see my [tests](https://github.com/rubygeek/simple-api/blob/master/test/simple_api/models_test.clj) for the rest of the functions.
