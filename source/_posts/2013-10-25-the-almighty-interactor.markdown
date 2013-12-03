---
layout: post
published: true
title: "The almighty interactor"
author: donbonifacio
date: 2013-12-03 13:21
comments: true
categories: [software development]
---

The concept of the _interactor_ is the biggest breakthrough that I have experienced in my
evolution as a programmer. It's simple, and it has always been right beside me. But
I have only acknowledged it when I saw [Arquitecthure: the lost years](/2013/10/11/architecture-the-lost-years/).

The concept is very simple. An interactor is:

<!-- more -->

* A class that handles business logic
* It solves only one problem
* It has a name in the form of an action: CreatePost, SendEmail, FinalizeDocument
* It does not know anything about persistence (e.g. databases, ORM's)
  and delivery mechanisms (e.g. Sinatra, Rails, CLI)
* It maps directly to an use case, and only one use case

An interactor like CreatePost needs to persist stuff. But it cannot know how.
To do so, it delegates the persistance to another class, a _gateway_. This gateway
may be an object that knows the persistence layer, or may be a dummy memory representation.

This separation makes sure that the interactor stays focused on it's task. And remains
very light. By being a plain ruby object, the interactor doesn't have dependecies
that will slow it down when running/booting tests.

Here's a small example:

``` ruby
class AddNumbers

  def initialize(*numbers)
    @numbers = numbers || []
  end

  def may_run?
    true
  end

  def run
    @numbers.inject(&:+)
  end

end
```

Now the application knows how to add numbers via the *AddNumbers* class. We can create some
fast, database and framework independent unit tests. We can run it on _irb_, on a rake task
or on an async worker.

We build our application on top of interactors. And so should you.
