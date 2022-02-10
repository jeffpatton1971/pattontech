---
layout: post
title:  "Defrag is not defragging"
date:   2012-02-06 15:02:00 -0600
categories: blog
tags: February 2012 KU SOECS
comments: false
---
I [posted](https://social.technet.microsoft.com/Forums/en-US/8d243252-05eb-4612-ae41-64eab522a2f5/increased-fragmentation-after-defrag?forum=winserverfiles) a question in the forums the other day after attempting to defrag one of the drives on my file server. The drive in question is where we have all the application installation files for the School stored.
This particular drive was quite low on free space, so I used the following switches in an attempt to force the defrag to happen anyway.

* W – Performs full defragmentation. Attempts to consolidate all file fragments, regardless of their size.
* F – Forces defragmentation of the volume when free space is low.
* V – Specifies verbose mode. The defragmentation and analysis output is more detailed.

``` dos
C:Usersjeffpatton.admin>defrag s: -w -f -v
Windows Disk Defragmenter
Copyright (c) 2006 Microsoft Corp.
Defragmentation report for volume S: Software Drive
    Volume size                         = 512 GB
    Cluster size                        = 4 KB
    Used space                          = 450 GB
    Free space                          = 62.40 GB
    Percent free space                  = 12 %
File fragmentation
    Percent file fragmentation          = 55 %
    Total movable files                 = 1,716,060
    Average file size                   = 605 KB
    Total fragmented files              = 1,536
    Total excess fragments              = 1,363,219
    Average fragments per file          = 1.85
    Total unmovable files               = 11
Free space fragmentation
    Free space                          = 62.40 GB
    Total free space extent             = 1,013,736
    Average free space per extent       = 65 KB
    Largest free space extent           = 85 MB
Folder fragmentation
    Total folders                       = 127,411
    Fragmented folders                  = 1
    Excess folder fragments             = 2,819
Master File Table (MFT) fragmentation
    Total MFT size                      = 1.70 GB
    MFT record count                    = 1,777,742
    Percent MFT in use                  = 99
    Total MFT fragments                 = 3
    Note: On NTFS volumes, file fragments larger than 64MB are not included in the fragmentation statistics
Defragmentation report for volume S: Software Drive
    Volume size                         = 512 GB
    Cluster size                        = 4 KB
    Used space                          = 450 GB
    Free space                          = 62.40 GB
    Percent free space                  = 12 %
File fragmentation
    Percent file fragmentation          = 70 %
    Total movable files                 = 1,716,060
    Average file size                   = 605 KB
    Total fragmented files              = 1,533
    Total excess fragments              = 1,363,206
    Average fragments per file          = 1.85
    Total unmovable files               = 11
Free space fragmentation
    Free space                          = 62.40 GB
    Total free space extent             = 1,012,697
    Average free space per extent       = 65 KB
    Largest free space extent           = 85 MB
Folder fragmentation
    Total folders                       = 127,411
    Fragmented folders                  = 1
    Excess folder fragments             = 2,819
Master File Table (MFT) fragmentation
    Total MFT size                      = 1.70 GB
    MFT record count                    = 1,777,742
    Percent MFT in use                  = 99
    Total MFT fragments                 = 3
    Note: On NTFS volumes, file fragments larger than 64MB are not included in the fragmentation statistics
```

Well, here it is the next day and defrag finished sometime during the night. You will notice now that file fragmentation is where it should be. My assumption from this is that while you can choose the –F option, there is most likely a threshold at which even that won’t work.

``` dos
C:Usersjeffpatton.admin>defrag s: -w -f -v
Windows Disk Defragmenter
Copyright (c) 2006 Microsoft Corp.
Defragmentation report for volume S: Software Drive
    Volume size                         = 640 GB
    Cluster size                        = 4 KB
    Used space                          = 445 GB
    Free space                          = 195 GB
    Percent free space                  = 30 %
File fragmentation
    Percent file fragmentation          = 45 %
    Total movable files                 = 1,717,746
    Average file size                   = 607 KB
    Total fragmented files              = 446
    Total excess fragments              = 1,159,235
    Average fragments per file          = 1.72
    Total unmovable files               = 11
Free space fragmentation
    Free space                          = 195 GB
    Total free space extent             = 995,580
    Average free space per extent       = 206 KB
    Largest free space extent           = 84.77 GB
Folder fragmentation
    Total folders                       = 127,414
    Fragmented folders                  = 1
    Excess folder fragments             = 0
Master File Table (MFT) fragmentation
    Total MFT size                      = 1.70 GB
    MFT record count                    = 1,778,403
    Percent MFT in use                  = 99
    Total MFT fragments                 = 3
    Note: On NTFS volumes, file fragments larger than 64MB are not included in the fragmentation statistics
Defragmentation report for volume S: Software Drive
    Volume size                         = 640 GB
    Cluster size                        = 4 KB
    Used space                          = 445 GB
    Free space                          = 195 GB
    Percent free space                  = 30 %
File fragmentation
    Percent file fragmentation          = 0 %
    Total movable files                 = 1,717,746
    Average file size                   = 607 KB
    Total fragmented files              = 7
    Total excess fragments              = 305,897
    Average fragments per file          = 1.19
    Total unmovable files               = 11
Free space fragmentation
    Free space                          = 195 GB
    Total free space extent             = 659,294
    Average free space per extent       = 311 KB
    Largest free space extent           = 2.67 GB
Folder fragmentation
    Total folders                       = 127,414
    Fragmented folders                  = 1
    Excess folder fragments             = 0
Master File Table (MFT) fragmentation
    Total MFT size                      = 1.70 GB
    MFT record count                    = 1,778,403
    Percent MFT in use                  = 99
    Total MFT fragments                 = 3
    Note: On NTFS volumes, file fragments larger than 64MB are not included in the fragmentation statistics
```
