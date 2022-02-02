---
layout: post
title:  "Managing Power Savings for XP via GPO"
date:   2009-10-23 14:18:00 -0600
categories: blog
tags: October 2009 KU SOECS
---
You can manage the power savings features of both Windows XP and Vista using a GPO in a domain environment. In order to do this you will need to get Group Policy Preference Client Side Extensions for Windows XP (KB943729). You can download this extension via the following URL:

[Group Policy Preference Client Side Extensions for Windows XP (KB943729)](http://www.microsoft.com/downloads/details.aspx?FamilyID=e60b5c8f-d7dc-4b27-a261-247ce3f6c4f8&DisplayLang=en)

The preferences are already available if you are working with Windows Vista or 7, so the previous download would not be needed for these. You should also be using the Group Policy Management tool which is available as part of the RSAT for Windows Vista and Windows 7, or the Administrative Tools for Windows Server 2003 on XP. These can be downloaded from the following URLs:

[Windows XP](http://www.microsoft.com/downloads/details.aspx?FamilyID=e487f885-f0c7-436a-a392-25793a25bad7&DisplayLang=en)

[Windows Vista](http://www.microsoft.com/downloads/details.aspx?FamilyId=9FF6E897-23CE-4A36-B7FC-D52065DE9960&displaylang=en)

[Windows 7](http://www.microsoft.com/downloads/details.aspx?familyid=7D2F6AD7-656B-4313-A005-4E344E43997D&displaylang=en)

These policies work best against an OU of computers. Depending on how your domain is structured you may need to create an OU for the computers you want to manage Power Savings for, as you cannot apply a GPO to the Computers container. If you already have defined an OU structure for your computers, you should create this as a separate GPO so you can link it to more than one place, and still only need to update one Policy.

I won’t go in to detail on how to create Group Policies or how they are applied; you can find that information on the Technet site at Microsoft.com.

Open up the Group Policy Management Console and create a GPO for Power Management (fig 1.)

[fig1](https://prdwebappstorage.blob.core.windows.net/pattontech/images/fig1.png)

Next you will need to edit your newly created GPO. The Power Management settings can be found in both Computer Configuration and User Configuration (fig 2.). Most of the settings that you would be interested in can be found under Computer ConfigurationPreferencesControl Panel SettingsPower Options. You can see that it is in a similar location for User Configuration.

[fig2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/fig2.png)

Right click and from the popup menu chose either New > Power Options (Windows XP) or New > Power Scheme (Windows XP) (fig 3.). There is also an option to configure a Windows Vista Power Plan. I think of most interest to the average Admin is the Power Scheme. This allows you to define what to turn on or off and when to go to standby.

[fig3](https://prdwebappstorage.blob.core.windows.net/pattontech/images/fig3.png)

This should look very familiar if you have ever configured the power settings in Windows XP before, the only difference would be the actions drop down at the top of the screen, and the Common tab which is related to the GPO itself. I will not go in to the settings found under the Common tab, but needless to say if you wanted to target a specific computer or group of computers you can do it there.

The following table lists what each of the options in the Action drop down do.

| Create | Create a newly configured Power Scheme. If a power scheme with the same name as the Power Scheme item exists, then the existing Power Scheme is not modified. |
| Delete | Remove a Power Scheme with the same name as the Power Scheme preference item. The extension performs no action if the Power Scheme does not exist. |
| Replace | Delete and recreate the named power scheme. The net result of the Replace action overwrites all existing settings associated with the power scheme. If the power scheme does not exist, then the Replace action creates a newly configured power scheme. |
| Update | Modify a power scheme. The action differs from Replace in that it updates the settings defined within the preference item. All other settings remain as they were previously configured. If the power scheme does not exist, then the Update action creates a new power scheme. |

I would like to pass along what I have found regarding defining preferences. This is a relatively new feature for Group Policy what we have found during testing is that these settings tattoo the registry and don’t always get cleanly removed like regular GPO’s do. As always something like this should be tested fully before deployed into production following your own internal roll out procedures.

Below is a list of references for your own edification.

[Group Policy on TechNet](http://technet.microsoft.com/en-us/library/cc754286.aspx)

[Group Policy New Features](http://technet.microsoft.com/en-us/library/cc725828(WS.10).aspx)

[Document describing Group Policy Preferences and Deployment practices](http://www.microsoft.com/downloads/details.aspx?FamilyID=42e30e3f-6f01-4610-9d6e-f6e0fb7a0790&DisplayLang=en)

[Group Policy Survival Guide](http://www.microsoft.com/downloads/details.aspx?familyid=66643D52-BD3D-4B10-972C-316ECA5DBEDF&displaylang=en)
