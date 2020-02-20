---
categories:
- design patterns
- ruby
date: "2009-11-24T00:00:00Z"
description: ""
title: Builder Pattern in Ruby
---

Call me crazy, but this is why I classify myself as a language geek. When I learn something fascinating, i wonder hmm how can I do that with X language? My <a href="http://www.rubygeek.com/2009/11/22/builder-pattern-in-java/">last post</a> I did an example of the Builder pattern as described in <a href="http://java.sun.com/docs/books/effective/">Effective Java by Joshua Bloch</a>. The main motivation for me to use Builder is to have flexible parameter lists, without worrying about order of parameters (there are a few other reasons outlined in the book, but this is what I find cool). 

Its too easy.

My class:
``` ruby
class Address
  attr_accessor :street, :street2, :city, :state, :zip, :address_type
  
  def initialize(&block)
    
    #set default values
    self.city = "Chicago"
    self.state = "IL"
    
    #set value from block 
    instance_eval &block if block_given?
  
  end
  
end
```

And the usage of it:
``` ruby
  def testDefaultValues
    myaddress = Address.new
    assert_equal("Chicago", myaddress.city)
    assert_equal("IL", myaddress.state)
  end
  
  def testStreetStateZip 
    myaddress = Address.new do 
      self.street = "this is the street"
      self.zip = "11111"
    end
    assert_equal("Chicago", myaddress.city)
    assert_equal("IL", myaddress.state)
    assert_equal("this is the street", myaddress.street)
    assert_equal("11111", myaddress.zip)
  end
```

Wow, huh? Compare to 


``` java
public void testBuilderDefaults() {
  Address expected = new Address.Builder("Chicago", "IL").build();
  assertEquals("State is IL", "IL", expected.getState());
  assertEquals("City is Chicago", "Chicago", expected.getCity());
}
```

then the optional params are chained to that: 
``` java
 public void testBuilderStreetStateZip() {
  Address expected = new Address.Builder("Chicago", "IL").street("this is an address").zip("11111").build();
  assertEquals("this is an address", expected.getStreet());
  assertEquals("11111", expected.getZip());
  assertEquals("IL", expected.getState());
  assertEquals("Chicago", expected.getCity());
}
```

Yes... sometimes its hard not to gloat. :-D

yeah yeah yeah, its not really "fair" to compare strongly typed languages... but as I said, my motivation is to have easy parameter lists without having to remember specific ordering of params for constructor or use multiple set methods.
