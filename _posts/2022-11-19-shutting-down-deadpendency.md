---
layout: post
title: "Shutting Down Deadpendency"
tags: [haskell]
hidden: true
comments: true
---

In Feb in 2020 I quit my fulltime job to create my idea for a startup, [Deadpendency](https://deadpendency.com/){:target="_blank" rel="noopener"}. There were some [trials and tribulations](/my-experience-creating-a-one-person-startup), but ultimately I was successful and a built the thing.

Unfortunately though, I have decided to pull the plug and I thought I would write up my reasoning and some reflections.

## What Is Deadpendency?

The idea is there is a bunch of due diligence developers perform when selecting what packages to depend on. This is a manual and one time effort, however what is an active project, may become abandoned or deprecated over time. Deadpendency is a continual check that alerts deveopers when their dependencies are 'dead'.

In other words, it gives you something like:


<img class="center-image" width="800" src="/images/deadpendency-example.png" alt="Deadpendency Example"/>

Developers can respond to 'dead' dependencies and switch to actively maintained ones (or ignore the dependency if they don't have other good options).

## Why Did I Build It?

I built it because:

* It was a small project I could build by myself.
* It was a new concept, thus no direct competitors.
* I er thought it was a cool idea.

To be honest I did not deeply evaluate how useful it would be to a team of developers. I did think most teams would have bigger fish to fry than 'dead' dependencies, but surely some teams would benefit.

## Did Anyone Use it?
