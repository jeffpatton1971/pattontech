---
layout: post
title:  "SchemaModule 3.0.1 Release"
date:   2022-02-19 00:23:00 -0600
categories: blog
tags: February 2022 Development
author: Jeff
comments: true
published: true
---
I've just released a new version of my [PowerShell SchemaModule](https://github.com/SchemaModule/PowerShell/releases/tag/v3.0.1).

This release adds a Headers parameter that accepts a hashtable of key/value pairs. This provides the ability to retrieve a schema from a server or REST endpoint that requires an api-key. Creating the headers is pretty straightforward:

```powershell
$Headers = @{}
$Headers.Add('Content-Type','application/json')
$Headers.Add('Accept','application/json')
$Headers.Add('Cache-Control','no-cache')
```

This example shows how to create a hashtable and add key/value pairs to it. These can then be passed along the command-line with the [optional](https://github.com/SchemaModule/PowerShell/blob/master/docs/Get-SchemaDocument.md#-headers) param -Headers.

The module can be downloaded from github or the PowerShell Gallery.

* [Github Release](https://github.com/SchemaModule/PowerShell/releases/tag/v3.0.1)
* [PowerShell Gallery](https://www.powershellgallery.com/packages/schema/3.0.1)

Feel free to leave comments or questions in the [Discussion](https://github.com/SchemaModule/PowerShell/discussions/47) board.
