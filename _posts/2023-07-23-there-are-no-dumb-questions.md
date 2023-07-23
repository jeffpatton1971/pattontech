---
layout: post
title:  "There are no dumb questions!"
date:   2023-07-23 15:06:00 -0600
categories: blog
tags: July 2023 Development
author: Jeff
comments: true
published: true
---
One of the many wonderful people I work with at Rackspace recently asked if they could as a dumb question. I taught technical classes for over 12 years before launching my career into "regular" IT work and in that time I learned that there truly are no dumb questions. If you are comfortable enough to talk to me and ask, then I will do my best to help you grasp whatever it is you want to know. That's really just a function of who I am.

## I've had coding classes, but can't grasp converting the basics into something functional

First off I want to set expectations, I write a lot of code and have been writing code for a very long time now. Do not take what I'm about to say as the best way to approach the problem, this is how I work day to day and it's worked extremely well for me.

I've never really given any in-depth thought about how I approach a development problem. Perhaps the best way for me to explain it is to give an example of a recent problem I've been working. Last week I wrote about an issue I face with Azure Devops, where you can only grab the latest release of a nuget package regardless if the package has been flagged as "release" or "pre-release".

My process starts researching the problem, in this instance I needed to validate that I didn't have something configured incorrectly, or that I was using the tool in the wrong way. From what I was able to discover there was nothing misconfigured in Azure Devops and I was using the nuget command in the correct way. As I have been working on a PowerShell module for Azure Devops, I was aware of the API and focused my research on the API and how it would respond. After an hour or so of reading and poking at the API with PowerShell I came to the conclusion that the "release" or "pre-release" monikers had no impact when it comes to a repsonse. Everything was returned as "type" release, which mean that my "pre-release" view was basically ignored.

The next step for me is to then take the big picture problem and break it down into smaller more manageable pieces. How do I get from the name of a package to the latest release version that I need? In Azure Devops the packages are stored in an [Artifact Feed](https://learn.microsoft.com/en-us/azure/devops/artifacts/concepts/feeds?view=azure-devops). Those feeds have [Views](https://learn.microsoft.com/en-us/azure/devops/artifacts/concepts/views?view=azure-devops) and Packages are associated with one or more of those Views. Each package has a specific version that goes along with it.

So I have my set of smaller steps:

1. Get the feed
2. Get the view
3. Get the package for that view
4. Get the lastest version

In reality for me, there are several sub-steps/tasks that go along with each step, for the purposes of this post we'll stick to the high-level steps. Again this may or may not be the best way to do things, but this is how I approach the problem.

I have existing code that I can use as a reference, and then I just work with the API documentation and the command-line until I get something that works close to what I want. It's at that point that I formalize the snippets I have into a function and then I iterate on that code until I feel it will always return what I expect it to return. The code itself is very straight forward and has never been truly complex, and this I think leads to the answer to this question.

## The Answer

Identify the problem you are facing, determine if there is a way to break it down into discreet steps and finally work out each step until it gives you the desire result. I know that sounds very simplistic, but that's really how I approach the problem. I don't try to solve the entire problem at once, I break it down into smaller pieces and then work on each piece until I get the desired result. I then take those pieces and put them together into a larger solution. What language you use makes little difference, the process is the same and all the languages functionally work in the same way.

1. Identify the problem
2. Break the problem down into manageable pieces
3. Solve each piece

## Conclusion

Honestly this is how I approach most problems, not every problem I work can be solved with code, but the steps are always the same. I hope this helps, and I hope you have a great day!
