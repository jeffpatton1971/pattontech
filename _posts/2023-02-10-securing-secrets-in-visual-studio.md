---
layout: post
title:  "Securing Secrets in Visual Studio"
date:   2023-02-10 12:02:00 -0600
categories: blog
tags: February 2023 Development
author: Jeff
comments: true
published: true
---
When working on a project locally it can be easy to simply add secret information into the appsettings.json file. The upside to this is that it's quick and easy, the downside is that this is stored along with the rest of your code in the repository (making the assumption you are using git or something similar). Often it's very easy to overlook that point and we wind up commiting things like passwords or any other secret infomation into the repo. While this may be acceptable for an internal group, it's a bad habit to get into.

## Visual Studio Secrets Manager

Someone at Microsoft wrote a nice little [article](https://devblogs.microsoft.com/premier-developer/managing-cloud-ready-net-app-secrets-in-visual-studio/) that could get you started. Additionally the [documentation](https://learn.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-7.0&tabs=windows) provides some very nice details on how to use the Secrets Manager from the command line. The best part of using Secrets Manager is that it stores the secrets outside of the project so it's nearly imposible to accidentally commit it into the repo.

In my previous [article](_posts\2023-02-09-a-mongodb-repository-framework-design.md) I mentioned making some code avaailable as soon as I had the secrets sorted out, it took most of the day but I've gotten it sorted. The code was already in place but it kept returning null, until I found an article on Medium that [mentioned](https://dotnetcoretutorials.com/2022/04/28/using-user-secrets-configuration-in-net/) I was missing an important package.

```powershell
Install-Package  Microsoft.Extensions.Configuration.UserSecrets
```

Once I got this package added into the project the code to pull in the secrets from the Secret Manager started working as expected. First I needed to add it into the configuration which was very straightforward.

```c-sharp
IConfiguration config = new ConfigurationBuilder()
    .AddJsonFile("appsettings.json")
    .AddUserSecrets(Assembly.GetExecutingAssembly(), true)
    .AddEnvironmentVariables()
    .Build();
```

Once that was in place then the code started working as expected!

```c-sharp
builder.Services.Configure<MongoDbSettings>(config.GetSection("MongoDbSettings"));
```

Happy Friday!
