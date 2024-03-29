---
layout: post
title:  "Azure API Management"
date:   2022-04-05 08:12:00 -0600
categories: blog
tags: April 2022 IT Azure
author: Jeff
comments: true
published: false
---
The bulk of the code that I write for work is wrapped around the idea of multi-cloud deployments. Basically the need to deploy resources for customers into any cloud in such a way that a virtual machine deployed into Azure is configured the same way as it would be in Amazon or Google. A lot of this is dependant on API's that I manage and maintain and I thought it was a good time to kind of talk about Azure API Management and maybe share a few lesson's learned along the way.

## Azure API Management, what is it?

Basically what APIM provides is a wrapper around your existing API's wherever they may live. It allows you to group your API's into a catalog that developers and consumers can then explore and learn from. Additionally it adds a layer of security over your API's to free the developer up from having to deal with certain things. For a more detailed an in-depth overview head on over to the [documentation](https://docs.microsoft.com/en-us/azure/api-management/api-management-key-concepts) to learn more.

One of the things that we like about it is the Api Gateway, this allows us to have a single front-end to all API's that are currently in use. This gives us control over the namespace of all API's, so while some may live in Azure and some on-prem, they are all accessed via a single URL. A single point of management for API's is also a very nice benefit, we don't leverage all the capabilities that are available, but being able to go to one place to define and customize an API is very convenient.

Step by Step guide for setting up an API hosted on Azure App Services (Linux Edition)

1. login in portal.azure.com
2. open the Api Management resource
3. you can add a product by clicking into product
   1. click add
   2. Provide a name, id, description
   3. optionally select if it is published or requires subscription (default) or approval
   4. you can give it a subscription limit and legal terms
   5. finally add an API if it already is setup
4. you can add an API but clicking into Api
   1. you can create an api fro an openApi document
      1. paste the url or provide the json you downloaded
      2. it should read in most properties from the json
         1. but you can define a display name, name and description
         2. set the url scheme and suffix
      3. you can add tags and products
      4. you can add which gateway to use
      5. finally whether this should be versioned