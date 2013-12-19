---
layout: post
published: true
title: "Development by copy paste"
author: donbonifacio
date: 2013-12-18 13:21
comments: true
categories: [software development]
---

I believe that *development by copy paste* is the worst work process that a programmer
may have. I'm talking about having a piece of software working, and copying it
fully to another location on the same project, and performing some changes.

<!-- more -->

 I may write some poor code if I'm tired or in a rush. My compromise is that a
 piece of code may be poorly written, but there is only one point of access. It
 may be refactored later and unit tested later. And all the code that uses the bad one
 won't need to be changed (hopefully :).

 When we copy paste large portions of code, we are creating a maintenance hell.
 No longer will we be able to change something on just one place. We'll have to find
 all related places to change that. And guess what? We will always miss the majority
 of them.

Some may argue that using this process is faster. I disagree. And I hope I'll never
have to support code from these guys.
