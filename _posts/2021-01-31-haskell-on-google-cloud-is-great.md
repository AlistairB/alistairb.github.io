---
layout: post
title: "Haskell On Google Cloud Is Great"
tags: [haskell, gcp]
comments: true
---

About a year ago I quit my job to attempt my startup idea ([Deadpendency](https://deadpendency.com)), writing it in Haskell. One key decision to make was what cloud provider would I go with.

Like many developers, I have a lot of professional experience developing against Amazon Web Services (AWS). The Haskell community seems to reflect wider industry in that AWS is the most popular choice. In fact, myself and a colleague had already managed to get [Haskell running in prod on AWS](https://www.rea-group.com/blog/a-haskell-in-prod-journey/).

However, I've always been in awe of google products and wanted to try out Google Cloud Platform (GCP). I decided this would be a good opportunity to do so.

tl;dr - Haskell on GCP is great. GCP vs AWS has various trade offs, however I think there a few reasons why GCP is great for Haskell.

First lets look at using Haskell on AWS.

## AWS + Haskell

### AWS Haskell Library Support

AWS has the excellent [`amazonka`](https://hackage.haskell.org/package/amazonka) library developed by Brendan Hay. This is actually a large set of libraries that support the many many AWS services. The bulk of the libraries are generated from AWS service definitions and thus can easily be kept up to date.

### AWS Lambda

[AWS Lambda](https://aws.amazon.com/lambda/) allows you to run a function in the cloud without having to worry about how it is hosted or how it integrates with other AWS services. With a bit of config you can configure a whole host of integrations or scaling options and AWS will make it happen.

Lambda is somewhat contraversial amoung developers in my experience. It doesn't always live up to its motto of `Run code without thinking about servers or clusters`. Nonetheless, I think it does have a good niche and can be very useful.

Unfortunately Haskell is not directly supported by Lambda. It does support [custom runtimes](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-custom.html) and there [are](https://hackage.haskell.org/package/serverless-haskell) [tools](https://theam.github.io/aws-lambda-haskell-runtime/) for using Haskell on Lambda.

Addmittedly, I have not actually used these Haskell on Lambda solutions, but I believe they take more manual effort to set up and configure. This to my mind takes away a lot of simplicity benefits of using Lambda.

## GCP + Haskell

### GCP Haskell Library Support

Likewise, GCP has [`gogol`](https://hackage.haskell.org/package/gogol) library developed by Brendan Hay. `gogol` has the disadvantage that it is a bit less popular (like GCP) than `amazonka` and is thus a bit less actively maintained. Still, there is nothing preventing you from regenerating the libraries and using them as required. I did this to support a new google service.

Overall GCP vs AWS here is mostly moot.

### Google Cloud Run

The equivilant of AWS Lambda on GCP is [cloud functions](https://cloud.google.com/functions). Cloud functions has much of the same limitations as Lambda and additionally has no custom runtime concept. Haskell cannot run on cloud functions.

However, GCP has another service [cloud run](https://cloud.google.com/run) which could be thought of as `AWS Lambda for Docker` and is a sweet spot for a niche language like Haskell. You use it by providing a docker container that listens on a port for HTTP requests. You can integrate it with other GCP services to automatically make those requests, then your API just needs to decode (with help from `gogol`) and handle them.

For example in my usage I have a pipeline of cloud run services connected with [pub sub](https://cloud.google.com/pubsub) (similar to AWS kinesis). I do not need any code to have new messages appear as HTTP requests to cloud run.

Additionally, your cloud run services can simply be HTTP APIs, without using any of the integration glue that GCP offers. It includes a bunch of features around auto scaling as well.

#### What About Kubernetes?

Kubernetes sounds cool, but I think it is a complex solution for a complex problem. If you need this complexity Haskell will work nicely, as Haskell on any Docker based solution works great. However, if you don't need the power of kubernetes I think cloud run is the better, simpler choice.

### Comprehensive Platform (with vendor lock in)

This one is based on my impressions and I find it hard to include clear examples. Take this with a grain of salt ;)

I think that AWS has more of a strategy of supporting many (many) products and services, both 1st and 3rd party. This often leaves the 1st party offerings fairly limited and customers will opt for AWS managed 3rd party options.

GCP has some 3rd party products, but more so I think they offer fewer, but more comprehensive 1st party products and services.

Why does this matter to Haskell?

Haskell doesn't always have a great story around libraries to integrate with various vendor products and services. AWS pushes you more to use these due to limitations of their offerings. When using first party AWS or GCP products you don't have this problem thanks to `gogol` and `amazonka`. Thus I think the comprehensive GCP platform is quite valuable to Haskell apps as you know you have a great library story.

### GCP is cheap

To my mind most Haskell shops are small to medium sized. At this size I think the cost of cloud resources is more of a factor. GCP is cheaper than AWS, particularly for small shops or startups (in my experience).

One reason for this is most of their services include a free tier that you need to exceed before having to pay anything. Cloud run for example has [2 million free requests per month](https://cloud.google.com/run/pricing) (and other free tier limits you may hit).

### Deadpendency Architecture

I am working on chopping out a nice skeleton architecture of how I deploy and use cloud run for [Deadpendency](https://deadpendency.com). I hope this will be valuable to others and lower the hurdle of using Haskell on GCP and cloud run. Coming soon :tm:.

## GCP downsides

So perhaps at this point you're really keen to start using Haskell on GCP. There are a couple of downsides when compared to AWS it is good to be aware of.

### Deployment as code

To me the killer feature of AWS is [Cloud Formation](https://aws.amazon.com/cloudformation). GCP has an equivelant in [Cloud Deployment Manager](https://cloud.google.com/deployment-manager). When I tried deployment manager a year ago it was greatly inferior to cloud formation which sucks.

This isn't the end of the world though. It just means [terraform](https://www.terraform.io/) is essential on GCP. Terraform has a learning curve and is another abstration layer to learn, but does also bring some benefits with the complexity. Additionally, Google and Terraform themselves maintain the [terraform google 'provider'](https://registry.terraform.io/providers/hashicorp/google/latest) so it is quite high quality and up to date.

### It is less popular

[GCP is growing faster than AWS](https://www.parkmycloud.com/blog/aws-vs-azure-vs-google-cloud-market-share/) in terms of percentage year on year revenue growth. However, AWS is currently 32% of the market, while GCP is only 7%. As Haskellers we know well the costs of using niche languages and products. It is manageable, but you can expect it harder to find answers to your random error message etc.

One nice thing is there are some [good free channels](https://cloud.google.com/community#home-support) to get help from GCP employees.

## Conclusion

Haskell runs great in Docker and has great library support for AWS and GCP. As such, Haskell can work well on both of them.

However, GCP has a few advantages, in particular cloud run is excellent for hosting APIs or gluing together pipelines with Haskell. I would strongly recommend considering it if you do Haskell, rather than just automatcially using AWS :)
