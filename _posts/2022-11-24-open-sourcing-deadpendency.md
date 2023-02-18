---
layout: post
title: "Open Sourcing Deadpendency"
tags: [Deadpendency, haskell]
comments: true
hidden: true
---

I created this thing called [Deadpendency](https://deadpendency.com/){:target="_blank" rel="noopener"}, which is a GitHub app which checks for dead dependencies in your GitHub repo.

There were some [trials and tribulations](/my-experience-creating-a-one-person-startup), but ultimately I was successful and I built the thing. In Haskell no less, which [worked out pretty great](/reflections-on-haskell-for-startup)!

Unfortunately, the product itself was not successful at getting paid users and I [pulled the plug](/shutting-down-deadpendency).

I have decided to open source it, so at least it might still serve some purpose!

## What Is Deadpendency?

tl;dr

<img class="center-image" width="800" src="/images/deadpendency-tldr.png" alt="Deadpendency Tldr"/>

## Why Open Source It?

In a nutshell, I don't have plans to work on it anymore, so it can at least have a chance of providing others some value if it is open source. It was a startup + I was juggling fulltime work for part of it, so certain corners were cut.. Nonetheless, I am generally quite proud of it and think it is may be interesting to Haskell developers in various ways.

I won't go into too much detail in the post, as I have put lots of detail in the readme and linked docs. However, what might be of value?

### A Haskell 'DevOps' Pattern

Probably the best aspect is something I have [iterated on for a number of years](https://github.com/AlistairB/docker-stack-haskell) is the way they code is built, tested and deployed, both locally and in CI. This solution is quite simple and offers great cacheability and reproducibility.

### The Deadpendency Solution

Purely as a solution to the problem, I think it is quite clean. I had to iterate on it a lot as it is seeminly pretty simple, but in actuality quite complex.

### A Complete Monorepo Haskell Solution

Contained is a complete solution with all the terraform to run Deadpendency, or any API feeding into a pipeline, on [google cloud run](https://cloud.google.com/run). It is done in a monorepo style with minimal overhead to connect a bunch of small components (AWS Lambda like functions) together. I think it worked out quite nicely.


### Lots Of Pretty Diagrams And Reflections

I tried to diagram and document as much as possible. You can trace the documentation through from the readme. Hopefully this is interesting to some people :)

## Can I Start Checking My Dependencies Right Now!?

No, not really. It is a very specific solution designed to run as a GitHub app in google cloud. Everything you need is there to convert it to some CLI tool, however it would take some time to do this conversion. I do not have plans to do so currently.

## Conclusion
