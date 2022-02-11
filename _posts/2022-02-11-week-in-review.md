---
layout: post
title:  "Week In Review: 02/11/2022"
date:   2022-02-11 09:13:00 -0600
categories: blog
tags: February 2022 Rackspace
author: Jeff
comments: true
---
Happy Friday! Most of this week has been involved with meetings and getting this site all sorted out, but I'd like to review what I've been working on so you all have a good idea of where things stand.

I've been working on my open source project [Multi-Cloud Deployment](https://github.com/MultiCloudDeployment). The goal is to provide anyone the ability to deploy resources to any cloud in a simple and efficient manner. Let me break down the various repositories so you have a better understanding of what everything is.

### Schema

The [Schema Repo](https://github.com/MultiCloudDeployment/schema) is C# project that defines classes for all the components and resources of a given cloud. The classes are grouped in folders by cloud, so everything related to Azure will live in the Azure folder. This schema defines a specific layout that can then be processed and used to produce cloud specific templates. A [sample design](https://github.com/MultiCloudDeployment/schema/blob/main/sample.json) is below:

``` json
{
  "Platform": {
    "Azure": {
      "Tenant": {
        "id": "",
        "displayName": "",
        "domains": [ "" ],
        "Subscriptions": [
          {
            "id": "",
            "displayName": "",
            "Resources": {
              "AutomationAccounts": [
                {
                  "name": "",
                  "location": "",
                  "tags": {},
                  "SkuName": "",
                  "DisableLocalAuth": false,
                  "PublicNetworkAccess": true
                }
              ]
            }
          }
        ]
      },
      "DeploymentLanguage": ""
    }
  }
}
```

You can see that top-most object is Platform, the Platform defines each supported Cloud (currently only Azure). Each cloud will be defined by the properties required to connect to it. In the case of Azure we have the Tenant object, which contains an array of subscription objects and the combination of Tenant and Subscription information allows you to connect to that account for a given deployment. Within each subscription then is a Resources object that contains all the supported Cloud Resources. Originally I had thought about breaking things up into environments but it quickly becomes very tedious, trust me, I'm going to keep with the 'keep it simple stupid' adage to make this as useful as possible. Environments if required can be defined in the various tagging options available for each cloud. Along with Cloud specific properties I have added a DeploymentLanguage, this would be the preferred templating for deployment. In the case of Azure this can be ARM, Terraform and, Bicep.

### Backend

The [Backend Repo](https://github.com/MultiCloudDeployment/backend) is a C# project that is used to generate the JSON Design Document, basically the JSON shown above. The Schema defines what is required and the backend passes values and generates the appropriate outputs so a valid Design Document can be created. Currently this all handled via Rest API's.

### Framework

The [Framework Repo](https://github.com/MultiCloudDeployment/framework) contains a C# project that will be used to convert the Design Document into the appropriate Cloud Specific templates. Currently this will be used as PowerShell module to make generating the templates easier.

### Template Repos

There will be several template repositories, each one would contain the necessary templating to deploy a specific cloud resource. I have a defined naming convention to make things easier to see in Github.

``` text
templatelanguage-resource
```

So, in the case of Azure we have several, I think what will be most common will be either [Terraform](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs) or [Bicep](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview?tabs=bicep). For azure the resources have the API path builtin so they look like this:

``` text
arm-microsoftautomation-automationaccounts
bicep-microsoftautomation-automationaccounts
terraform-microsoftautomation-automationaccounts
```

As you can see this will make things much easier to find as I continue to add more resources then these will continue to grow. I created them as seperate repositories so anyone can contribute or just use as-is. But we should be able to track issues and progress within each resource this way.

### Templating Comparisons

What follows is me more or less ranting about a few things as I've been doing a very long time. I started out writing templates for Lowe's when we managed IrisByLowes in Azure. We needed a way to quickly deploy virtual machines and it needed to be re-useable and repeatable and simple. At that time ARM had just been released in favor of ASM so we transitioned from scripts to templates. I have gotten quite good at writing ARM templates, in fact I have a repo that is more or less a [Template Engine](https://github.com/jeffpatton1971/arm-property-transform-collector) it will build templates from templates it's pretty crazy and pretty cool and pretty much overwhelming. But I digress, when I got into building at Rackspace we had a collection of templates that everyone used, but the downside is that they were very complicated and had so many dependencies, it made it difficult to troubleshoot at times. So I started working on creating what could loosely be described as standard deployments templates, that were re-useable and repeatable. I personally enjoy writing ARM template and have no issues with them, when they started adding functions and basic logic they became incredibly powerful. The best part is when Microsoft releases and API for a new Azure Resource, the ARM is more or less already avaiable.

Terraform is a 'cloud-agnostic' templating language, I put that in quotes because I don't know if that's truly the case or not. They leverage providers that allow you to use the same language construct ([HCL](https://github.com/hashicorp/hcl)) to define your resources. They have the added ability to sort out the dependencies in advance, something that ARM lacked and was a highly requested feature. But basically the idea is if you know Terraform for one Cloud you should be able to then create Terraform for any cloud and in that sense you could say it's agnostic. I personally am not a huge fan, simple because as you look the resources you can see that in the case of Azure (since that's where I do most of work) several resources are woefully outdated.

So Microsoft decided to compete in this area by inventing Bicep, which is a translating language when you run it, it outputs ARM templates. On the upside it has the same look and feel as Terraform, so if you are Terraform dev then Bicep is pretty straightforward or at least familar. It also provides the dependency chaining that Terraform offers. My largest complaint has to do with modules.

Both Terrafom and Bicep provide a module format, which allows you to encapsulate several things into a single template. Basically you could have a module that contained an entire tiered application deployment and only provide a handful of values and get a complete build when you're done. Terraform has the ability to reference local files or github URL's this makes it incredibly easy to work with external/third-party repositories or even your own. Bicep on the other had opted to confine you to either local files or an [Azure Container Registry](https://docs.microsoft.com/en-us/azure/container-registry/) and while I think that's a free option, you will need an Azure account to set it up. I feel this is some sort of sales gimmick/marketing nonesense...to be blunt it doesn't make any sense, I can see this as one of multiple options, but as the only online option can be a non-starter. But, on the other hand, if they never provide the ability to use these for other clouds, then I guess it doesn't really matter.

At any rate I think I've ranted long enough, I'm very excited about this project as it closely aligns with what I'm working on at Rackspace. Please feel free contribute or comment or whatever, and I hope you have a wonderful rest of your day!
