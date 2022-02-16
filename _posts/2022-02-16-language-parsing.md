---
layout: post
title:  "Language Parsing"
date:   2022-02-16 08:04:00 -0600
categories: blog
tags: February 2022 MultiCloudDeployment Development
author: Jeff
comments: true
---
[Yesterday]({% post_url 2022-02-15-multicloud-deployment-framework %}) I talked about changing up the framework to make it more flexible. Ideally the framework shouldn't need to know all the details about a given template, it should discover that and then do something with it. After that post I spent a fair amount of time trying to figure out what I meant!

See the problem is, well there is no problem for ARM as it's literally just JSON and handling that is more or less baked into just about everything anymore. The real problem is Terraform and Bicep, they are both [HCL](https://github.com/hashicorp/hcl). While Terraform may be mostly straight HCL, I think [Bicep](https://github.com/Azure/bicep) is a slightly modified hybrid of that, I've not dug in too deeply yet. While they do have patterns and blocks it's not as simple as key/value pairs in JSON.

At first I thought I might be able to sort out classes and such on my own, and it could be possible. I started looking at how to write your own parser. I wound up looking at articles that had some hefty regex in them and that reminded me about the problem inherent with regex:

![Regex](https://imgs.xkcd.com/comics/perl_problems.png)

While the idea of writing classes to parse this out isn't that intimidating to be honest it seems like this work should already have been done. It turns out that of course it has been done and the answer is [ANTLR](https://github.com/antlr/antlr4). So most likely the next few weeks will involve some in-depth reading on working with antlr and writing grammar files. Fortunately there is a [Terraform](https://github.com/antlr/grammars-v4/tree/master/terraform) grammar file so that should help greatly.

Anyway, welcome to Hump Day!
