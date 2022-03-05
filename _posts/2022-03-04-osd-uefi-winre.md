---
layout: post
title:  "OSD and UEFI and a missing WinRE"
date:   2022-03-04 20:58:00 -0600
categories: blog
tags: March 2022 Development
author: Jeff
comments: true
published: true
---
So purely by chance I came across a [tweet](https://twitter.com/SeguraOSD/status/1495647079980482562) talking about a new release of OSDCloud. I had no idea what it was so I clicked the links. As far as I can tell [OSDCloud](https://www.osdcloud.com/) allows you to deploy Windows over the internet onto your local computer. So I followed the directions, downloaded all the bits and it failed. I had other things to keep me occupied until today!

Digging in I found one of the functions was returning several drives that already existed, which seemed an odd place to fail. So I started looking around to see if I could find someone else that had the same problem, I was alone, so I did the only other thing I could think of [DM the author](https://twitter.com/SeguraOSD)! I'll be honest I was not expecting a response, let alone a response on the same day. It turns out that 'New-OSDCloudTemplate -WinRE' is looking for the Windows Recovery Image. So time for a little history of my work rig.

I run a [Dell Precision T5500](https://www.dell.com/support/kbdoc/en-us/000138275/precision-t5500-precision-desktop-workstation-specifications) it has a pair of [Intel Xeon X5650](https://www.intel.com/content/www/us/en/products/sku/47922/intel-xeon-processor-x5650-12m-cache-2-66-ghz-6-40-gts-intel-qpi/specifications.html)'s (I have a pair of 3ghz Xeon's sitting in the closet waiting to get the call up) with 144GB of Ram and just to round this summary out, an [Nvidia GeForce GTX 1060](https://www.nvidia.com/en-us/geforce/news/nvidia-geforce-gtx-1060/) video card.

With the recent release of Windows 11 I decided that I wanted to upgrade my desktop. It turns out that while my system can certainly run Windows 11 with ease, the CPU's are not on the list, and my system didn't currently support [Secure Boot](https://support.microsoft.com/en-us/windows/windows-11-and-secure-boot-a8ff1202-c0d9-42f5-940f-843abef64fad). I did what most anyone in my position would do, how do I turn this on without completely wiping and starting over? I found an [article](https://www.windowscentral.com/how-convert-mbr-disk-gpt-move-bios-uefi-windows-10) that was right up my alley. It was this last part that threw the wrench into things.

It turns out that while this process worked, per se, I don't think it worked in a way that you could say, oh this is standard.

``` bash
Get-PartitionWinRE


   DiskPath: \\?\scsi#disk&ven_samsung&prod_ssd#4&2e284e25&0&000000#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                                                                                                    Size Type
---------------  ----------- ------                                                                                                                    ---- ----
1                           1048576                                                                                                                 579 MB IFS


   DiskPath: \\?\scsi#disk&ven_sata&prod_ssd#4&2e284e25&0&000300#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                                                                                                    Size Type
---------------  ----------- ------                                                                                                                    ---- ----
1                K           1048576                                                                                                              238.47 GB IFS


   DiskPath: \\?\scsi#disk&ven_sata&prod_ssd#4&2e284e25&0&000400#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                                                                                                    Size Type
---------------  ----------- ------                                                                                                                    ---- ----
1                L           1048576                                                                                                              238.47 GB IFS


   DiskPath: \\?\scsi#disk&ven_st350063&prod_0ns#4&2e284e25&0&000100#{53f56307-b6bf-11d0-94f2-00a0c91efb8b}

PartitionNumber  DriveLetter Offset                                                                                                                    Size Type
---------------  ----------- ------                                                                                                                    ---- ----
1                D           1048576                                                                                                              465.76 GB IFS
```

One thing worth noting in the output above is the Offset for all output drives is the same. Additionally, only after some discussion with the author, there are more than one partitions output.

``` powershell
Get-ReAgentXml


CustomImageAvailable         : 0
DownlevelWinreLocationGuid   : {00000000-0000-0000-0000-000000000000}
DownlevelWinreLocationId     : 0
DownlevelWinreLocationOffset : 0
DownlevelWinreLocationPath   :
ImageLocationGuid            : {00000000-0000-0000-0000-000000000000}
ImageLocationId              : 0
ImageLocationOffset          : 0
ImageLocationPath            :
InstallState                 : 1
IsAutoRepairOn               : 1
IsServer                     : 0
IsWimBoot                    : 0
NarratorScheduled            : 0
OemTool                      : 0
OperationParam               :
OperationPermanent           : 0
OsBuildVersion               : 19041.1.amd64fre.vb_release.191206-1406
OsInstallAvailable           : 0
PBRCustomImageLocationGuid   : {00000000-0000-0000-0000-000000000000}
PBRCustomImageLocationId     : 0
PBRCustomImageLocationOffset : 0
PBRCustomImageLocationPath   :
PBRImageLocationGuid         : {00000000-0000-0000-0000-000000000000}
PBRImageLocationId           : 0
PBRImageLocationOffset       : 0
PBRImageLocationPath         :
ScheduledOperation           : 5
WinREStaged                  : 0
WinreBCD                     : {90911a1d-8a48-11ea-8a7c-f832c37718cd}
WinreLocationGuid            : {00000000-0000-0000-0000-000000000000}
WinreLocationId              : 1303698096
WinreLocationOffset          : 1048576
WinreLocationPath            : \Recovery\WindowsRE
WindowsRElocation            : \\?\GLOBALROOT\device\harddisk1303698096\partition1\Recovery\WindowsRE
BootConfigurationDataBCD     : 90911a1d-8a48-11ea-8a7c-f832c37718cd
```

The function above is used to determine which drive contains the Windows Recovery Image by matching the WinreLocationOffset to the Offset returned from Get-PartitionWinRE. In an ideal world this would narrow the list to a single partition that may or may not contain a drive letter.

Working with the author he had me open up the module and make a change to just return the first item in the array, which allowed the command to work as intended. I have yet to really give this utility a try, but I will say that I'm a lifelong fan, and to be honest I don't think it takes much anymore for that to happen. Be kind, be responsive, be helpful and let the customer know that you want them to have a positive outcome and that will pay huge dividends.

Maybe I should update my [Week in Review]( {% post_url 2022-03-04-week-in-review %})!
