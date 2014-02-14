---
layout: post
published: true
author: donbonifacio
title: "Objects on Rails Book"
date: 2014-02-14 13:47
comments: true
categories: books, [software development]
tags: ruby
---

Some time ago our team started to write code differently. Inspired by [Architecture: The Lost Years](/2013/10/11/architecture-the-lost-years/),
we started using [interactors](/2013/12/03/the-almighty-interactor/) and [presenters](/2013/11/27/refactoring-views-with-presenters-ruby-on-rails/), greatly improving our code base.
We also started writting _rails-free_ unit tests. It's good to know that [we are not alone on this process](http://devblog.reverb.com/post/70344683203/5-architecture-anti-patterns-and-solutions-for-large).
The [Objects on Rails book](http://objectsonrails.com/) shows a step by step process to build this kind of code.

<!-- more -->

I really loved this book. It's more on the rails side than I would like, but it
still provided an enourmous amount of insights on application development. It's
also very _ruby intensive_, and I confess that I prefer code without deep ruby tricks.

I'm awalys saying that I don't like to use constants in code. I always like an
abstraction on top of constants. Imagine that you have:

``` ruby
PER_PAGE = 10
```

And you use the *PER_PAGE* all over the place. Now if you need to have logic on
_per page_, for example, depending on the current user, you'll have to change it
all over the place. That's why I always do:

``` ruby
def per_page
  10
end
```

Objects on Rails made me think about this on a wider scope. This is a code smell:

``` ruby
doc = Document.find(params[:id])
```

I we have this code all over the place, what happens if we need to inject some
logic on that find? An alternative:

``` ruby
def find_document(id, finder=Document)
  finder.find(id)
end

doc = find_document(params[:id])
```

This makes the code so much better for so many different reasons... The author
goes on step further and makes all ActiveRecord interface private on the models,
forcing you to write better code.

You can read the book online for free or spend 5$ and have it digital. It's a must
read!
