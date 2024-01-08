---
layout: post
title:  "Querying PowerShell Gallery and Nuget.org"
date:   2024-01-08 08:43:00 -0600
categories: blog
tags: January 2024 Development
author: Jeff
comments: true
published: true
---
I've decided that I want to changeup how I display my projects on the blog, I think I could create a landing page for all my code and then create dedicated locations for things that are published. Additionally I think it would be fun to see some of the stats displayed here, which is easy enough for an individual project, but not for a large group. This got me thinking how can I query nuget.org and powershellgallery.com for a list of packages that I've published.

In terms of nuget.org it's not to terribly difficult and after a few hit and miss attempts I was able to put together a nice little one-liner that returns all the code that I have published there. It was simply a matter of finding the right [endpoint](https://learn.microsoft.com/en-us/nuget/api/search-query-service-resource) and knowing what to ask.

```powershell
(Invoke-RestMethod -Method Get -Uri "https://azuresearch-usnc.nuget.org/query?q=owner:jeffpatton1971") |Select-Object -ExpandProperty data

@id             : https://api.nuget.org/v3/registration5-semver1/patton.githubclient/index.json
@type           : Package
registration    : https://api.nuget.org/v3/registration5-semver1/patton.githubclient/index.json
id              : Patton.GithubClient
version         : 2.3.0
description     : A C# Dll library for working with the Github Rest API
summary         :
title           : Patton.GithubClient
licenseUrl      : https://www.nuget.org/packages/Patton.GithubClient/2.3.0/license
projectUrl      : https://jeffpatton1971.github.io/GithubClient/
tags            : {Github, GithubApi}
authors         : {Jeffrey Patton}
owners          : {jeffpatton1971}
totalDownloads  : 1694
verified        : False
packageTypes    : {@{name=Dependency}}
versions        : {@{version=1.0.0; downloads=339; @id=https://api.nuget.org/v3/registration5-semver1/patton.githubclient/1.0.0.json}, @{version=1.1.0; downloads=327;
                  @id=https://api.nuget.org/v3/registration5-semver1/patton.githubclient/1.1.0.json}, @{version=1.1.1; downloads=341;
                  @id=https://api.nuget.org/v3/registration5-semver1/patton.githubclient/1.1.1.json}, @{version=2.0.0; downloads=336;
                  @id=https://api.nuget.org/v3/registration5-semver1/patton.githubclient/2.0.0.json}…}
vulnerabilities : {}

@id             : https://api.nuget.org/v3/registration5-semver1/retrosheet/index.json
@type           : Package
registration    : https://api.nuget.org/v3/registration5-semver1/retrosheet/index.json
id              : RetroSheet
version         : 2.0.0
description     : A collection of models that represent the data structures in Retrosheet data files.
summary         :
title           : RetroSheet
tags            : {}
authors         : {Jeffrey S. Patton}
owners          : {jeffpatton1971}
totalDownloads  : 118
verified        : False
packageTypes    : {@{name=Dependency}}
versions        : {@{version=2.0.0; downloads=118; @id=https://api.nuget.org/v3/registration5-semver1/retrosheet/2.0.0.json}}
vulnerabilities : {}

@id             : https://api.nuget.org/v3/registration5-semver1/modposh.logger/index.json
@type           : Package
registration    : https://api.nuget.org/v3/registration5-semver1/modposh.logger/index.json
id              : ModPosh.Logger
version         : 1.0.0
description     : A Logging library for .Net and PowerShell
summary         :
title           : ModPosh.Logger
projectUrl      : https://github.com/mod-posh/Logger
tags            : {}
authors         : {Jeffrey S. Patton}
owners          : {jeffpatton1971}
totalDownloads  : 55
verified        : False
packageTypes    : {@{name=Dependency}}
versions        : {@{version=1.0.0; downloads=55; @id=https://api.nuget.org/v3/registration5-semver1/modposh.logger/1.0.0.json}}
vulnerabilities : {}
```

Asking the PowerShellGallery was a little more difficult, where the nuget api is version 3 and there is decent [documentation](https://learn.microsoft.com/en-us/nuget/api/overview) for it, the PowerShellGallery is an older version of nuget. It seems really to be geared almost exclusively to the tools you are meant to use to interact with it and the only [documentation](https://joelverhagen.github.io/NuGetUndocs/) I've found is an unofficial documentation page. Fortunately I was able to get something to work.

```powershell
Invoke-RestMethod -Method Get -Uri "https://www.powershellgallery.com/api/v2/Packages()?`$filter=Authors eq 'jeffrey@patton-tech.com'"

id         : https://www.powershellgallery.com/api/v2/Packages(Id='AzDevOps',Version='1.0.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-31T11:17:41Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='AzDevOps',Version='1.0.1')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-31T17:37:49Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='AzDevOps',Version='1.0.2')
category   : category
link       : {link, link}
title      : title
updated    : 2023-09-30T19:03:41Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='AzDevOps',Version='2.0.0')
category   : category
link       : {link, link}
title      : title
updated    : 2023-12-19T22:54:04Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='AzDevOps',Version='2.1.0')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-08T14:30:19Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='Pipelines',Version='1.4.0')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-04T18:07:26Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='Pipelines',Version='1.5.0')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-04T18:07:26Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='Pipelines',Version='1.6.0')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-04T18:07:26Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='Pipelines',Version='2.0.0')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-07T16:15:17Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshAdoTask',Version='1.0.0')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-01T07:32:03Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='1.0.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-11-24T03:40:04Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='1.1.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-11-24T03:40:04Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='2.2.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-11-26T07:25:57Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='2.3.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-05T14:59:18Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='2.4.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-05T14:59:18Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='2.4.1')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-08T03:34:27Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='2.4.2')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-08T10:34:28Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='3.0.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-20T21:09:53Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='3.1.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-20T21:09:53Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='3.1.1')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-20T21:09:53Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='3.2.0')
category   : category
link       : {link, link}
title      : title
updated    : 2022-12-20T21:09:53Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='PoshMongo',Version='3.2.1')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-04T21:52:28Z
author     : author
content    : content
properties : properties

id         : https://www.powershellgallery.com/api/v2/Packages(Id='RetroSheetModule',Version='2.0.0')
category   : category
link       : {link, link}
title      : title
updated    : 2024-01-01T08:42:04Z
author     : author
content    : content
properties : properties
```

The main difference here, is while Nuget has an Owner property, PowerShellGallery only has an Authors property. Turns out I've used 3 different Author names over the years! So step one will be to re-push all the packages so they all share the same Author and then see if we can consolidate the data. The PowerShellGallery returns all packages as discreet id's, there is a Properties property that will allow this list to be sorted, grouped and filtered.

At any rate this little post is just to remind me that I've actually done this, so we can come back to this later and try to create some sort of landing page that would accumulate downloads and such.

Have a great Monday!
(if that's possible)
