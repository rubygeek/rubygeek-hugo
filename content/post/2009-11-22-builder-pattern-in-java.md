---
categories:
- design patterns
- java
date: "2009-11-22T00:00:00Z"
description: ""
title: Builder Pattern in Java
---

I have been reading <a href="http://java.sun.com/docs/books/effective/">Effective Java by Joshua Bloch</a> and learned something new so I wanted to try it out.

Making long parameter lists in your constructor is not fun and prone to errors. Previously I thought that best way to handle this situation was to make a bunch of setter methods. This can get kind of tedious and hard to follow when there are alot of parameters:


```java
Address myaddress = new Address("Chicago", "IL");
myaddress.setZip("44422");
```


Using a Builder object:
```java
Address myaddress = new Address.Builder().city("Chicago").state("IL").zip("44422").build();
```


Using a builder class allows you to specify params in any order too! 


The required parameters are set in the  Builder constructor:


```java
public void testBuilderDefaults() {
  Address expected = new Address.Builder("Chicago", "IL").build();
  assertEquals("State is IL", "IL", expected.getState());
  assertEquals("City is Chicago", "Chicago", expected.getCity());
}
```

then the optional params are chained to that: 

```java
public void testBuilderStreetStateZip() {
  Address expected = new Address.Builder("Chicago", "IL")
                                       .street("this is an address").zip("11111").build();
  assertEquals("Address is this is an address", "this is an address", expected.getStreet());
  assertEquals("Zip is 11111", "11111", expected.getZip());
  assertEquals("State is IL", "IL", expected.getState());
  assertEquals("City is Chicago", "Chicago", expected.getCity());
}

```


The book goes into more discussion that I haven't quite wrapped my head around yet, but this is interesting stuff .. even though it is java ;)

