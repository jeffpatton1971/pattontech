---
layout: post
title:  "Forwarding Events from WS08 Core DC"
date:   2010-09-22 19:29:00 -0600
categories: blog
tags: September 2010 KU SOECS
comments: false
---
There were some changes between Windows Server 2008 and Windows Server 2008 R2, the one I’m interested in is WinRM. The default HTTP port on Windows Server 2008 is TCP 80, but on Windows Server 2008 R2 the default HTTP port is TCP 5985. There are a couple of ways to get around this; either change the listener port on the Windows Server 2008 machine, or use a Collector Initiated subscription and change the port on the Advanced tab. In my example these computers are both Domain joined. Setting this up in a Workgroup environment is a little different and I may write up something for that later.

### Configuring the Source (Windows Server 2008 Core)

Verify that the WinRM is either on or off

``` dos
winrm e winrm/config/listener
```

![20100922-SourceWinRMDisabled](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-SourceWinRMDisabled.png)

Here you can see that WinRM is not configured. So now we need to enable WinRM

``` dos
winrm qc
```

![20100922-SourceWinRMEnabled](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-SourceWinRMEnabled.png)

The quickconfig (qc) option does the initial configuration of WinRM. It creates an HTTP listener on port 80, and enables firewall exceptions. In order to connect from the Collector and start getting events we also need to allow the remote administration service through the firewall.

``` dos
netsh firewall set service type=remoteadmin mode=enable
```

![20100922-SourceFirewallExceptionsOn](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-SourceFirewallExceptionsOn.png)

The configuration of the source server is done.

### Configuring the Collector (Windows Server 2008 R2)

In order for this server to pull information from the Source you will need to setup a subscription

> Start > Administrator Tools > Server Manager > Diagnostics > Event Viewer > Subscriptions

![20100922-CollectorEnableWCESVC](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-CollectorEnableWCESVC.png)

You will notice that before you can setup a Subscription you need to enable the Windows Event Collector Service, click Yes. Now you can click Create Subscription from the Action pane on the right. At the very least you will need a Subscription name.

![20100922-CollectorSubscriptionProperties](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-CollectorSubscriptionProperties.png)

We are going to configure a Collector initiated subscription, so browse the Directory and find your Source server. If you click the Test button, you may receive an error message that lets you know the Collector can’t talk to the Source. That’s ok, we’re going to fix that in a minute.

![20100922-CollectorSubscriptionError](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-CollectorSubscriptionError.png)

Now we need to configure the list of Events that we are interested in. These are the default events available on any 2008 computer, you can write an XML query that you can paste into the XML tab.

![20100922-CollectorSubscriptionEvents](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-CollectorSubscriptionEvents.png)

The last thing we need to configure is the Protocol settings and Delivery Optimizations. If your Source server is not a Domain Controller then you can add the computer account of the Collector to the Local Administrators group on the Source. If your Source server is a Domain Controller, you may want to use a Service Account.

I set my Event Delivery Optimization to Minimize Latency, this ensures that events are delivered with minimal delay. If you are collecting events from the Security log, this may not be a setting you want to enable.

Finally the Protocol section, here you can change the HTTP port to 80.

![20100922-CollectorSubscriptionAdvancedProperties](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-CollectorSubscriptionAdvancedProperties.png)

After a few minutes you should start to see events showing up under Forwarded Events.

![20100922-CollectorForwardedEvents](https://prdwebappstorage.blob.core.windows.net/pattontech/images/20100922-CollectorForwardedEvents.png)
