---
layout: post
title:  "Well this is fun!"
date:   2023-07-14 10:21:00 -0600
categories: blog
tags: July 2023 Development
author: Jeff
comments: true
published: true
---
I've started working on the latest version of my AzDevOps PowerShell Module, and I've run into an error that has me stumped.

```powershell
Import-Module .\output\AzDevOps -Force
Import-Module: The given assembly name was invalid
```

Now I've seen this error message before when working with my Binary modules (C# modules), but this is a script module, there are no assemblies, it's all straight PowerShell script files. So let's look at the exception.

```powershell
$Error[1].exception

Message        : The given assembly name was invalid.
FileName       : D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps\output\AzDevOps\AWS.Tools.Installer\AWS.Tools.Installer.psd1
FusionLog      :
TargetSite     : System.Management.Automation.PSModuleInfo LoadBinaryModule(System.Management.Automation.PSModuleInfo, System.String, System.String, System.Reflection.Assembly,
                 System.String, System.Management.Automation.SessionState, ImportModuleOptions, ManifestProcessingFlags, System.String, Boolean ByRef, System.String, Boolean)
Data           : {}
InnerException :
HelpLink       :
Source         : System.Management.Automation
HResult        : -2146232799
StackTrace     :    at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadBinaryModule(PSModuleInfo parentModule, String moduleName, String fileName, Assembly assemblyToLoad, String
                 moduleBase, SessionState ss, ImportModuleOptions options, ManifestProcessingFlags manifestProcessingFlags, String prefix, Boolean& found, String shortModuleName, Boolean
                 disableFormatUpdates)
                    at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadModuleNamedInManifest(PSModuleInfo parentModule, ModuleSpecification moduleSpecification, String moduleBase,
                 Boolean searchModulePath, String prefix, SessionState ss, ImportModuleOptions options, ManifestProcessingFlags manifestProcessingFlags, Object privateData, Boolean&
                 found, String shortModuleName, Nullable`1 manifestLanguageMode)
                    at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadModuleManifest(String moduleManifestPath, ExternalScriptInfo manifestScriptInfo, Hashtable data, Hashtable
                 localizedData, ManifestProcessingFlags manifestProcessingFlags, Version minimumVersion, Version maximumVersion, Version requiredVersion, Nullable`1 requiredModuleGuid,
                 ImportModuleOptions& options, Boolean& containedErrors)
                    at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadModule(PSModuleInfo parentModule, String fileName, String moduleBase, String prefix, SessionState ss, Object
                 privateData, ImportModuleOptions& options, ManifestProcessingFlags manifestProcessingFlags, Boolean& found, Boolean& moduleFileFound)
                    at Microsoft.PowerShell.Commands.ModuleCmdletBase.LoadUsingExtensions(PSModuleInfo parentModule, String moduleName, String fileBaseName, String extension, String
                 moduleBase, String prefix, SessionState ss, ImportModuleOptions options, ManifestProcessingFlags manifestProcessingFlags, Boolean& found, Boolean& moduleFileFound)
                    at Microsoft.PowerShell.Commands.ImportModuleCommand.ImportModule_LocallyViaName(ImportModuleOptions importModuleOptions, String name)
                    at Microsoft.PowerShell.Commands.ImportModuleCommand.ImportModule_LocallyViaName_WithTelemetry(ImportModuleOptions importModuleOptions, String name)
                    at Microsoft.PowerShell.Commands.ImportModuleCommand.ProcessRecord()
                    at System.Management.Automation.CommandProcessor.ProcessRecord()
```

Note that filename up there, it's referencing an AWS module, somehow. So to be safe we'll check to see if that file exists.

```powershell
Get-Item D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps\output\AzDevOps\AWS.Tools.Installer\AWS.Tools.Installer.psd1
Get-Item: Cannot find path 'D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps\output\AzDevOps\AWS.Tools.Installer\AWS.Tools.Installer.psd1' because it does not exist.
```

Well that's interesting, it doesn't exist. I'm not really sure where this is coming from, my only guess, is that I recently changed my psake file to grab all the modules into a variable to cut down the query time. Perhaps those are getting autoloaded somehow.

I did in fact install the AWS PowerShell modules when I was doing some testing for something, but I don't think they are actually needed anymore so I should be able to remove them.

```powershell
# JeffreyPatton@FSTNQL1 | 10:19:02 | 07-14-2023 | [4.84GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
$ModuleList = Get-Module -ListAvailable
# JeffreyPatton@FSTNQL1 | 10:19:21 | 07-14-2023 | [4.84GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
$ModuleList |Where-Object -Property Name -Like "*AWS*"

    Directory: D:\Documents\PowerShell\Modules

ModuleType Version    PreRelease Name                                PSEdition ExportedCommands
---------- -------    ---------- ----                                --------- ----------------
Script     1.0.2.0               AWS.Tools.Installer                 Core,Desk {Uninstall-AWSToolsModule, Install-AWSToolsModule, Update-AWSToolsModule}
Binary     4.1.8.0               AWSPowerShell                       Desk
Binary     4.1.8.0               AWSPowerShell.NetCore               Desk

# JeffreyPatton@FSTNQL1 | 10:27:34 | 07-14-2023 | [5.17GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
$ModuleList |Where-Object -Property Name -Like "*AWS*" |Select-Object -ExpandProperty Name |ForEach-Object {Uninstall-Module -Name $_}

# JeffreyPatton@FSTNQL1 | 10:28:08 | 07-14-2023 | [5.49GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
$ModuleList = Get-Module -ListAvailable
# JeffreyPatton@FSTNQL1 | 10:29:01 | 07-14-2023 | [5.49GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
$ModuleList |Where-Object -Property Name -Like "*AWS*"
# JeffreyPatton@FSTNQL1 | 10:29:04 | 07-14-2023 | [5.49GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
```

So they have been removed we can try and load my module again.

```powershell
Import-Module .\output\AzDevOps -Force
Import-Module: The given assembly name was invalid.
```

Same problem. I decided to change how I grab those modules in the psakefile to see if that would help.

```powershell
# JeffreyPatton@FSTNQL1 | 10:33:39 | 07-14-2023 | [5.49GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
Import-Module .\output\AzDevOps -Force
# JeffreyPatton@FSTNQL1 | 10:33:40 | 07-14-2023 | [5.49GB] D:\CODE\projects\Azure-Devops-PowerShell-Module\AzDevOps $  [37-get-a-feed]
```

That did it! I didn't realize that simply using Get-Module -ListAvailable would somehow cause PowerShell to think files were loaded that weren't, but here we are. The quick fix for me was simple.

```powershell
Get-Module -ListAvailable -Name $ModuleName
```

That gave me what I needed without iterating over all modules and potentially causing the previous error.

Hope you have a wonderful Friday!
