---
layout: post
published: true
title: "Pure functions"
author: donbonifacio
comments: true
categories: [software development]
---

While learning scala/clojure and functional programming I have come across a
very familiar concept: [pure functions](http://en.wikipedia.org/wiki/Pure_function).
It reminds me of an [interactor](/2013/12/03/the-almighty-interactor/), but I
feel that is much more valuable as a concept. The _wikipedia_ says that a function may
be considered pure if:

<!-- more -->

> 1. The function always evaluates the same result value given the same argument value(s). The function result value cannot depend on any hidden information or state that may change as program execution proceeds or between different executions of the program, nor can it depend on any external input from I/O devices.
> 2. Evaluation of the result does not cause any semantically observable side effect or output, such as mutation of mutable objects or output to I/O devices.

It may be difficult to build these kind of functions, it forces us to think
differently. Having a pure function doing our logic means that we are forced
to abstract from gateways, storages, and all kind of foreign dependencies.

However, building these pure functions yields several advantages:

* The function code is cleaner and easier to understand
* It's easier to test; no dependecies, just focus on the logic
* Because we don't have dependencies, we don't need mocks while testing
* Pure functions can be easilly run on other threads, without worrying about
  sync mechanisms


