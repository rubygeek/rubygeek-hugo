---
categories: null
comments: true
date: "2014-10-06T00:00:00Z"
title: Testing routes with RSpec and Minitest
---

The poor often forgotten routes folder in your tests or specs folder (if you even have one!). People don't know what goes in them or how to write them. I like to write them because they are pretty easy to write and often a great first-step when it comes to a legacy codebase. 

It gives you the chance to get a good look at that routes file and see if there are routes there you really don't need? Often the `resources :whatever` is the culprit, the others are either out of date (the controller/action was removed but routes file never updated) .. I wondered why should you only include the routes you need? what harm is there? It make the `rake routes` file cleaner but are there other reasons to be very specific in `routes.rb` ? 

So I sought the wisdom of  twitter:       

    Peter Harkins @pushcx
    Documentation to future coders.
    --
    Karthik Hariharan @hkarthik 
    avoids the "action controller method not found" errors and pushes 
    a 404 instead of a 500. Really important with REST APIs.
    --
    Brad Fults @h3h
    FWIW, I highly recommend avoiding `resources` and preferring 
    a `get/post/patch` for every route. Much clearer.  
    --
    Caleb Thompson @calebthompson 
    :only 

I got some really good answers!!  [See all the responses](https://twitter.com/rubygeek/status/516612558238015488)

Avoiding the resources magic is one way to make it very clear:

```ruby
get "/users", "users#index"
get "/users/:id", "users#show" 
post "/users", "users#create"
```

Verses resources style:

```ruby
resources :users, only: [:index, :show, :create]
```

Using `only` is also pretty clear and maybe less messy? This probably come down to personal preference and which way allows you to sleep best at night. Decide with your team so you can be consistant in your `routes.rb` file.

So how do you test them?

It depends on what test framework you are using. Here's what I've figured it out.

### RSpec

```ruby
  context "should route to member_sessions" do
    it "should initialize a new session" do
      expect(get: "/member_sessions/new").to route_to(controller: 'member_sessions', action: 'new')
    end
    it "should create a new session" do
      expect(post: "/member_sessions").to route_to(controller: 'member_sessions', action: 'create')
    end
    it "should destroy a session" do
      expect(delete: "/member_sessions/1").to route_to(controller: 'member_sessions', action: 'destroy', id: "1")
    end
  end
```

This will test that the method, url and params will go to the right controller/action. It seems pretty straight forward right? 

### Minitest / TestUnit

We have three methods: 

* `assert_generates` - testing that the provided url, will break down to a controller,action and parameters
* `assert_recognizes`   - testing that rails will recognize the route, given controller and action, it goes to a certain method and path 
* `assert_routing` - a combination of the above, almost literally, if you look at the source! 

One thing I can't figure out is how to test the methods along with the routing info like I can with RSpec.  We can test in other ways (feature tests), but its not quite as nice as RSpec. I guess that is just a difference in the two frameworks. I  found when the same path is common to multiple methods, than `assert_generates` works and `assert_routing` does not (and `assert_recognizes` does not also). For example in the case of Create, Update, Delete and Show they use the same path so I have to use the `assert_generates` to that the path goes to the right controller. 

```
| method | path          | controller |
| ------ | ------------- | ---------- |
| create | /register/:id |  register  |
| update | /register/:id |  register  |
| delete | /register/:id |  register  |
| show   | /register/:id |  register  |
```

So once I realized that, I understood (duh) which route testing methods to use. Some might argue these are pointless, but I still think its valuable as it forces you to really think about your routes.


```ruby
class RoutingTest < ActionDispatch::IntegrationTest
    it "routes to register index" do
      assert_generates "/register", controller: "register", action: "index"
    end
    it "routes to register show" do
      assert_generates "/register/1", controller: "register", action: "show", id: "1"
    end
  end
```

These are mostly the same, except for the starting of the test. The testunit style has a class and spec style has a description. The describe string ** must ** end in Acceptance Test.

```ruby
describe "Routing Acceptance Test" do
  it "routes to register index" do
    assert_generates "/register", controller: "register", action: "index"
  end

  it "routes to register show" do
    assert_generates "/register/1", controller: "register", action: "show", id: "1"
  end
end
```

As I've been writing route tests, I've increase the security, clarity and interface to my apps. My routes file has become a very clear documentation of the api of my application. I hope that testing routes becomes as important to you as it is for me!

