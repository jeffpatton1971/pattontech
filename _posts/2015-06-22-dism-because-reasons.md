---
layout: post
title:  "DISM...'because reasons'"
date:   2015-06-22 13:01:00 -0600
categories: blog
tags: June 2015 KU IT
---
I don’t know why this is a thing, it shouldn’t be a thing. I’m going to post a link to the page on TechNet, and then just paste in the content.

[https://technet.microsoft.com/en-us/library/dn482069.aspx](https://technet.microsoft.com/en-us/library/dn482069.aspx)

You can use the Deployment Image Servicing and Management (DISM) command-line tool to create a modified image to deploy .NET Framework 3.5.

### Important

For images that will support more than one language, you must add .NET Framework 3.5 binaries before adding any language packs. This order ensures that .NET Framework 3.5 language resources are installed correctly in the reference image and available to users and applications.

In this topic:

* Using DISM with Internet connectivity
* Using DISM with no Internet connectivity

## Using DISM with Internet connectivity

### Requirements

* Internet connection
* Access to Windows Update. If the PC or server is behind a firewall or uses a proxy server, see [KB900935](http://support.microsoft.com/kb/900935) – How the Windows Update client determines which proxy server to use to connect to the Windows Update Web site.
* Windows® 8, Windows Server® 2012, or the [Windows Assessment and Deployment Kit](http://go.microsoft.com/fwlink/?LinkID=325506) (Windows ADK) tools.
* Installation media
* Administrator user rights. The current user must be a member of the local Administrators group to add or remove Windows features.

For an online reference image that can access Windows Update

1. Open a command prompt with administrator user rights (Run as Administrator) in Windows 8 or Windows Server 2012.
2. To Install .NET Framework 3.5 feature files from Windows Update, use the following command:
3. On Windows 8 PCs, after installation .NET Framework 3.5 is displayed as enabled in Turn Windows features on or off in Control Panel. For Windows Server 2012 systems, feature installation state can be viewed in Server Manager.

``` powershell
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All
```

Use /All to enable all parent features of the specified feature. For more information on DISM arguments, see [Enable or Disable Windows Features Using DISM](http://go.microsoft.com/fwlink/?LinkID=259118).

For an offline reference image

Run the following DISM command (image mounted to the c:\test\offline folder and the installation media in the D:\drive) to install .NET 3.5:

``` powershell
DISM /Image:C:\test\offline /Enable-Feature /FeatureName:NetFx3 /All /LimitAccess /Source:D:\sources\sxs
```

> Use /All to enable all parent features of the specified feature.
>
> Use /LimitAccess to prevent DISM from contacting Windows Update/WSUS.
>
> Use /Source to specify the location of the files that are needed to restore the feature.

To use DISM from an installation of the Windows ADK, locate the Windows ADK servicing folder and navigate to this directory. By default, DISM is installed at C:\Program Files (x86)\Windows Kits\8.0\Assessment and Deployment Kit\Deployment Tools\. You can install DISM and other deployment and imaging tools, such as Windows System Image Manager (Windows SIM), on another supported operating system from the Windows ADK. For information about [DISM-supported platforms](http://go.microsoft.com/fwlink/?LinkID=317105), see DISM Supported Platforms.

Run the following command to look up the status of .NET Framework 3.5 (offline image mounted to c:\test\offline):

``` powershell
DISM /Image:c:\test\offline /Get-Features /Format:Table
```

A status of Enable Pending indicates that the image must be brought online to complete the installation.

## Using DISM with no Internet connectivity

You can use DISM to add .NET Framework 3.5 and provide access to the \sources\SxS folder on the installation media to an installation of Windows® that is not connected to the Internet.

Requirements

* Windows 8, Windows Server 2012, or the Windows Assessment and Deployment Kit (Windows ADK) tools.
* Installation media
* Administrator user rights. The current user must be a member of the local Administrators group to add or remove Windows features.

Open a command prompt with administrator user rights (Run as Administrator) in Windows 8 or Windows Server 2012.
To install .NET Framework 3.5 from installation media located on the D: drive, use the following command:

``` powershell
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All /LimitAccess /Source:d:\sources\sxs
```

> Use /All to enable all parent features of the specified feature.
>
> Use /LimitAccess to prevent DISM from contacting Windows Update/WSUS.
>
> Use /Source to specify the location of the files that are needed to restore the feature.

For more information on DISM arguments, see [Enable or Disable Windows Features Using DISM](http://go.microsoft.com/fwlink/?LinkID=259118).

On Windows 8 PCs, after installation, .NET Framework 3.5 is displayed as enabled in Turn Windows features on or off in Control Panel.
