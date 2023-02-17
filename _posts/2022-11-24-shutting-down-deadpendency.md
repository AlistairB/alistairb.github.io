---
layout: post
title: "Shutting Down Deadpendency"
tags: []
tags: [Deadpendency]
comments: true
---

In Feb in 2020 I quit my full time job to create my idea for a startup, [Deadpendency](https://deadpendency.com/){:target="_blank" rel="noopener"}. There were some [trials and tribulations](/my-experience-creating-a-one-person-startup), but ultimately I was successful and I built the thing.

Unfortunately, I have decided to pull the plug and I thought I would write up my reasoning and some reflections.

## What Is Deadpendency?

The idea is there is a bunch of due diligence developers perform when selecting what packages to depend on. This is a manual and one time effort, however what is an active project, may become abandoned or deprecated over time. Deadpendency is a continual check that alerts developers when their dependencies become 'dead' projects.

In other words, it gives you something like:


<img class="center-image" width="800" src="/images/deadpendency-example.png" alt="Deadpendency Example"/>

Developers can respond to 'dead' dependencies and switch to actively maintained ones (or ignore the dependency if they don't have other good options).

## Why Did I Build It?

I built it because:

* I was eager to try a startup and learn new technology.
* It was a small project I could build by myself.
* It was a new concept, thus no direct competitors.
* I er thought it was a cool idea.

To be honest I did not deeply evaluate how useful it would be to a team of developers. I did think most teams would have bigger fish to fry than unmaintained dependencies, but surely some teams would benefit.

In particular, I thought teams that were highly effective and had ticked off a lot of other boxes, like automated dependency updates, might appreciate it as a bit of icing on their development cake.

## Did Anyone Use it?

Yup, it is currently sitting at 350 installs which is not too shabby at all. It generally has a steady stream of free installs for public repositories. Every now and again it cycles to the [home page of GitHub marketplace](https://github.com/marketplace/) and it gets a lot of installs and usually some paid ones!

There are also certain passionate users that will [give me feedback](https://github.com/deadpendency/deadpendency/issues) and are clearly quite enthusiastic about the tool.

Unfortunately, of the roughly 30 paid installs that have occurred, all but 1 have cancelled in the 2 week free trial period. There does not appear to be any way to provide an uninstall link when a user uninstalls an app, so I have no way to solicit feedback about why they uninstalled. I can only infer.

## A Fresh Analysis

I now have the information that while individual developers will use and get enthusiastic about the free version, this does not translate into them going to their companies and getting them to pay for it. Or if they do, the tool is not actually that useful for them and they uninstall it.

The question becomes why not? Dependending on a dead / deprecated dependency is surely not a good thing, so why will people not pay to know about this?

### The App Is Feature Complete

Firstly I would largely rule out that Deadpendency lacks certain features or polish. It is solid and works. There are only [so many ways](https://deadpendency.com/docs/rules) you can assess a software dependency for aliveness and Deadpendency does all the key ones.

The [Deadpendency report](https://github.com/deadpendency/deadpendency-example/pull/1/checks?check_run_id=3386648844) is a bit bare bones, as it lives on GitHub as a 'Check Run Report', but it includes all the key information. I could host it externally for something more polished, but it would be functionally equivalent.

### Development Teams Are Time Poor

I think the key reason to understand why Deadpendency is not successful is to understand that all development teams are extremely time poor (unless they are being greatly mismanaged). Any time spent on something like assessing and potentially replacing 'dead' dependencies is time not spent on other work or improvement.

The idea that certain teams who are highly competent might use it as 'icing' or 'bells and whistles', having set up lots of other good automation is a false one. Any work needs to be highly justified and competes against other uplift of which there is an almost endless amount.

In short, all teams will have plenty of other competing hygiene or uplift that they can work on and 'dead' dependencies is not as effective as the other options.

### Automated Dependency Updates

To start by contrasting, what about automating dependency updates? Is that time well spent?

Yes, I strongly believe that automating dependency updates is time well spent.

* Generally being on the latest versions will provide the most secure, reliable and performant functionality.
* Dependency versions must be updated eventually, before various platform upgrades render them broken.
* Delaying dependency updates until being forced, leads to 'big bang' and more difficult upgrades that take more time and are more likely to break something. The most time efficient approach is to continually update dependencies.
* The problem is highly automatable with tools like [Dependabot](https://docs.github.com/en/code-security/dependabot).

And yet, I would hazard that most teams have not automated this. Most teams are either too time poor, or have other low hanging fruit options to perform uplift, that they will not get around to this.

### Deadpendency

Whilst I am proud of it and I think it is a cool idea, it is overall less time efficient when compared to automated dependency updates. When a dependency is flagged as 'dead', you still need to manually review the dependency, look for alternatives and perform time doing the switch. This is all relatively high effort.

There is a scenario which is efficient, which is when a dependency is flagged as deprecated in favor of some other dependency. This can include renames and it is quite easy and automatable to make this switch. Having said that, I think this is something that should be incorporated into Dependabot and the like.

Proactively switching away from dead dependencies is also a tradeoff. If you can delay that switch, the dependency may become active and healthy again. Or in the future a more compelling alternative may appear.

Instead, if dead dependencies are a problem, I think a better use of time is to write more tests. Tests can flag problems due to old dependencies as platform or other upgrades occur and then you know it is time to look to switch. Tests obviously have a lot of other benefits too.

## Reflections

The key takeaway for me is that development tools really need to help with the critical path of developing and maintaining applications. They can target niche problems, but these problems should be unavoidable for certain development teams. A tool that requires a time investment and is a 'nice to have', doesn't really make sense.

I spent a bit over a year working solely on Deadpendency and have been supporting it on the side since then. I certainly don't regret this time as I learnt a huge amount. However, it is not free to run and I don't think it has a profitable future.

I plan to make Deadpendency er dead on the 10th of December. Thanks to all that used it! 😊
