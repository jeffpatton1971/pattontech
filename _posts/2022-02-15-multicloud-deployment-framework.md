---
layout: post
title:  "Multi-Cloud Deployment Framework"
date:   2022-02-15 16:25:00 -0600
categories: blog
tags: February 2022 MultiCloudDeployment Development
author: Jeff
comments: true
---
So the way the original framework was designed was from a console perspective. While it was designed to be run from an Azure Devops Build Agent an engineer _could_ run this locally. So there were command-line arguments and all sorts of things you would expect from a script/utility. So as I began working on this version it started out more or less in the same vein. But I find myself sitting here looking at the code and thinking, this seems rather restrictive and redundant. For example, I'm defining in code an arm template that exists in a repository. You can then take that to the nth degree 1 template per templating language per cloud, that's a massive amount of duplication. Where is the source of truth for the templates? Does it live in the framework code? Does it live with the templates themselves?

### Framework Code

While there is nothing wrong with this design, you must admit it can be daunting. If we scope the problem to just Azure, we have 3 template languages, ARM, Bicep and Terraform. So for every one Azure Resource there are at least 3 classes. So the more I think about framework the more I think it doesn't really make sense that framework would be that source of truth. The templates themselves should be that, which means the framework needs to be able pull those templates down and then manipulate them in some fashion before returning the completed template.

### Template Code

It makes infinitely more sense to me that the source of truth should live within the templates themselves. This would allow a potential end-user to define their own templates. In fact it could even give that user the potential to deploy from differing versions of the same templates. Say a customer was deployed with v1 of a template and that template is now at v3, they could pass in the v1 url of that template to the framework and it would use the 'original' template as the origina deployment.

### What does this mean?

Well I think it means that the framework will wind up being a processor that has basically 3 functions. Each function is designed to process a given template language and update it with data from the Design Document before outputting the completed template to the user. I don't know at the moment what that would look like as this idea de-couples framework from the actual tempalates. It's possible that this may just be implemented as a PowerShell code module, where you have a series of cmdlets.

* Get-Template
* New-Template

Maybe? I'm not really sure, but the more I think on this the more it potentially make sense to me. Basically you would pass along the template url to the cmdlet and perhaps the payload and it would then return the updated template to the console. Something simple in PowerShell to get the template would look like this:

```powershell
[System.Net.Http.HttpClient]$client = New-Object System.Net.Http.HttpClient;
$client.GetStringAsync("https://raw.githubusercontent.com/MultiCloudDeployment/terraform-microsoftautomation-automationaccounts/main/variables.tf").GetAwaiter().GetResult();

variable "ResourceGroupName" {
  type        = string
  description = "Name of the ResourceGroup to deploy into."
}

variable "name" {
  type        = string
  description = "Name of the Automation Account."
}

variable "tags" {
 type         = object
 default      = {}
 description  = "Tags to add to the Automation Account."
}
```

So we would wind up storing the result, perhaps in a class that defines the processor for that language which should then allow to update the relevant parts so the output would be a useable template. This is just the raw output of my thinking, it makes sense in my head, just not really sure how to get that into workable code. The benefit here is framework wouldn't _need_ to know about the templates at all, just how to get them and how to update the proper values which would make this super flexible.
