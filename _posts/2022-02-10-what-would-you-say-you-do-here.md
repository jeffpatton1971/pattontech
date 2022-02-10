---
layout: post
title:  "What would you say, you do here?"
date:   2022-02-10 09:06:00 -0600
categories: blog
tags: February 2022 Rackspace
author: Jeff
comments: true
---
Let's start with a brief history. I started at Rackspace in July of 2016 as a DevOps Engineer III and orignally worked in the Azure Support Team. For about a year I worked with large and small customers as they brought their products into Azure and helped them sort out what was going on when things weren't working. About 2 years later I moved over the Azure Build Team where once again I worked with customers, but this time I helped them bring their resouces into Azure. Finally about a year ago a small Automation Team was created that borrowed folks from Azure Build, AWS Build and GCP Build. This team worked on defining a process to automate deployments into all three major cloud vendors, globally.

When I was working in Support I wrote a lot of code, most of it was mainly around doing rather common things, pulling down all the vm's from a customer account that met a certain criteria, making copies of Network Security Groups, things of that nature. When I moved into Build the code changed, most of it revolved around automating my deployments. As a team we took over the ARM templates that Rackspace used for deployments and transitioned those into Visual Studio Team Services (now [Azure DevOps](https://dev.azure.com)). From that point we started re-defining those templates so that they were up to date and met with our current build standards at the time. During that time I put together a process that would collect all the build information required in a single JSON document and then from there generate the parameter files required for the build, before ultimately deploying those via a Release pipeline.

If we look at the work that I've recently done it's very similar. Collectively we met with Engineers from each team and standardized on a common set of inputs. From there we defined a schema that allowed us to validate those inputs and from that generate a JSON document. That document then contained the sum of the data required for the build which was then passed into our automation code to generate the required terraform and finally that terraform was then deployed into a customer's environment. Terraform was chosen by Rackspace mainly because it was "cloud agnostic" (to an extent) and allowed "Engineers to easily deploy any cloud".

As for what I do, my job has become an almost exclusive developer role. I write an maintain C# API's, PowerShell Script and PowerShell Code modules, everything I do is almost exclusively maintained within [Github](https://github.com) now and our automation runs throuh Azure DevOps. I can honestly say that this past year has been one of the most exciting I've had at Rackspace and I look forward to what we can accomplish.
