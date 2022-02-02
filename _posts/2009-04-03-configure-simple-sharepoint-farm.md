---
layout: post
title:  "Configure simple Sharepoint farm"
date:   2009-04-03 14:02:00 -0600
categories: blog
tags: April 2009 KU SOECS
---
Requirements:

* Active Directory domain for user authentication
* 1 Server that meets minimum [SharePoint hardware requirements](http://technet.microsoft.com/en-us/library/cc288751.aspx)
* 1 Server that meets minimum [SQL hardware requirements](http://www.microsoft.com/Sqlserver/2005/en/us/system-requirements.aspx)
* Enough CAL’s to support installing Windows Server on each

Windows Setup

Install Windows Server in a default configuration for both servers, install the latest service pack and all updates. The SharePoint server should also have the Application Server Role and SMTP.

Active Directory Configuration

Setting up SharePoint in a farm is slightly more complex than a stand-alone installation. For a simple farm like the one we’re setting up a handful of accounts need to be created in advance. These accounts provide the needed functionality for SharePoint as well as provide required security that most administrators want.

The following accounts should be created as regular domain users with complex passwords:

* Setup Account: This will be the SharePoint local admin and used during installation
  * This account needs to have a login on the SQL instance
* Farm Account: This is the Database Access Account used to connect to SQL
  * This account needs to have the following roles on the SQL instance hosting SharePoint
    * dbCreator
    * SecurityAdmin
* Index Account: This is used by the indexing service on SharePoint
* Content Account: This account is used by the indexing service to search the content

SQL Server Setup

Install SQL onto the server that will become your SQL Server. My preference is to create a named instance for each app that will be connecting to a database, otherwise use the default instance. Make sure that you have set the proper [collation](http://technet.microsoft.com/en-us/library/cc288970.aspx) during SQL setup. Stop all services for your newly create SQL instance before the service pack install to avoid a reboot. Apply the most recent SQL Server service pack from the [Microsoft Download site](http://www.microsoft.com/downloads/en/results.aspx?freetext=sql+service+pack&displaylang=en&stype=s_basic). Then restart the services related to your SQL instance.

SharePoint Setup

Logon to your SharePoint server with the Setup Account you created, you may need to add it to the local Administrator group first. You may also want to add the user accounts from the domain that will be your Farm Administrators to the local Administrators group if they are not Domain Admins.

[Download](http://technet.microsoft.com/en-us/windowsserver/sharepoint/bb400747.aspx) the appropriate build of SharePoint for our preferred architecture. Run the SharePoint.exe from your download location and choose the Advanced option. After setup is complete you may want to download any updates there may be to your computer using [Microsoft Update](http://update.microsoft.com/).

Run the configuration wizard to finish the SharePoint configuration. The Database server will be the name of your SQL server, then a backslash, then the name of your SQL instance, if you created one. The Database name you can leave at the default or change it to something more meaningful. The Database access account is the Farm account you created earlier. This account should also have the dbCreator and SecurityAdmin roles on the instance or the wizard will fail.

You can specify an alternate port number for the Central Administration website, I would recommend you do this otherwise you may forget the random one. For authentication you can leave the default, which is NTLM or you can choose Kerberos. If choosing Kerberos you will need to configure your SPN properly.

The advanced button on the last page of the wizard gives you the option of allowing SharePoint to create users in your domain. I’m not sure what your stance may be on this, but in production that may not be a good idea. Please consult with either your Domain Administrator or Security Administrator if you have questions.

Once everything is defined the installation should progress normally. If things are working properly the final configuration will take a while to complete. If there is a problem logs are stored in the web server extensions folder in Common Files on the drive where SharePoint was installed.

Central Administration Site Configuration

Some things will need to be configured after the setup and configuration wizard complete. You will need to add the user accounts of the SharePoint administrators to the Farm Administrators group. This can be done under the Operations tab. You will need to configure the Search service with the user accounts you defined for Indexing and Content, this can also be done under Operations. Finally you will need to create your initial site, this is done under the Application Management tab. After you have the Administration site and the initial site created you may want to define more friendly names to them, this is done using the Alternate Mappings on the Operations tab.

Site Collections

In order for each site you create to be hosted on a separate content database, you will first need to limit the number of sites that can be created on the initial or portal site. This is done in Application Management, using the Content Databases tool. The value you want to change is the Maximum Number of Sites. This number needs to be larger than the Site Level Warning which can be set to zero.

Once you have defined these values then you will simply add new content databases for each of your sub sites. Each site is accessed through a special URL that is displayed after your default url. The default path is /site/ and you can have as many of these as you want to help define what each site collection contains. Like departmental sites, research sites, organizational sites the list can be as long as you need.
