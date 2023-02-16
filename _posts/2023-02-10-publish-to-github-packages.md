---
layout: post
title:  "Publish to Github Packages"
date:   2023-02-10 15:02:00 -0600
categories: blog
tags: February 2023 Development
author: Jeff
comments: true
published: true
---
This goes along with my last [post]({% post_url 2023-02-10-securing-secrets-in-visual-studio %}). I want to be able to consume a package from the Github Packages location within the org, so we need a way to publish there. Here is the workflow I used, it is a mix of the default .net desktop build and a [stackoverflow](https://stackoverflow.com/questions/57889719/how-to-push-nuget-package-in-github-actions) that I found.

```yaml
name: Publish to Github Release

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:

  build:

    strategy:
      matrix:
        configuration: [Release]
    runs-on: windows-latest  # For a list of available runner types, refer to
    env:
      Solution_Name: schema.sln

    steps:
    - name: Checkout
      uses: actions/checkout@v3
      with:
        fetch-depth: 0

    # Install the .NET Core workload
    - name: Install .NET Core
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: 6.0.x

    - name: Setup NuGet.exe for use with actions
      uses: NuGet/setup-nuget@v1.1.1
      with:
        nuget-version: latest

    # Add  MSBuild to the PATH: https://github.com/microsoft/setup-msbuild
    - name: Setup MSBuild.exe
      uses: microsoft/setup-msbuild@v1.0.2

    # Restore the application to populate the obj folder with RuntimeIdentifiers
    - name: Restore the application
      run: msbuild $env:Solution_Name /t:Restore /p:Configuration=$env:Configuration
      env:
        Configuration: ${{ matrix.configuration }}

    # Pack application
    - name: Pack application
      run: dotnet pack --configuration ${{ matrix.configuration }}

    - name: Configure Nuget
      run: nuget sources add -name "GPR" -Source https://nuget.pkg.github.com/MultiCloudDeployment/index.json -Username MultiCloudDeployment -Password ${{ secrets.GITHUB_TOKEN }}

    - name: Push package to GitHub registry
      run: nuget push .\bin\${{ matrix.configuration }}\*.nupkg -Source "GPR" -SkipDuplicate
```

I would consider this a rough-draft, most likely this I'll iterate on this for a while as there are still a few things hard-coded that I'm not a fan of.
