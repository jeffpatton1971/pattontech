---
layout: post
title:  "Hacking Sharepoint"
date:   2009-04-18 14:05:00 -0600
categories: blog
tags: April 2009 KU SOECS
---
LOOK & FEEL CONFIGURATION

---
The Look and Feel of SharePoint can be changed by modifying the assorted .master files that can be found in the “C:/Program Files/Common Files/Microsoft Shared/web server extensions” directories. The two main master files are application.master and default.master.

The application.master file controls the look of application pages, these pages are common to all sites and libraries, the main example is the one found by clicking “View All Site Content” in the left menu.

The default.master file controls the look of all of the other content pages that a site uses. The main file on disk is copied into each site when it is created, this allows each site to have it’s own look and feel. However, this also means that if the default.master file is changed it needs to be updated on every site. The simplest way to sync all of the default.master files is to use the Microsoft Office SharePoint Designer (formerly FrontPage), in this product when you find the default.master file in a site you can revert to the main default.master rather than the site’s current version.

The styling for the SharePoint site can be found in the Dropbox. This zip file contains the modified application.master, default.master, and KU images and style folder. The folder can be copied into the Images directory at:

> Program Files/Common Files/Microsoft Shared/web server extensions/12/TEMPLATE/IMAGES

The application.master can be copied into the Layouts directory at:

> Program Files/Common Files/Microsoft Shared/web server extensions/12/TEMPLATE/LAYOUTS

The default.master file can be copied into the Global directory at:

> Program Files/Common Files/Microsoft Shared/web server extensions/12/TEMPLATE/GLOBAL

Once these files and folders are in place all new sites created will utilize the KU styling included.

BROKEN MICROSOFT-ISM

---
When applying a SSL key to a given SharePoint site you must fix all of the .css files used by the site themes to not refer to:

> background-image:url(“<http://localhost:2415/topnavhover_simple.gif>”)

Otherwise, IE will complain about unsecured content each time a page is loaded.

Simply remove the <http://localhost:2415> as these files are never actually served to the clients. Yes you read that right, the site wide CSS style sheets tell the Enterprise SharePoint Product to refer to files on the clients computer.

Also, you will need to apply the “updated” theme to each site in the entire SharePoint install to make all of the errors go away.

This content was proudly stolen from Nick, I removed the rants from the Unix guy against Microsoft. Keep in mind we wouldn’t have this information if it wasn’t for his wonderful tweaky adherences to standards!
