---
layout: post
title:  "May I Pester you?"
date:   2022-10-07 22:50:00 -0600
categories: blog
tags: October 2022 Development
author: Jeff
comments: true
published: true
---
Sorry, I couldn't resist, I've been pretty busy, and over the past few weeks I've been completely overhauling our entire build process. One of things I've dont is to completely re-write the modules we use. Previously, we had modules for most every step of the process and they were in seperate repos. Additionally the modules were monolithic script modules (for lack of a better word), all functions were encapsulated in a single psm1 file. We are left with a "single" module that has several nested modules within it. Each module has a set file structure.

```bash
-- Root
   -- Public
   -- Private
   -- Tests
   Root.psm1
   Root.psd1
```

I don't recall where I picked this structure up from, but it's modified from several different articles I read up to this point. Functions are self-contained scripts in either the Public or Private folders, each function has it's own test within the Public or Private folder. Finally the Module itself has a test in the Tests folder. The accepted testing Framework for PowerShell is Pester, I wrote some Pester tests in the past, like pre version 4 and currently we're at version 5.3.3, all the stuff I used to be able to do is no longer really possible, so it's been a real learning experience.

### Pester

So [Pester](https://pester.dev/) is the default testing framework for PowerShell, it comes pre-installed on Windows, but depending on your version of Windows you may or may not have the latest. So the first step for me was to get my Pester version up to date. Fortunately there is a [doc](https://pester.dev/docs/introduction/installation/#removing-the-built-in-version-of-pester) to walk you through the entire process. I need to write my tests, I think technically the tests I've written are [Unit tests](https://en.wikipedia.org/wiki/Unit_testing), we may get into Integration or Acceptance tests later, but I don't really know.

### BeforeAll

The [BeforeAll](https://pester.dev/docs/commands/BeforeAll) construct lets you do something before any tests run in your script, describe, context or it. I use in a few different places, at the top of each test to setup the module, and further down to setup things required for individual tests.

```powershell
BeforeAll {
    $Module = "Cloud";
    $RootPath = (Get-Item -Path .).FullName;
    Import-Module "$($RootPath)\Build\$($Module)\$($Module).psd1" -Force
}
```
> This block defines the name of the module, determines where we are on the filesystem and then loads the module

There are places within the tests where I use this to set things up so the tests will work.

```powershell
BeforeAll {
    [System.Data.DataSet]$ds = New-Object System.Data.DataSet('myDataSet');
    [System.Data.DataTable]$dt = New-Object System.Data.DataTable('myTable');
    $dt.Columns.Add('Field1','string') |Out-Null;
    [System.Data.DataRow]$dr = $dt.NewRow();
    $dr.Field1 = "Value";
    $dt.Rows.Add($dr);
}
```
> This block defines some things that are required for a test to work properly.

### ForEach

In the module tests I need to loop over the functions and while you used to be able to use Foreach-Object, that's not really possible now. To do this Pester has a [ForEach](https://pester.dev/docs/commands/Describe#-foreach) construct. This allows you to iterate over an array of objects so you can test them.

```powershell
$Functions = @('Add-DataTable', 'Find-RowNumber', 'Get-DataSet', 'New-DataSet', 'Update-DataTable')
it "<_>.ps1 should exist" -ForEach $Functions {
    "$($RootPath)\public\$_.ps1" | Should -Exist
}
```
> This passes the array Functions into the test

If you need to reference the items in the array, you have access to the pipeline variable '$_'. In the example above you can see that variable being used in the test as a part of the path, but it's also available in the description of the It test. Which you enclose in the angle brackets, but you need to remember to drop the dollar sign.

### Tests

The tests have allowed me to really look at the functions and to think about inputs and outputs. In a lot of places I've actually added and or changed the function code in order to make the tests work. This sounds bad, but the test should enforce the idea behind the function and I feel for the most part I've been able to accomplish this. I'm really happy with how the testing has turned out this article isn't really about how to use Pester as much as it's just how I've used Pester.

Have a great weekend!