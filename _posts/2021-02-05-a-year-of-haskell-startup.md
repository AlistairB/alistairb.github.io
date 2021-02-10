---
layout: post
title: "Reflections On Creating A Haskell Startup"
tags: [haskell, gcp]
comments: true
---

Almost exactly one year ago I quit my job to attempt to create a Haskell startup as a solo developer. I had about 20 ideas, but eventually settled on the idea of dependency project health tracking with [Deadpendency](https://deadpendency.com/).

Disclaimer: This blog post contains a bunch of memes. They are trying to be humourous, not accurrate 😉.

## Why Use Haskell?

Since about 2016 I developed a strong ~~obsession~~ love of Haskell. Prior to learning Haskell, I was an experienced OO style developer but I didn't really know how to keep improving my raw programming ability. Haskell introduced me to the world of FP which has an almost infinite depth of concepts to learn that can be leveraged to improve code quality and application architecture.

<img class="center-image" width="400" src="https://i.imgflip.com/4x9eeq.jpg" alt="I should learn functional programming meme"/>

Haskell is challenging to learn, but extremely fun to write. For my own learning and pleasure, if my startup succeeds, I want to be doing Haskell.

Additionally, I think Haskell is the best general purpose programming language (that you can use in production). In particular, Haskell excells at writing 'boring' business applications which is typically what I work on. ['Why Haskell For Production'](https://www.foxhound.systems/blog/why-haskell-for-production/) goes into more detail on the benefits Haskell offers.

<img class="center-image" width="400" src="https://i.imgflip.com/4x9fwz.jpg" alt="Haskell is the best change my mind meme"/>

## The Setup Phase

Probably the most challenging part was building out a skeleton architecture to hang my code on. I decided to go with, even within Haskell, fairly advanced libraries of [`servant`](https://docs.servant.dev/en/stable/) and [`fused-effects`](https://hackage.haskell.org/package/fused-effects).

I spent a fair amount of time banging my head against a wall trying to get these libraries to work nicely together. This was a primarily from a lack of Haskell ability on my part. I had prepared as best I could, but Haskell is deep and I needed to learn more to work day to day with it. I was lucky enough to eventually find [an example](https://github.com/mitchellwrosen/hspolls) that marries these two libraries together, which was a life saver. I'm sure I would have gotten there eventually, but I was in a bit over my head at that point.

<img class="center-image" width="400" src="https://i.imgflip.com/4x9j14.jpg" alt="Haskell with servant fused-effects is hard meme"/>

Haskell is awesome, but like most languages there is cruft and legacy to be avoided. Haskell has a standard library known as [`base`](https://hackage.haskell.org/package/base) which unfortunately has a fair amount of unsafe or unperformant functions included. As such I went with an alternative standard library [`relude`](https://hackage.haskell.org/package/relude) that builds on and improves `base`. On top of this, there are many core libraries that are not part of the standard library I wanted to use and have nice patterns around.

Additionally, I was [deploying to google cloud](/haskell-on-google-cloud-is-great) and so needed to figure out good patterns for that integration from Haskell.

This setup effort was quite challenging, yet it only took about 2 weeks to have a good foundation of code to start building my business logic upon.

## Building It Out

This is when it started to get really fun. I had my core patterns set out and I could focus on building a pipeline. The day in day out of writing out my logic as small pure functions that I composed together was very nice.

Haskell has such impressive auto-magic code generation techniques that you spend much more time focused on the interesting logic of your application not boilerplate.

```haskell
data HappinessLevel =
    Miserable
  | Sad
  | Average
  | Happy
  | SuperFantastic
  deriving (Show, Eq, Ord, Bounded, Enum, ToJSON, FromJSON) -- magic code generation

-- ok not really magic, think 'convention over configuration'
-- where you can have generated sane defaults, or customise if you like
```

And personally I think Haskell is quite beautiful to read and write. #notbiased

### Parsing Libraries

A lot of the logic of Deadpendency is parsing. Either parsing dependency files or parsing various API responses. Haskell has many excellent parsing libraries, most notably [`aeson`](https://hackage.haskell.org/package/aeson) for JSON.

Why is this nice in Haskell? The 'monad' abstraction is excellent for dealing with code with a lot of failure conditions (ie. parsing) and avoids 'pyramid of doom' type code. Haskell worked out really well in this key area.

<img class="center-image" width="400" src="/images/hadouken.jpeg" alt="Pyramid of doom meme"/>

### Testing

Another strong positive for writing Deadpendency was testing. Haskell has a lesser known style of testing libraries that do 'property based testing' (PBT).

PBT allows you to write value generators for your data types, which you use to generate 100s or 1000s of test cases. Then, you run these generated values against some function and check that certain properties hold.

For example, part of the Deadpendency logic is generating an HTML report at the end. I had some `toHtml :: Report -> HTML` function that I wanted to test. So I wrote a `fromHtml :: HTML -> Report` function where it goes the other way (ok writing that was pretty painful). Then my PBT test will generate 100s of `Report` values and check that `report == fromHtml (toHtml report)` (this is known as 'roundtrip testing'). With this single test I was able to find many edge case bugs with my HTML report generation logic.

<img class="center-image" width="400" src="https://i.imgflip.com/4x9tqj.jpg" alt="Haskell with servant fused-effects is hard meme"/>

PBT exists in some other languages, but it originated (I believe?) in Haskell so the libraries are excellent.

### Not Actively Maintained Libraries

A big challenge of working with Haskell was the lack of well maintained libraries. Ironically, of the 75 (!) packages I depend upon 19 are flagged by Deadpendency as unhealthy. This means I often don't have the luxury of asking library maintainers to fix bugs. Even if I PR a fix, often that PR will be ignored for months.

This I think is the reality of using a niche language like Haskell. To be clear, I do not think library developers owe me anything, but it is nonetheless a downside when compared to more popular languages.

<img class="center-image" width="400" src="https://i.imgflip.com/4x9xjq.jpg" alt="Haskell not actively maintained meme"/>

Thankfully Haskell build tools have good support for loading a package from git. This means you can PR some bug fix or feature and immediately use your fork to work around the problem.

### Compile Times.. Were Fine

I thought I'd call this out as it is a common complaint I see around Haskell. I followed some [good advice](https://www.parsonsmatt.org/2019/11/27/keeping_compilation_fast.html) which kept compilation fast (aside from [one interesting edge case I resolved](https://twitter.com/AlistairBuzz/status/1253507016242294784)). So what are the numbers?

#### Build dependencies from scratch

Time: 13m41s

This is compiling all application dependencies, which needs to be done before you can compile your application code. Rebuilding all from scratch rarely happens as both my dev machines and CI will cache and only rebuild what has changed.

You do sometimes update a very core package which triggers a lot of dependent packages to recompile which can take a while. Although, I usually do dependency updates at the start of the day while I'm sipping my coffee, so usually don't notice.

#### Build app from scratch (including tests)

Time: 1m7s

Likewise, due to caching a full recompilation rarely happens. As such, most code edits do not trigger many modules to be recompiled and it is fast.

Additionally, Haskell has nice 'continuous compilation' tools that fire on save. Usually by the time I actually look at my terminal compilation is already done.

#### Build Executable

with [full optimisations](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/using-optimisation.html) (-02)

Time: 3m30s

This typically runs in CI. It runs in parallel with a host of other checks such as running my tests. Due to this, the time doesn't really impact the build + deploy time much.

<img class="center-image" width="400" src="https://imgs.xkcd.com/comics/compiling.png" alt="Haskell code compiling meme"/>

### Refactoring Pain

Deadpendency is relatively simple in what it does, but there is a lot of hidden complexity to the problem. Which is to say, it is like 99% of applications 😉. When developing it I was constantly realising I had modelled things a bit too simplistically and would need to refactor.

Haskell has a couple of [good](https://hackage.haskell.org/package/apply-refact) [tools](https://hackage.haskell.org/package/retrie) that do refactoring, but they seem difficult to learn and apparently don't do the core refactorings that I want. Namely, move/rename module and rename function/variable name. As such I did it all manually with text search replace, or just change something and fix all the new compiler errors.

The dream of course is nice refactoring built into an IDE.

<img class="center-image" width="400" src="https://i.redd.it/dbdshzzflgd31.jpg" alt="Haskell had an IDE meme"/>

(Stolen from [reddit](https://www.reddit.com/r/ProgrammerHumor/comments/cjtbfj/society_if_haskell_has_ide/))

Having said that, it should be noted that Haskell does have an excellent IDE now in the form of [Haskell Language Server](https://github.com/haskell/haskell-language-server) (HLS). The momentum around the project is insane and I applaud the developers. One fixed pain point from HLS is it beautifully does auto imports now, which used to greatly contribute to the friction of working with Haskell.

### Waiting For New GHC Versions To Be Usable

This is mostly me complaining for the sake of it, but as someone pretty obsessed with new shiny versions of things and obessed with Haskell, waiting for new GHC (GHC is the Haskell compiler) versions to be usable has been painful. There is a long tail of libraries and platforms that need to be updated before I can use a new GHC versions. Sometimes these updates can drag a lot.

For example GHC 9 was just released, but I still haven't been able to upgrade to the GHC 8.10 yet which was first released in March 2020.

<img class="center-image" width="400" src="https://i.imgflip.com/4xebid.jpg" alt="GHC releases meme"/>

## Launching

So after about 8 months of work I was ready to start getting users. I slowly soft launched promoting it in a few small channels. How did my Haskell fair in prod?

### Very Few Logic Bugs

My core Haskell had very few logic bugs. This is because Haskell is very safe by default and I had opted into strict types that help catch edge cases.

For example, I was using a lot of [`NonEmpty`](https://hackage.haskell.org/package/base-4.14.1.0/docs/Data-List-NonEmpty.html) lists which the compiler will gaurentee is not empty. To use them you must specifiy how to handle the empty case. ie. what do I do if Deadpendency can't find any dependencies to check?

And of course, I had many tests as the compiler cannot find all the bugs.. yet..

<img class="center-image" width="400" src="https://i.imgflip.com/4xevk6.jpg" alt="GHC releases meme"/>

### Too Strict Parsing

Haskell and its libraries have a philosophy of being explicit by default. In terms of parsing libraries, you tend to specify exactly the shape that you want to parse. I haven't done much parsing in non-FP lanugages, but I assume they are more permissive by default.

This bit me, where it turned out that almost every assumption that I made about the various package registry APIs turned out to be incorrect. Especially [NPM](https://docs.npmjs.com/cli/v6/using-npm/registry).. For example, for an [NPM package](https://registry.npmjs.org/nomnom) you can get the latest version by getting `dist-tags -> latest`. What about a package that has no release? Well you get `dist-tags: {}`, except that it turns out that some packages don't even have `dist-tags` at all.

I don't blame Haskell for this, I think explicit by default is the way to go. However, I quickly realised I would need to gracefully handle parse failures like these as there was so much variance in structure. It is also worth noting that this explicitness is [not a strike against statically typed lanugages](https://lexi-lambda.github.io/blog/2020/01/19/no-dynamic-type-systems-are-not-inherently-more-open/).

<img class="center-image" width="400" src="https://i.imgflip.com/4xeytm.jpg" alt="Registry API are inconsistent meme"/>

### Memory issues

The other big pain point was memory usage. I was using [Google Cloud Run](https://cloud.google.com/run) which is sort of like AWS Lambda where you can specify how much memory you need. To keep things cheap and to learn if my app needed a lot of memory, I went with the minimum of 256MiB. This amount seemed fine until I went to prod and Deadpendency was used on random repos.

The core issue was.. NPM again had some rare packages that are huge, the [worst case](https://registry.npmjs.org/rendition) being 84MB uncompressed. It turns out that [`aeson`](https://hackage.haskell.org/package/aeson) will convert all the JSON into an [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree) first, before it then attempts to parse it to your type. This is fine when the JSON is small, or you are loading most of the contents of the JSON. In my case the AST apparently took about 20x the ammount of memory of the raw JSON, when I only needed a tiny amount of the data.

Eventually I realised I should use a [library designed to parse in constant memory](https://hackage.haskell.org/package/json-stream) and all was well. I can parse the 84MB file and only see 84MB used. I could take this even further and stream the response, but for now it is working fine as is.

<img class="center-image" width="400" src="https://i.imgflip.com/4xf4i0.jpg" alt="Registry API are inconsistent meme"/>

#### Any Memory Issues Due To Laziness?

As a lazy language, Haskell is known to have memory issues due to unevaluated expressions accumulating in unexpected ways. Thankfully I have avoided these issues so far.

I did this by making my types strict by default with the [`StrictData`](https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html) extension. Additionally, Haskell has lazy linked lists as the default list type. Instead I used a [strict list](https://hackage.haskell.org/package/vector) type.

## Overall

In a bit over a year I was able to build Deadpendency supporting 11 languages (and set up a bunch of cloud junk around it 😉). At this point I think it is actually pretty stable. I consider the project a big success.

A huge part of this has been due to Haskell and its excellent ecosystem. Of course, I spent 4 years dabbling as I learnt it, but once skilled up it is super effective. I do believe anyone developer can learn Haskell and even learn it quickly in the right environment.

What's next? I am working on promoting [Deadpendency](https://deadpendency.com/) and I hope to get more users. Have I spent too much time geeking out on Haskell and not enough time thinking about the idea? I guess we will see 😊. Either way, I have had enough fun and learnt enough that I will consider the experience worth it.

<img class="center-image" width="400" src="https://i.imgflip.com/4xf7ou.jpg" alt="Hide the pain Haskell meme"/>
