---
layout: post
title: "Haskell On Google Cloud Is Great"
tags: [haskell, gcp]
comments: true
---

About a year ago I quit my job to attempt my startup idea ([Deadpendency](https://deadpendency.com)), writing it in Haskell. One key decision to make was what cloud provider would I go with.

Like many developers, I have a bunch of professional experience developing against Amazon Web Services (AWS). The Haskell community seems to reflect wider industry in that AWS is by far the most popular choice. In fact, myself and a colleague had already managed to get [Haskell running in prod on AWS](https://www.rea-group.com/blog/a-haskell-in-prod-journey/) and it was great.

However, I've always been in awe of Google products and wanted to try out Google Cloud Platform (GCP). I decided this would be a good opportunity to do so.

tl;dr - Haskell on GCP is great. GCP vs AWS has various trade offs, however I think there are a few reasons why GCP is great for Haskell.

First let's look at using Haskell on AWS.

## AWS + Haskell

### AWS Haskell Library Support

AWS has the excellent [`amazonka`](https://hackage.haskell.org/package/amazonka) library developed by Brendan Hay. This is actually a large set of libraries that support the many many AWS services. The bulk of the libraries are generated from AWS service definitions and thus can easily be kept up to date.

### AWS Docker Options

Haskell works really well with Docker. You can build executables with [official Haskell images](https://hub.docker.com/_/haskell/). Then simply embed them in plain old Debain based containers. AWS has numerous Docker based options and they work great.

### AWS Lambda

[AWS Lambda](https://aws.amazon.com/lambda/) allows you to run a function in the cloud without having to worry about how it is hosted, or how it integrates with other AWS services. With a bit of config you can configure a whole host of integrations or scaling options and AWS will make it happen.

Lambda is somewhat controversial among developers in my experience. It doesn't always live up to its motto of `Run code without thinking about servers or clusters`. Nonetheless, I think it does have a good niche and can be very useful.

Unfortunately Haskell is not directly supported by Lambda. It does support [custom runtimes](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-custom.html) and there [are](https://hackage.haskell.org/package/serverless-haskell) [tools](https://theam.github.io/aws-lambda-haskell-runtime/) for using Haskell on Lambda.

Admittedly, I have not actually used these Haskell on Lambda solutions, but I believe they take more manual effort to set up and use. This to my mind takes away a lot of the simplicity benefits of using Lambda.

## GCP + Haskell

### GCP Haskell Library Support

Likewise, GCP has the [`gogol`](https://hackage.haskell.org/package/gogol) library developed by Brendan Hay. `gogol` has the disadvantage that it is a bit less popular (like GCP) than `amazonka` and is thus a bit less actively maintained. Still, there is nothing preventing you from regenerating the libraries and using them as required. I did this recently to gain support for a new google service.

Overall GCP vs AWS here is mostly moot.

### GCP Docker Options

GCP also has many good solutions with Docker and they work great with Haskell.

### Google Cloud Run

The equivalent of AWS Lambda on GCP is [cloud functions](https://cloud.google.com/functions). Cloud functions has much of the same limitations as Lambda and additionally has no custom runtime concept. Haskell cannot run on cloud functions, so a negative strike there.

However, GCP has another service, [cloud run](https://cloud.google.com/run), which could be thought of as `AWS Lambda for Docker` and is a sweet spot for a niche language like Haskell. You use it by providing a docker container that listens on a port for HTTP requests. You can integrate it with other GCP services to automatically make those requests, then your API just needs to decode (with help from `gogol`) and handle them.

For example in my usage I have a pipeline of cloud run services connected with [pub sub](https://cloud.google.com/pubsub) (similar to AWS kinesis). I do not need any code to have new messages appear as HTTP requests to cloud run.

Additionally, your cloud run services can simply be HTTP APIs, without using any of the integration glue that GCP offers. So it does some of what Lambda does while also being a simple solution to host standalone APIs.

Cloud run is great, especially for niche languages like Haskell. Unlike AWS Lambda Haskell does not become a second class citizen.

#### What About Kubernetes?

Kubernetes sounds cool, but I think it is a complex solution for a complex problem. If you need this complexity Haskell will work nicely, of course. However, I think for most use cases cloud run is the better, simpler choice.

### Comprehensive Platform (with vendor lock in)

This one is based on my impressions and I find it hard to include clear examples. Take this with a grain of salt 😉.

GCP more so than AWS seems to focus on fewer, more developed first party products. AWS on the other hand has more products, particularly more managed third party products. As such, on AWS you are probably going to supplement the AWS platform with these third party tools.

Why does this matter to Haskell?

While you are sticking to the core AWS / GCP platform, you know you have excellent library support with `amazonka` / `gogol`. When using third party product X, library support can be patchy. GCP helps you avoid this problem with more of a chance of sticking entirely to the GCP platform.

This comes with vendor lock in concerns, but beggars can't be choosers 😉.

### GCP Is Cheap

To my mind most Haskell shops are small to medium sized. At this size I think the cost of cloud resources is more of a factor. GCP is cheaper than AWS, particularly for small shops or startups (in my experience).

One reason for this is most of their services include a free tier that you need to exceed before having to pay anything. Cloud run for example, has [2 million free requests per month (and other free tier limits you may hit)](https://cloud.google.com/run/pricing).

### Deadpendency Architecture

I am working on chopping out a nice skeleton architecture of how I deploy and use GCP for [Deadpendency](https://deadpendency.com). I hope this will be valuable to others and lower the barrier of using Haskell on GCP and cloud run. Coming soon™️ 😅.

## GCP Downsides

So perhaps at this point you're really keen to start using Haskell on GCP. There are a couple of downsides to GCP when compared to AWS it is good to be aware of.

### Deployment As Code

To me the killer feature of AWS is [Cloud Formation](https://aws.amazon.com/cloudformation). GCP has an equivalent in [Cloud Deployment Manager](https://cloud.google.com/deployment-manager), however it is (or was 1 year ago when I tried it) greatly inferior to cloud formation.

This isn't the end of the world though. It just means [terraform](https://www.terraform.io/) is essential on GCP. Terraform has a learning curve and is another abstraction layer to learn, but does also bring some benefits with the complexity. Additionally, Google and Terraform themselves maintain the [terraform google 'provider'](https://registry.terraform.io/providers/hashicorp/google/latest) so it is quite high quality and up to date.

### It Is Less Popular

[GCP is growing faster than AWS](https://www.parkmycloud.com/blog/aws-vs-azure-vs-google-cloud-market-share/) in terms of percentage year on year revenue growth. However, [AWS is currently 32% of the market, while GCP is only 7%](https://www.parkmycloud.com/blog/aws-vs-azure-vs-google-cloud-market-share/). As Haskellers we know well the costs of using niche languages and products. It is manageable, but you can expect it harder to find answers to your random error message etc.

One nice thing is there are some [good free channels](https://cloud.google.com/community#home-support) to get help from GCP employees.

## Conclusion

Haskell runs great in Docker and has great library support for AWS and GCP. As such, Haskell can work well on both of them.

However, GCP has a few advantages, in particular cloud run is excellent for hosting APIs or gluing together pipelines with Haskell. I would strongly recommend considering it if you do Haskell, rather than just automatically reaching for AWS 😀.
