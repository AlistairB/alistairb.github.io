---
layout: post
title: "The Benefits of Learning Haskell"
tags: [haskell]
comments: true
---

As developers, there is so much we can learn to improve our skills, ranging from deep theory, to small practical tidbits of information. It can be overwhelming at times and we are forced to pick what we want to learn deeply and what we just want to gloss over. You can't learn it all!

My belief is that in terms of _long term_ improvement, learning [Haskell](https://www.haskell.org/){:target="_blank" rel="noopener"} is one of the best ways to up your raw programming ability.

<img class="center-image" width="400" src="https://i.imgflip.com/537cg3.jpg" alt="Haskell meme"/>

## Why Learn any New Language

Learning a new programming language is a great way to improve as a programmer. Each language has its own unique take on things and will have some great ideas.

I ended up working in Ruby for a few years when at the time I was much more interested in Scala. However, I saw many cool things about Ruby and it ended up teaching me a lot. For example, I had not done strict [TDD](https://en.wikipedia.org/wiki/Test-driven_development){:target="_blank" rel="noopener"} before which is very popular in Ruby. I learnt the pros and cons of this approach and now I can apply it well when needed.

Usually these new learnings are not strictly tied to the new language. You can take them and use them in many different programming languages.

<img class="center-image" width="400" src="https://i.imgflip.com/537dq5.jpg" alt="Haskell meme"/>

## Ok but Why Learn Haskell

When learning a new programming language for improvement, it is usually best to choose some language that is very different from your known programming languages. For example, if you know Ruby you probably wouldn't get a huge amount out of learning Python, as they are quite similar.

Haskell is very different from mainstream popular programming languages. Most developers work in these mainstream languages and so Haskell has a lot to teach them.

<img class="center-image" width="400" src="https://i.imgflip.com/53jqnd.jpg" alt="Haskell meme"/>

How is it different?

### Functional Programming

Haskell is a purely functional programming (FP) language. What is FP? I see it as programming enhanced by ideas and constructs from math.

* Haskell uses FP from the ground up. The whole ecosystem builds on these concepts, so you can't avoid them.
* It is pure. It does not have unconstrained side effects, so you need to learn the FP way of handling them.
* Similarly, it is immutable, so you must learn how FP manages this.
* It's implementation closely mirrors the math behind FP, so you are learning something close to the real underlying concepts.

Haskell actually inspired a lot of the FP implementations in more popular languages. Knowing FP in Haskell will make these (typically simpler) implementations easy to understand.

For example, [`Aggregate`](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.aggregate?view=net-5.0){:target="_blank" rel="noopener"} in [LINQ](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/), is really just [`foldl`](https://hackage.haskell.org/package/base-4.15.0.0/docs/Data-List.html#v:foldl){:target="_blank" rel="noopener"} from Haskell.

<img class="center-image" width="400" src="https://i.imgflip.com/537fk6.jpg" alt="Haskell meme"/>

### Static Type Checking

Haskell builds on [type theory](https://en.wikipedia.org/wiki/Type_theory){:target="_blank" rel="noopener"} and has an advanced type system. This includes 'type-level' programming, or code that is run by the compiler at compile time, not at run time.

Like how many mainstream languages are getting FP features, they are also getting (more sophisticated) [type](https://blog.appsignal.com/2021/01/27/rbs-the-new-ruby-3-typing-language-in-action.html){:target="_blank" rel="noopener"} [checkers](http://mypy-lang.org){:target="_blank" rel="noopener"}. Again, if you learn Haskell these (likely simpler) type systems should be easy to use.

For example, Kotlin has [sealed classes](https://kotlinlang.org/docs/sealed-classes.html){:target="_blank" rel="noopener"} which [support matching](https://kotlinlang.org/docs/sealed-classes.html#sealed-classes-and-when-expression){:target="_blank" rel="noopener"}. It can be hard to see how these might be useful and I imagine many OO background developers would ignore them.

However, these a really just a form of [sum types and pattern matching](https://www.schoolofhaskell.com/school/to-infinity-and-beyond/pick-of-the-week/sum-types){:target="_blank" rel="noopener"}, which are incredibly useful. You can learn the clean implementation in Haskell then apply this knowledge in other languages.

<img class="center-image" width="400" src="https://i.imgflip.com/54rcfq.jpg" alt="Haskell meme"/>

## You Can Use Haskell in Prod

There are even fancier languages than Haskell. Not more advanced in all ways, but that take a specific idea and go much further than Haskell. Notably, [Idris](https://www.idris-lang.org){:target="_blank" rel="noopener"} and [Agda](https://agda.readthedocs.io){:target="_blank" rel="noopener"}. However, you can't easily use these languages in prod. Either because they are too immature, or they aren't really designed for it.

Haskell on the other hand is a sweet spot where it is highly advanced, but you can (and many companies do) [use it in prod](/reflections-on-haskell-for-startup){:target="_blank" rel="noopener"}.

This means:

* You can learn Haskell by building real things.
* One day you might be able to just use Haskell directly in prod, not do FP in OO languages. 😉

<img class="center-image" width="400" src="https://i.imgflip.com/53ofxl.jpg" alt="Haskell meme"/>

## Learning Haskell

Haskell is awesome, but it is a niche language. You can expect some challenge in getting things set up and of course learning the language. Some describe it as learning to program again from scratch.

<img class="center-image" width="400" src="https://i.imgflip.com/537i18.jpg" alt="Haskell meme"/>

The key to making the journey manageable is to find some community to support you. The [FP slack](https://fpchat-invite.herokuapp.com/){:target="_blank" rel="noopener"} and [FP discord](https://top.gg/servers/280033776820813825){:target="_blank" rel="noopener"} are both excellent choices.

I like [this guide](https://github.com/bitemyapp/learnhaskell){:target="_blank" rel="noopener"} (although I would use the official [fp-course](https://github.com/system-f/fp-course){:target="_blank" rel="noopener"} rather than the forked one).

Above all, I think learning Haskell should be fun. Try and enjoy the ride. 😊

<img class="center-image" width="400" src="https://i.imgflip.com/53oi49.jpg" alt="Haskell meme"/>
