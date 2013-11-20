---
layout: post
title: "The almighty interactor"
author: donbonifacio
date: 2013-10-25 13:21
comments: true
categories: [software development]
---

The concept of the _interactor_ is the biggest breakthrough that I have experienced in my
evolution as a programmer. It's simple, and it has always been right besied me. But
I have only acknowledged it when I saw [Arquitecthure: the lost years](/2013/10/11/architecture-the-lost-years/).

The concept is very simple. An interactor is:

* A class that handles business logic
* It solves only one problem
* It has a name in the form of an action: CreatePost, SendEmail, FinalizeDocument
* It does not know anything about persistance, databases, rails, etc
* It expects simple objects, operates on them, and returns a result

An interactor like CreatePost needs to persist stuff. But it cannot know how.
To do so, it delegates the persistance to another entity, a _gateway_. This gateway
may be an object that knows rails, or may be a dummy memory representation.

This separation makes sure that the interactor stays focused on it's task. And remains
very light. We can use it on very fast unit tests.

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
