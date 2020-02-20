---
categories:
- books
- clojure
comments: true
date: "2015-08-30T21:12:57Z"
title: Book Review - Clojure Applied
---

[Clojure Applied](http://amzn.to/2qO825o) -- Written by [Alex Miller](https://twitter.com/puredanger) and [Ben Vangrift](https://twitter.com/bvandgrift).

I am a junkie for books and Clojure books are no exception. I have been doing Clojure since Jan 2014 and feel like I kinda got the hang of it but now what? When I saw *Clojure Applied "From Practice to Practitioner"* was being written I kept a close eye on Pragmatic Programmer's coming soon list and an eye on twitter. The day I heard the beta book was available I bought it. When I was offered a free copy to do this review, I said thanks but I already have it and I'll still be happy to write a review. :)

I was not disappointed as I dived into the book, this is just the thing I needed. Here's is how to do clojure for the real world with real application of clojure’s concepts applied.

This book is in 3 parts:

* Foundations
* Applications
* Practices

## Foundations

### Chapter 1: Modeling your Domain
This section covers aspects of your application including modeling your domain using maps and record and when to use each of those. Getting your mind around immutable data is one of the hardest parts for me in getting into clojure and the discussion on entities here really helped solidify the concepts. Once your data is modeled you want to do operations, in OOP languages you have polymorphism. In clojure there are multi methods and protocols. Here are examples of each and when you would use them.

### Chapter 2: Collecting and organizing your data.
This chapter talks about collections and which one to use and how to write a custom sorting function. Then it goes on to talk about updating data - wait I thought data was immutable in clojure? It is, so when you update a collection it returns a new copy with the data updated. Its typical of mutable functions to end in ! (danger Will Robinson!), This chapter closes with how to define a "new" datatype and what is involved there.

### Chapter 3: Processing Sequential Data
This chapter starts with explaining how sequences are an abstraction connecting collections and the functions that act on those collection. This is the really beauty of clojure and at first mind blowing then once it sinks in, you can imagine how you could have thought differently about the concept. Reducing, Filtering, Selection, Sorting, Removing Duplicates and grouping are all operations you will want to do on your collection once you have it.

## Applications

### Chapter 4: State, Identity, and Change
Change, if our applications couldn’t change they would be boring, probably even useless. If you need more than one change at a time, you should use a transaction. Different data types (atom, ref, etc) have different functions to update and this goes over each of them in detail.

### Chapter 5: Use your cores
This chapter talks about threads and introduces the concept of agents and promises to transfer a value from one state to the other. The talk about using reducers to process data in parallel and the performance considerations. Ending with with discussion about thinking in process using channels in core.async to create a sort of a pipeline for processing data. More meat to this chapter than I can summarize here.

### Chapter 6: Creating Components
One of my favorite chapters. Now that you have your structure and processing down, how to combine it into something we can put in a package with a nice bow? There are a few ways to organize this and the first is a namespace. You want to use one or more for your code so it is easily read and comprehended. The chapter goes over some common namespaces used in project and what might go into those. You can connect your components with channels and structure their lifecycle with a record and functions to make start and stop. Leading to the next chapter...

### Chapter 7: Compose your Application
Composing your application! Early in the chapter it starts talking about [Stuart Sierra's](https://github.com/stuartsierra/component) library name precisely that. Component. It’s like the previous chapter laid the foundation of why, and now this is how. Then putting those into a system and using profiles and configs to make your code operate in different environments (dev, staging etc).

## Practice

### Chapter 8: Testing Clojure
Ok maybe this is my favorite chapter. My love of testing has been equated with "sickness" for loving tests so much.  This chapter kicks off with repl-based testing, by developing your function and calling it in the repl to work out the inputs/outputs. Then moving on to example based testing which I know as unit testing. The new part to me is in Clojure are "are" test function which can easily test a lot of short tests. Testing exceptions is also a thing which is mentioned briefly. Then some other features of clojure test are fixtures, a handy way to provide baseline data for testing. The section ends with a mention of 'Expections' test library which takes a different approach than 'clojure.test'. This chapter ends with talking about property based testing and generators, which if you don’t know about them will blow your mind and leaving you wondering why we haven’t been testing this way all along, :)

### Chapter 9: Formatting Data
This chapter seems shorter compared to the other chapters, talking about edn, son and transit. The first two I get and using them in clojure is awesome. But Transit, the authors follow the explanation with a concrete example, I think I get it but I will need to use it in a project for it to really sink in.

### Chapter 10: Getting out the Door
This is where it talks about publishing your code and deployment. A few things to think about when publishing your code is to choose the collaboration platform (Github, Bitbucket etc), the contributing agreements and licensing and the minimum files you should have in your repo. When your code is ready to be published as a library most people use Clojars, Maven Central Repository or a self hosted Maven repo. The last part of this chapter is deploying your application to heroku or provisioning your own server by running a jar or to deploying to an application server.

## Appendix
This contains two sections: Roots which talks about some of the concepts and Thinking in Clojure which describes the mindset of how clojure is written and what I think community strives to be like.

Overall this is a fantastic book and I’ve tried to summarize the key points to help you make the decision if this book would help you in your clojure career. I bet it will! :) Go [Buy it!](http://amzn.to/2qO825o)
