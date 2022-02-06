---
layout: post
title:  "Create VSTS Service Principal"
date:   2017-01-25 15:02:00 -0600
categories: blog
tags: January 2017 Rackspace
---
Working with one of our CSP customer’s and they needed to connect their Visual Studio Team Services account to their CSP Azure account. If you have a regular Pay-As-You-Go subscription, then you have access to the old portal (manage.windowsazure.com) but if you’re a CSP that doesn’t work. So after talking Brian Moore at Microsoft I created a series of steps that I thought I’d get down for the next time I need to do this.

![Step 1](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-step-1.png)

You will need to logon to your Visual Studio Team Services account. As you can see I have logged into mine and I have a couple of projects.

![Step 2](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-step-2.png)

You will need to select a project that you will deploy/integrate with Azure, I’ve selected my sample project.

![Step 3](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-step-3.png)

This step is where you configure the project to connect to VSTS by creating an endpoint.

![Step 4](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-step-4.png)

I’ve given my endpoint an incredibly creative name and associated it with a specific subscription.

![Step 5](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-step-5.png)

Here the endpoint is complete and you have the option to change it’s configuration, manage the endpoint’s role within Azure as well as manage the service principal itself, and finally to disconnect the service principal. The disconnect will in fact delete the service principal from azure, so in production this service principal should only ever be used with Visual Studio.

As a side note, the manage service principal link kicks you over to the old portal, so for CSP customer’s this may in fact fail. See images below.

![Update Service Configuration](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-update-service-configuration.png)

You have a couple of options here, change the connection name, and change the subscription.

![Manage Endpoint Roles](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-manage-endpoint-roles.png)

This will take you to the Azure portal and let you adjust and generally fiddle with the roles associated with this Service Principal.

![Manage Service Principal](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-manage-service-principal.png)

By default this appears to connect you over to the old portal. But this gives you the ability to manipulate the properties of the Service Principal.

![Manage Service Principal New](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-manage-service-principal-new.png)

Here is where you can find the same information in the new portal. This is difficult to see, but Azure Active Directory > App Registrations and then choose the Service Principal named VisualStudioSPN.

![Disconnect](https://prdwebappstorage.blob.core.windows.net/pattontech/images/vsts-disconnect.png)

Finally, to remove the Endpoint and Service Principal, simply choose disconnect, and this will go through and clean everything up.
