---
layout: post
title:  "PackageManagement/nuget fails to pull release from AzureDevops"
date:   2023-07-13 09:50:00 -0600
categories: blog
tags: July 2023 Development
author: Jeff
comments: true
published: true
---
At work I use Azure Devops Artifacts to store the Modules and C# libraries that I work with. I leverage the views for Release and Pre-Release as a way for me to keep track of where I am and what's in progress.

In my pipelines for builds I tend to use nuget as that's what I use in my own local process. But working an issue last night I noticed that one of my modules was not pulling the correct DLL. The build pipeline was pulling a pre-release version as it was technically a later version of the DLL. I was able to quickly resolve this using the Artifact task. The Artifact task allows you to select a feed, a view and a package. Finally you can specify a version or in my case 'latest' which will grab the latest package from teh specified view.

Where this became a real issue was in my local environment. I use psake to handle my local build operations and I've used nuget to grab the DLLs from the feed. The problem though is like in my pipeline it was pulling the most recent release. So I started down a sprialing circle of disappointing discovery.

## Problem

I need to be able to download the latest version of a specific package from the Release view in Artifacts. Currently available tools are unable to do this.

## Nuget

This is the tool of choice for me, it's simple and easy all I really need to do is point it at a source and tell it what I want. If I want to allow Pre-Releases there is a -PreRelease flag that I can pass to make this work. What I noticed first was I wasn't using that flag anywhere. There isn't a whole lot more to this command so I added the -Verbose Detailed option in the hopes that it would yield information that I could parse out but that was not the case.

I could ask for a list of AllVersions and I would see the specific version I was after, but that didn't really address what I wanted to be able to do.

## PackageManagement

Then I decided to see if I would have more luck with PackageManagement, I've used it before with Ado Artifacts and it worked well enoguh. But again, it only pulled the latest version of the package and again, I coudl list all packages and see the correct version, but no workable way to retrieve that specific one.

## Ado Rest API

Finally I tried the API, I've been working on a module for the [Azure Devops Rest API]({% post_url code %}) and I did get some nice sample code, so there should be an update coming soon. I was able to get a list of feeds from the organization.

```powershell
$Feeds = Invoke-AdoEndpoint -Uri "https://feeds.dev.azure.com/$($OrgName)/_apis/packaging/Feeds?api-version=7.1-preview.1" -Method GET -Headers $Global:azDevOpsHeader
$Feed = $Feed.value |Where-Object -Property Name -eq $FeedName
```

From the feed I was able to get specific views.

```powershell
$View = Invoke-AdoEndpoint -Uri "$($Feed.url)/views/$($Feed.id)?api-version=7.1-preview.1" -Method GET -Headers $Global:azDevOpsHeader
```

I could get a list of all the pacakges in the view.

```powershell
$Packages = Invoke-AdoEndpoint -Uri $Feed._links.packages.href -Method GET
```

From the packages I could get a list of all the versions.

```powershell
$Package = $Packages.value |Where-Object -Property Name -eq $PackageName
$Versions = Invoke-AdoEndpoint -Uri $Package._links.versions.href -Method GET
$Version = $Versions.value |Where-Object -Property isLatest -eq $true
```

From here I could potentially get the file from the Nuget API.

```powershell
Invoke-AdoEndpoint -Uri "https://pkgs.dev.azure.com/$($OrgName)/_apis/packaging/feeds/$($Feed.id)/nuget/packages/$($Package.normalizedName)/versions/$($Version.version)/content?api-version=7.1-preview.1" -Method GET -Headers $Global:azDevOpsHeader -ContentType 'application/octet-stream' > "$($Package.normalizedName).nupkg"
```

It's at this point though where things, I receive a file, but it's not a valid nupkg file. The documentation is extremely light on this subject, but I'll keep searching as this is the closest I've come to a solution.

## Conclusion

Until such time as I'm able to sort out the Nuget API, my solution was to place a Version variable in my psake, and try my best to remember when I update the DLL to also update that version in the psake file.

Hope you all have a wonderful day and thanks for stopping by!
