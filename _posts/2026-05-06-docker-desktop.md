---
layout: single
title:  "Docker Desktop"
date:   2026-05-06 10:29:00 -0600
categories: blog
tags: May 2026 Development Docker
author: Jeff
comments: true
published: true
---

Working on migrating my personal site [kansaspattons.org](https://kansaspattons.org) and decided I wanted to pull down all my "posts" from Social Media aka Facebook and Instagram. This has resulted in a very large number of images and posts, and we're at the very limit of what Jekyll on Github Pages can do. So I did what any sane person would do and write a full backend photo api, hence [PhotoArchive](https://github.com/jeffpatton1971/PhotoArchive).

Ultimately since I currently write all images to a storage account in Azure, I decided that I'll continue to leverage that and create an Azure PostgreSql instance. In order to develop the API I needed to haev PostgreSql installed locally, so instead of doing a whole installation I thought Docker was the answer.

So the issue, on Windows (I'm  running Windows 11, but this may be an issue for earlier vesrions), I was getting a weird error message:

> [!IMPORTANT]
> C:\ProgramData\DockerDesktop must be owned by an elevated account

This error occurs when Docker Desktop’s installer detects that the C:\ProgramData\DockerDesktop folder is not owned by an elevated (administrator) account. It’s a security measure introduced to prevent privilege escalation vulnerabilities. The issue often happens if the folder already exists with incorrect permissions or ownership, possibly from a previous installation attempt.

My solution as I had not attempted to install Docker was to open an elevated prompt and delete and recreate that folder:

``` powershell
cd C:\ProgramData\DockerDesktop
Remove-Item DockerDesktop -Recurse -Force
New-Item -Name DockerDesktop -ItemType Directory
```

After that re-run the installation and everything should go, if however you already had Docker installed, it might be enough to just take ownership and assign Administrators. If you want to do that via PowerShell:

``` powershell
$dockerData = "C:\ProgramData\DockerDesktop"
$owner = New-Object System.Security.Principal.NTAccount("Administrators")
$dir = Get-Item $dockerData
$sd = $dir.GetAccessControl()
$sd.SetOwner($owner)
$dir.SetAccessControl($sd)
```

Your mileage may vary on that one.

