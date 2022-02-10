---
layout: post
title:  "Managing your network interfaces for VMware Player"
date:   2010-09-21 19:27:00 -0600
categories: blog
tags: September 2010 Personal
comments: false
---
If you need to reconfigure your virtual network, you will need the vmnetcfg.exe utility. This is part of the VMware Player download, but you have to run the installer such that it only extracts it’s files. Once you have extracted the files to a temporary folder, you will then need to extract the network.cab file. This cabinet file contains almost everything you need.

In the temporary folder that contains all the VMware player files, you will need to copy the following files to the same location you extracted the network.cab file to.

* sigc-2.0.dll
* vmwarebase.dll
* libxml2.dll
* iconv.dll
* zlib1.dll
* vmwarecui.dll
* glib-2.0.dll
* intl.dll
* gobject-2.0.dll
* glibmm-2.4.dll
* gmodule-2.0.dll
* vmwarestring.dll
* libcurl.dll
* ssleay32.dll
* libeay32.dll
* libldap_r.dll
* liblber.dll
* vmwarewui.dll
* vixdiskmountapi.dll
* sysimgbase.dll
* vnetlib.dll

Once all the supporting files are in the same folder as the vmnetcfg.exe file, you will be able to manually adjust the virtual network adapters.
