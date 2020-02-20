---
categories: null
comments: true
date: "2012-11-20T00:00:00Z"
title: Simple Breadcrumbs
---

I needed a simple breadcrumb module/class that would let me set the first crumb and add to it later. I looked at [www.ruby-toolbox.com](http://www.ruby-toolbox.com) and saw a bunch, none of them looked simple enough for me. I already had a template so I dont care about changing that...so I whipped up my own breadcrumbs:

``` ruby
class Breadcrumb

  Crumb = Struct.new(:name, :link)

  attr_reader :crumbs

  def initialize(name = "Home", link = "/")
    @crumbs = []
    self.add(name, link)
  end

  def add(name, link)
    @crumbs << Crumb.new(name,link)
  end

end
```

After watching [www.rubytapas.com](http://www.rubytapas.com) talk about using Struct (I had never really used it before or understood why not just make a class). At first I defined crumb outside of Breadcrumb, then after I had tests, moved inside and it still worked. It seems a bit odd, to have that inside Breadcrumb, but its only accessible inside Breadcrumb so I guess that is good.


``` ruby
describe Breadcrumb do

  describe '#new' do

    it 'sets default crumb of Home /' do
      @breadcrumb = Breadcrumb.new
      @breadcrumb.should have(1).crumb 
      @breadcrumb.crumbs.first.name.should == 'Home'
      @breadcrumb.crumbs.first.link.should == '/'
    end

    it 'sets initial crumb of Main /main' do
      @breadcrumb = Breadcrumb.new('Main', '/main')
      @breadcrumb.should have(1).crumb 
      @breadcrumb.crumbs.first.name.should == 'Main'
      @breadcrumb.crumbs.first.link.should == '/main'
    end
  
  end

  describe '#add' do

    it 'a second crumb' do
      @breadcrumb = Breadcrumb.new
      @breadcrumb.add('Products', '/products')
      @breadcrumb.should have(2).crumbs
    end

  end

```


output of spec doc

```
Breadcrumb
  #new
    sets default crumb of Home /
    sets initial crumb of Main /main
  #add
    a second crumb
```

I did a codereview on twitter and got some good comments, and my code is better because of it! Thanks [@benhamil](http://www.twitter.com/benhamill) and [@therealadam](http://www.twitter.com/therealadam). See the [comments on initial versions](https://gist.github.com/4111823) of the code.
