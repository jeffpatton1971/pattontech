---
layout: post
title:  "Delegation of OU and GPO's"
date:   2010-08-13 19:19:00 -0600
categories: blog
tags: August 2010 KU SOECS
comments: false
---
Recently A department within the School wanted to leverage the capabilities that we currently provide many other departments. They currently run their own domain, and they needed the same amount of control if they were to migrate over to our domain. Of course the “easy” button here is the [Delegation of Control Wizard](http://technet.microsoft.com/en-us/magazine/2007.02.activedirectory.aspx). This gives you the ability to grant a user in your domain or another trusted domain the ability to administer all or a portion of the objects within an OU.

Now, while your newly created OU Admin can create user accounts, computer accounts, organizational units and groups, she does not get the ability to create Group Policies. If you gave the account Full Control you can check permissions on the OU you will see that Create and Delete [GroupPolicyContainer](http://technet.microsoft.com/en-us/windowsserver/cc817587.aspx) objects are checked Allowed. When your admin attempts to create a group policy or run the modeling wizard it will fail with “Access is Denied.”

This is because while she has Full Control at the OU level, her account needs to be added to the Group Policy Objects container in the GPMC interface. Once you have added the account there, your admin will now be able to create/edit/delete gpo’s.
