---
layout: post
title:  "Splitting a single Sharepoint Site"
date:   2009-04-18 14:04:00 -0600
categories: blog
tags: April 2009 KU SOECS
---
DISCLAIMER

---
This is the method we chose, this is by no means the best method nor is it the only method but quite simply the method we chose.

OUR SCENARIO

---
We deployed SharePoint to initially fix a glitch. We had several departments within the School and across campus that continually needed access to a file share on the server. This worked for some time but then they wanted to be able to do more with it, access it over the web, collaborate on documents. This seemed a task for which SharePoint was ideally suited.

We performed a basic single server deployment and from there proceeded to delve into a level of SharePoint hackery that no-one should ever undertake. If you would like details you can find them in this [article]({% post_url 2009-04-18-hacking-sharepoint %}). over the span of a year we saw significant growth in the user of SharePoint, albeit mostly in a file-serving capacity but that was expected.

We are in a position now where we need clean everything up. Initially we shot ourselves in the foot by providing extraordinarily tweaky sets of permissions and it has become increasingly difficult to prevent users from accessing data they shouldn’t. Our solution is to try and get back to the three roles in SharePoint, Owner, Member and Visitor. Additionally we wanted to have more flexibility in terms of storage for the databases.

PRE-REQUIREMENTS

---
Make sure that both the new SharePoint server and it’s SQL server backend are configured properly, see [article](2009-04-03-configure-simple-sharepoint-farm) for details on how to do this.

Alert staff that the intranet may be down for several hours to several days depending on the size of your content and your personal level of tweakiness.

Prevent users from inadvertently accessing the SharePoint site, several ideas come to mind the easiest may be to just unplug the server from the network during the backup. Once the backup is complete disable the IIS service and copy the backup file wherever it needs to go.

BACKUP EXISTING SITE

---
Farm Administrator Account

I have had a lot of success using stsadm to perform this type of migration. There may be other ways that are better, but this route seems to work best for me. Logon to the existing SharePoint server and open the console. You will need to use the stsadm utility, if it’s not in your path it can be found by searching under C:Program FilesCommon Files.

In order for the backup to successfully complete you will need to know that URL for your site, if you are unsure you should check in the SharePoint Central Administration site. For this example the name of the site is <https://sharepoint.company.com>. So you would type the following at the command line:

```powershell
stsadm -o backup -url https://sharepoint.company.com -filename SharePointSite.bak
```

* Depending on the size of the content it may take quite a while for the backup to be complete.

CREATE UPLOAD SITE

---
Farm Administrator Account

On the new SharePoint server you will want to create a new site that is blank that has unique permissions for only your user account. Additionally you may want to prevent from showing up in any of the normal navigational links on the site. This will be your work area, call it whatever you want it will be completely deleted when we’re done.

RESTORE SITE

---
Farm Administrator Account

The restore can be done over an smb share if you like, or by copying the previously created backup file onto the new server. Depending on your network it may be a good idea to copy the file to the new server and run from there. There will be a performance impact on the SQL server if you have a significant amount of data, so if other services use the same SQL server you may want to find a time when this will be less of an impact.

Logon on to the new SharePoint server and open a command shell, we will use stsadm to perform a restore of the existing content into a blank site. Assuming you are using <https://intranet.company.com> for the new site and created a blank site called temp you would enter the following at the command line:

```powershell
stsadm -o restore -url https://intranet.company.com/temp -filename SharePointSite.bak -overwrite
```

* Depending on the size of the content it may take quite a while for the restore to be complete.

Once the restore is complete you will have your existing site living under your new work area on the server. This site’s content should no be living on the SQL server.

SPLITTING THE SITES

---
Site Owner Account
Farm Administrator Account

This process will be repeated for each site you wish to create. Basically you delete all the sites but the one you want to keep. Then you backup the newly created site to a file, delete the entire site in SharePoint and start all over from the Restore site heading.

RESTORE SITES

---
Farm Administrator Account
Site Owner Account

The end result of the previous steps leaves you with individual backups of the sites you want to separate into their own databases. Using your Farm Administrator Account you will create a new site. In the SharePoint Central Administration site, you will need to set the minimum and maximum number of sites under the Content Databases page. In our arrangement there will be one “Portal” site. This will basically have a list of all the sub sites hosted on the server.

For each site behind the Portal site you will need to create a Content Database using the SharePoint Central Administration site. These databases will be stored on the SQL server which provides us the the flexibility we need if a site’s content grows larger than the server’s disk space. This also allows us to create a separate backup for each site and the ability restore one site without affecting any of the other sites on the server.

Once the databases are created you will need to perform a restore of that sites backup file to its new home on the SharePoint server.

```powershell
stsadm -o restore -url https://intranet.company.com/sites/HRDept -filename HRDeptSharePoint.bak -overwrite
```

You will perform the Content Database creation and restore operation for each site you will be hosting on the server. Each of these sites will be given the default permission of the Site Owner Account being set as the only user.

DEFINING SITE PERMISSIONS

---
In our environment we have created departmental security groups which we can use in each site’s Member’s role. Usually this is an adequate setting, but using this structure we can define in the Visitor’s role other department’s or individual’s who need read access to that particular site.

This arrangement now allows us to define groups and sites when special requests are needed to allow cross-departmental collaboration or read-only sites.
