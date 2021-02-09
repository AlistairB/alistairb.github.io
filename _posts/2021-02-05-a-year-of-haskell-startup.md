---
layout: post
title: "Reflections On A Year Of A Haskell Startup"
tags: [haskell, gcp]
comments: true
---

Almost exactly one year ago I quit my job to attempt to create a Haskell startup as a solo developer. I had about 20 ideas, but eventually settled on the idea of dependency project health tracking with [Deadpendency](https://deadpendency.com/).

## Why Use Haskell?

Since about 2016 I developed a strong ~~obsession~~ love of Haskell. Prior to learning Haskell, I was an experienced OO style developer and didn't really know how to improve my raw programming ability. Haskell introduced me to the world of FP which has an almost infinite depth of concepts to learn that can be leveraged to improve code quality and application architecture.

Haskell is challenging to learn, but extremely fun to write. For my own learning and pleasure, if my startup succeeds, I want to be doing Haskell.

Additionally, I think Haskell is the best general purpose programming language (that you can use in production). In particular, Haskell excells at writing 'boring' business applications which is typically what I work on. ['Why Haskell For Production'](https://www.foxhound.systems/blog/why-haskell-for-production/) goes into more detail on the benefits Haskell offers.

## The Setup Phase

Probably the most challenging part was building out a skeleton architecture to hang my code on. I decided to go with, even within Haskell, fairly advanced libraries of [`servant`](https://docs.servant.dev/en/stable/) and [`fused-effects`](https://hackage.haskell.org/package/fused-effects).

I spent a fair amount of time banging my head against a wall trying to get these libraries to work nicely together. Part of this was a lack of Haskell ability on my part. I had prepared as best I could, but Haskell is deep and I needed to learn more to work day to day with it. I was lucky enough to eventually find [an example](https://github.com/mitchellwrosen/hspolls) that marries these two libraries together, which was a life saver. I'm sure I would have gotten there eventually, but I was in a bit over my head at that point.

Haskell is awesome, but like most language there is cruft and legacy to be avoided. Haskell has a standard library known as [`base`](https://hackage.haskell.org/package/base) which unfortunately has a fair amount of unsafe or unperformant functions included. As such I went with an alternative standard library [`relude`](https://hackage.haskell.org/package/relude) that builds on and improves `base`. On top of this, there are many core libraries that are not part of the standard library I wanted to use and have nice patterns around.

Additionally, I was [deploying to google cloud](/haskell-on-google-cloud-is-great) and so needed to figure out good patterns for that integration from Haskell.

This setup effort was quite challenging, yet it only took about 2 weeks to have a good foundation of code to start building my business logic upon.

## Building It Out

This is when it started to get really fun. I had my core patterns set out and I could focus on building a pipeline. I had the following phases (simplified)

## Launching

## Conclusion
