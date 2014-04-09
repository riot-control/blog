---
layout: post
author: donbonifacio
title: "Confident Ruby Book"
date: 2013-12-23 13:47
comments: true
categories: books, [software development]
---

After reading [Objects on Rails](#) by the same author, I got the Confident Ruby book,
about the [confident code talk](#). If the first book was about web application
arquitecture, this one is about method arquitecture. How to write methods that
tell a good story, properly handle input, errors and processing.

The book has several patterns for several cases. If the talk opened my eyes for
this way of coding, the book provided many examples. I believe the best examples
to be on how to protect ourselfs from _NoMethodError_ exceptions. The book is
against type checking and states that checking for _nil_ is type checking. So 
things like:

``` ruby
obj.present?
obj.nil?
```

Should be avoided. The book provides several examples for several examples for many use cases.
