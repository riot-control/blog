---
layout: post
published: true
author: Ricardo Fiel
title: "The small change"
date: 2014-08-28 01:09
comments: true
categories: [culture]
---

So, today I screwed up. Plain and simple.

My team has a ~~big~~ huge delivery happening in about a month. The process was painful and long. More than ever, there's no room for scope changes, last minute requests or anything else that may, in any way, affect the work they're doing. We've been more and more cautious on this but still, today something happened.

Yesterday, I had a request to "just add this tiny piece of code on the app" so that some marketing stuff (which is actually cool) could be done. Which I said no, not today, everyone needs focus. So I asked a team member when they could spare 1 hour (tops!) to do this and we decided to do it today, early morning. I mean _what could possibly go wrong_?

So, adding this tiny piece of code was simple, fast and didn't really take much more than what we'd expected. So a build was triggered, a PR (Pull Request) was created and all tests passed. "Oh, but there's this small change I need to make so it works properly". Ok. No worries there. Tests passed on dev machine, pushed to github, BANG! build server tests failed. And failed again. And again. For no apparent reason. For some reason only known to I don't know who, local tests started failing. No apparent reason. CPU 100%. Then they passed again. Build server tests still failed. Trigger another branch build. BANG. Also failed. Trigger another project build. BANG. Turns out, the problem was with the platform we use for build servers. They had a glitch and all builds failed. They applied a hotfix and everything was back to normal. All builds passing.

Just for context, build were taking almost one hour until they finally failed.

One freaking day lost on this! That's 8 hours of dev cost + some hours of another dev + some hours of me. That's **expensive**!

How many times have you gone through this before?

Next time, don't think twice. You can do it, but make sure you don't impact the team working on the critical stuff. You can be *sure* that will be quick. If only you weren't overconfident (hey, we're humans after all, this is normal) and wouldn't have to deal with uncertainty.

Lesson re-learned.

Joel Spolsky has some articles on this. [Here's one.](http://www.joelonsoftware.com/items/2007/10/26.html)
