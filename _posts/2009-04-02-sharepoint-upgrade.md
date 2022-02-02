---
layout: post
title:  "Sharepoint Upgrade"
date:   2009-04-02 14:00:00 -0600
categories: blog
tags: April 2009 KU SOECS
---
Current Situation:

The SharePoint server which host’s the School of Engineering’s (SOE) intranet has exceeded the Microsoft recommended size for a single server hosting content. The recommendation is that if your data exceeds 5GB that you should move to hosting that content on a SQL server.

Proposed Solution:

Create a new server to host the front end for the SOE intranet and have the data hosted on an existing SQL server.

Impact Statement:

Several things will change as a result of this problem. Currently each tab on SharePoint represents a departmental site and is logically separated from other sites. In order to host the data on SQL these tabs will be separated into individual site collections. A site collection represents a database object on a SQL server.

The main intranet site will become in effect a “portal” to other sites hosted on the server. This change will provide greater flexibility for future growth. We have seen an increase in requests for SharePoint sites and this can now be supported. The intranet will be broken down into categories and each category will be represented by a tab on the main page.

Categories:

Research Projects
Student Projects
Educational Departments
SOE Departments
These categories will become the top navigation bar on the main site and each tab will contain a list of site links below it. Each link is a separate site collection on the SQL server as well as a separate site on SharePoint. Access to these sites will be based on group membership, but everyone will see all site links on the main Engineering portal.

Additionally this change will allow us to provision sites for specific security needs if needed. For example, currently it is very difficult to allow three people the ability to change a document, but only four people the ability to read it. With the new structure in place, a Document Workspace site can be created for a given project and the three people who need to change something would become that sites Members, while the four people who need read access would become that sites Visitors. There is no limit within the foreseeable future on the number of these sites as we can expand the SQL server to store data on the SOE’s Storage Area Network (SAN).

![Sharepoint Accesibility](https://prdwebappstorage.blob.core.windows.net/pattontech/images/SharepointAccessibility.jpg)
