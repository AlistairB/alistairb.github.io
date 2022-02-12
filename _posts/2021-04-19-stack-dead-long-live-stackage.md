---
layout: post
title: "Stack is dead, long live Stackage!"
tags: []
comments: true
hidden: true
---

[Haskell Stack](https://www.haskellstack.org/) (and [Stackage](https://www.stackage.org/)) have been a crucial part of the Haskell ecosystem since 2015. It provided a compelling alternative to `cabal-install` which has helped push Haskell forward in many ways. However, of late development on Stack has significantly slowed, while `cabal-install` is rapidly improving. It may be time to consider unifying again under `cabal-install`, thanking Stack for the great good it has done.

## Stack Rocks

Haskell Stack (and Stackage) has made a number of key contributions to the Haskell ecosystem.

* It provided a dead simple way to get started with Haskell.
* It has nice and predictable UX.
* It (with [Hpack](https://hackage.haskell.org/package/hpack)) provided really simple ways to configure Haskell projects, avoiding friction like adding modules to the cabal file.
* It provided stability for industrial usage of Haskell.

I personally continue to heavily depend on Stack.

### Stackage Rocks!!!

In my opinion the killer feature of Stack is Stackage support. Stackage provides a number of benefits.

* Much simpler to find a set of packages that work together. You don't see new versions of packages until they actually work with the wider ecosystem of packages.
* More safety as a Stackage snapshot has run tests of packages to be sure they work with their dependencies. `cabal-install` will only ensure they build with their dependencies.
* Pushes package maintainers to update when their dependencies are updated, which is generally useful for the ecosystem.

## Stack development is slowing

I am not affiliated with Stack in anyway, but as an outside observer development on Stack has [slowed in the past 2 or so years](https://github.com/commercialhaskell/stack/graphs/contributors). With Michael Snoyman (core Stack contributor) [having twins](https://www.snoyman.com/blog/babies-oss-maintenance/) it has pretty much stopped entirely. He further states that he [no longer intends to work on new features for Stack](https://www.snoyman.com/blog/babies-oss-maintenance/#special-call-out-stack).

One clear impact of this slowdown is that Stack still depends on [Cabal (the library)](https://hackage.haskell.org/package/Cabal) 3.2.1.0 released on October 2020. It becomes more questionable if the benefits of Stack are worth it if you lose out on improvements to Cabal. Addmittidely I don't know much about how Stack works under the hood, so I don't know the impact of this is.

As good as Stack as been, without active maintenance it is not a good long term bet.

Thankfully, while Stack development is slowing, Stackage has continued to nicely tick along. It is still excellent and healthily maintained.

## Cabal is rapidly improving

Meanwhile, the [cabal project is going swimmingly](https://github.com/haskell/cabal/graphs/contributors). I won't really claim to understand why, but I think some credit should go to the [Haskell Foundation](https://haskell.foundation/).

As an outsider I observe:

* Lots of active work, including paying down tech debt and new features.
* Much more welcoming attitude towards newcomers.
* Much more openness to
