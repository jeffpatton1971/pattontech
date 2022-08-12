---
layout: post
title:  "Introducing GithubClient"
date:   2022-08-11 11:30:00 -0600
categories: blog
tags: August 2022 Development
author: Jeff
comments: true
published: true
---
Introducting [GithubClient](https://github.com/jeffpatton1971/GithubClient), I started working on this because we had a need to pull information from one of our [Organizational](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/about-organizations) repositories. It started out as a POC with all the code written in the Controller for the API. Once that was working I decided that I would like a little more flexibility, and while I could write the code in a class within the API, I felt that perhaps having an external library would be a better solution. This would allow me to include it in several projects as needed or even write some PowerShell around it for utilties.

I released version [2.0.0](https://www.nuget.org/packages/Patton.GithubClient/) of the client today and also tagged it as a [release](https://github.com/jeffpatton1971/GithubClient/releases/tag/2.0.0) in Github.

### What's New
**BREAKING CHANGES**

Originally the methods were stored in folders named for the endpoint they represented, this made it difficult to easily include them as you had to include each one.

```c-sharp
using GithubClient.Repository.Methods;
using GithubClient.Blobs.Methods;
using GithubClient.Content.Methods;
using GithubClient.Trees.Methods;
```

All the class methods have been moved into a methods folder which makes the namespace much simpler, and a lot easier to include and use.

```c-sharp
using GithubClient.Methods;
```

This release now has completed documentation for all classes and methods, and the [project site](https://jeffpatton1971.github.io/GithubClient/) has been setup using Githug-Pages. Additionally, a simple method has been added to each class that defines the default header that should be used for that class. This function is now in use in all the methods to define the headers for the httpClient.

### What's Next
Since the Github API Url is standard, I was thinking about perhaps adding a helper function similar to GetHeader, perhaps even one that would build the proper API Url. This would keep that code within the class that requires it which I think would make a little more sense.

The httpClient is setup and used across all the methods and I think I could use a single client to reduce the complexity. I think there is a factory that would make it easier to create clients. Although most of the examples seem to revolve around Web API's rather than class libraries.

Logging, it would make debugging while doing development a lot easier. Again, most of what I've seen has been aroudn Web Api's versus class libraries.

I'll be setting up some [issues](https://github.com/jeffpatton1971/GithubClient/issues) later this evening.
