---
layout: post
title: "Haskell On Google Cloud Is Great"
tags: [haskell, gcp]
comments: true
---

My first experience of developing and deploying to the cloud was in 2016. I was a fresh faced.. middle aged developer learning about a whole host of technologies such as Docker and AWS. In fact my first task was, with help, to dockerise a legacy ruby application and deploy it to AWS EC2. It was a grueling 13 week effort, but we got there in the end.

Fast foward 3.5 years, I was really comfortable with AWS and liked it a lot. I knew how to wire together various services to solve a problem. Meanwhile, I had also become quite obssessed with Haskell and had been improving steadily.

I then had an idea of a tool around dependency management ([Deadpendency](https://deadpendency.com)) and quit my job to attempt to build it, in Haskell! A key choice to make was how to deploy and host it.

## AWS or... ?

The natural choice for me was AWS. Indeed, myself and a collegue had in fact managed to get [Haskell running in prod](https://www.rea-group.com/blog/a-haskell-in-prod-journey/) on AWS at my company for a trivial service. However, I have always been in awe of Google products and.. addmittedly a bit of a fanboy so I was quite curious about Google Cloud Platform (GCP).

And.. I did end up going with GCP and I think it worked out really well. So well in fact, I think GCP is a better default choice for Haskell than AWS. Why?

## Google Cloud Run

[Cloud run](https://cloud.google.com/run) is the killer feature for a niche language like Haskell.
