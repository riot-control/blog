---
layout: post
author: Paulo Silva
title: "Practical Object-Oriented Design in Ruby"
date: 2014-04-03 13:25
comments: true
categories: [software design]
---

Pratical Object-Oriented Design in Ruby by Sandi Metz

<img src="{{ root_url }}/blog/images/poodr.jpeg" height="300px" width="200px"/>

This is one of my favorite Ruby books ever. It is very well written and a joy to read. I highly recommend this book.

So, why is software design so important?

When starting a new software application you have certain requirments. You analise these requirments, build your software and it works. Unfortunately, those requirments will change and you will need to refactor your code. If you have a good design architecture, with loosely coupled classes, refactoring your code to meet the new requirments will be smooth. Otherwise, it will be very hard to make changes and you will feel the pain.

Every software application will need to change sooner or later. Having this in mind, you should care about your applications's design early on.


<!-- more -->


Software applications that are easy to change have these qualities:

* **Transparent** - The consequences of change should be obvious in the code that is changing and in distant code that relies upon it.

* **Reasonable** - The cost of any change should be proportional to the benefits the change achieves.

* **Usable** - Existing code should be usable in new and unexpected contexts.

* **Exemplary** - The code itself should encourage those who change it to perpetuate
these qualities.


Tests are another important aspect of well designed software. You can only refactor your code with confidence if you have tests. They also reduce the cost of maintaining your application and they supply documentation.

"An understanding of object-oriented design, good refactoring skills, and the ability to write efficient tests form a three-legged stool upon which changeable code rests. Well-designed code is easy to change, refactoring is how you change from one design to the next, and tests free you to refactor with impunity." - Sandi Metz
