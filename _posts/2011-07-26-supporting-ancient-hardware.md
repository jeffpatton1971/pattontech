---
layout: post
title:  "Supporting Ancient Hardware"
date:   2011-07-26 13:11:00 -0600
categories: blog
tags: July 2011 KU SOECS
---
Today we’ll be working on some moldy oldies! I give you the SGI Indigo, and it’s successor the SGI Octane! Bow in front of their immense glory!

| ![SGI Indigo2 Circa 1994](https://upload.wikimedia.org/wikipedia/commons/d/d3/Indigo2.jpeg) | ![SGI Octane Circa 1997](https://upload.wikimedia.org/wikipedia/commons/3/35/SgiOctane.jpg) |

Here is the problem:

> We have research analysis software designed solely for SGI, and our main computer for these analysese, the SGI Octane unix computer, OCTANE, will not boot due to apparent hard drive RAM failure. This 10+year old may not be recoverable.
>We can regain our analysis functionality using the functional SGI Indigo 2 computer that has the necessary software, INDIGO. However, the network functionality is not currently available on INDIGO to allow file transfer to/from the computer. Thus, my primary request is that INDIGO be configured to allow secure ftp and remote shell access only.
>I believe the computer needs to be added to the workgroup (or in setup in that “domain”). Please assign someone to complete this network access task in the next few days. Please coordinate with my lab assistant to assure that sftp and secure shell from our PC computers is working.
>Secondarily, if it is staightforward to recover an image of the failed SGI Octane hard drive, we may consider replacing the drive (if possible), restoring the image on the “new” (likely used) drive and getting the SGI Octane, OCTANE working again. This is a lower priority, as there is no substantial data loss, and we have already been looking at ways to move the analyses done on the SGI over to Windows PC software (or possibly Linux). We have no desire to invest large amounts of time and/or money to restore the failed SGI. But, if the process is simple and cost is “cheap”, then we would like to restore the SGI octane until a long-term solution is found.

![SGI_Error_Mesage](https://prdwebappstorage.blob.core.windows.net/pattontech/images/SGI_Error_Mesage.JPG)

So how do you start? Well, we started with one assumption, the SCSI card is dead. If the disk itself is dead, then there is really nothing we can do. Working on our assumption we were able to scrounge up a full length PCI SCSI card.

![WP_001576](https://prdwebappstorage.blob.core.windows.net/pattontech/images/WP_001576.jpg)

We verified that it was recognized in the BIOS and could see an attached disk.

![WP_001574](https://prdwebappstorage.blob.core.windows.net/pattontech/images/WP_001574.jpg)
![WP_001575](https://prdwebappstorage.blob.core.windows.net/pattontech/images/WP_001575.jpg)

After our regularly scheduled lunch (Buffalo Wild Wings…yum) and our staff meeting we headed over to the lab. We had been there once before with Nick, and it wasn’t much fun then either. It was during an IP inventory that we found these machines, and it was on that visit that we configured them to use our network. Sadly the “Supercomputers” were so old, they pre-dated DHCP…but that’s another story. We spoke with the advisor who was unaware that SGI ceased to exist as a company about 8yrs ago, further he was also unaware that SGI stopped using MIPS and switched to Intel based CPU’s about 4yrs before the company filed for bankruptcy.
In the lab we found many things, lovely wiring:

![WP_001577](https://prdwebappstorage.blob.core.windows.net/pattontech/images/WP_001577.jpg)

We also found the sad little Octane, with it’s drive removed. The first thing we noted was that the disk was six years newer than the computer. This was good news for us as we had servers that we could mount that disk into natively. We also “fixed” their network issue, turns out that the Indigo2 had the subnet incorrectly specified, sounds like a stupid mistake, aside from the fact you had to set the network id in hex, ya…you heard me. We gave them the IP of their box and they were able to access it via sftp.

![WP_001578](https://prdwebappstorage.blob.core.windows.net/pattontech/images/WP_001578.jpg)

We informed the advisor that we would attempt to access the disk back in our office, but first we had to make a stop over to our storage room. We picked up a lovely [Dell PowerEdge 2650](http://www.dell.com/downloads/global/products/pedge/en/2650_specs.pdf), and swapped one of it’s disks for the failed SGI disk.

![WP_001580](https://prdwebappstorage.blob.core.windows.net/pattontech/images/WP_001580.jpg)

Upon booting into the BIOS we ran disk utilities and it informed us that the disk was in fact dead. Sadly this means we were unable to fix that part of the problem. But the good news is that we had a nice trip down memory lane playing with hardware that used to cost thousands of dollars, and now is up for sale on [E-bay](http://shop.ebay.com/i.html?_from=R40&_trksid=p5197.m570.l1313&_nkw=sgi+octane&_sacat=See-All-Categories) for about $400. Sorry this has been a rather rambling post, but I felt upon receiving this ticket in the help desk that it really merited some form of posting.
