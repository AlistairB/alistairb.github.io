---
layout: post
title: "Haskell On Google Cloud Is Great"
tags: [haskell, gcp]
comments: true
---

About a year ago I quit my job to attempt my startup idea ([Deadpendency](https://deadpendency.com)), writing it in Haskell. One key decision to make was what cloud provider would I go with.

Like many developers, I have a lot of professional experience developing against Amazon Web Services (AWS). Haskell itself seems to reflect industry in that AWS is the most popular choice. In fact, myself and a colleague had already managed to get [Haskell running in prod on AWS](https://www.rea-group.com/blog/a-haskell-in-prod-journey/).

However, I've always been a bit in awe of google products and wanted to try out Google Cloud Platform (GCP). I decided this would be a good opportunity to do so.

tl;dr - Haskell on GCP is great. GCP vs AWS has various trade offs, however I think there a few reasons why GCP is great for Haskell.

First lets look at using Haskell on AWS.

## AWS + Haskell

### AWS Haskell Library Support

AWS has the excellent `[amazonka](https://hackage.haskell.org/package/amazonka)` library developed by Brendan Hay. This is actually a large set of libraries that support the many many AWS services. The bulk of the libraries are generated from AWS service definitions and thus can easily be kept up to date.

### AWS Lambda

[AWS Lambda](https://aws.amazon.com/lambda/) allows you to run a function in the cloud without having to worry about how it is hosted or how it integrates with other AWS services. With a bit of config you can configure a whole host of integrations or scaling options and AWS will make it happen.

Lambda is somewhat contraversial amoung developers in my experience. It doesn't always live up to its motto of 'Run code without thinking about servers or clusters.'. Nonetheless, I think it does have a good niche and can be very useful.

The big drawback for Haskell is it only supports a limited number of languages, Haskell not included. It does support [custom runtimes](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-custom.html) and there [are](https://hackage.haskell.org/package/serverless-haskell) [tools](https://theam.github.io/aws-lambda-haskell-runtime/) for using Haskell on Lambda.

Addmittedly, I have not actually used these Haskell on Lambda solutions, but I believe they take more manual effort to set up and configure. This to my mind takes away a lot of simplicity benefits of using Lambda.

## GCP + Haskell

### GCP Haskell Library Support

Likewise, GCP has `[gogol](https://hackage.haskell.org/package/gogol)` library developed by Brendan Hay. `gogol` has the disadvantage that it is a bit less popular (like GCP) than `amazonka` and is thus a bit less actively maintained. Still, there is nothing preventing you from regenerating the libraries and using them as required. I did this to support a new google service.

Overall GCP vs AWS here is mostly moot.

### Google Cloud Run

The equivilant of AWS Lambda on GCP is [cloud functions](https://cloud.google.com/functions). Cloud functions has much of the same limitations as Lambda and actually has no custom runtime concept. Haskell cannot run on cloud functions.

However, GCP has another service [cloud run](https://cloud.google.com/run) which could be thought of as 'AWS Lambda for Docker' and is a sweet spot for niche language like Haskell. You use it by providing a docker container that listens on a port for HTTP requests. You can integrate it with other GCP services to automatically make those requests, then your API just needs to decode and handle them.

For example in my usage I have a pipeline of cloud run services connected with [pub sub](https://cloud.google.com/pubsub) (similar to AWS kinesis). I do not need any code to have new messages appear as HTTP requests to cloud run.

Additionally, these can simply be HTTP APIs, without using any of the integration glue that GCP offers. It includes a bunch of features around auto scaling as well.

#### Kubernetes?

Kubernetes sounds cool, but I think it is a complex solution for a complex problem. If you need this complexity Haskell will work nicely, as Haskell on any Docker based solution works great. However, if you don't need the power of kubernetes I think cloud run is the better, simpler choice.

### Comprehensive Platform (with vendor lock in)

This one is based on my impressions and I find it hard to include clear examples. Nonetheless, I think that AWS has more of a strategy of supporting many 3rd party services to augment their simplistic core offerings. GCP has some 3rd party services, but more so I think they offer comprehensive first party products.

Why does this matter to Haskell?

Haskell doesn't always have a great story around libraries to integrate with various vendor products and services. AWS pushes you more to use these due to limitations of their offerings. When using first party AWS or GCP products you don't have this problem thanks to `gogol` and `amazonka`. Thus I think the comprehensive platform they offer is quite valuable to Haskell apps as you know you have a great library story.

### GCP is cheap

Haskell people are mostly smaller shops where cost is factor.

## GCP downsides

### Deployment as code

### It is less popular
