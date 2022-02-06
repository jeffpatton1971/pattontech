---
layout: post
title:  "RDP over SSH"
date:   2011-03-17 15:04:00 -0600
categories: blog
tags: March 2012 KU SOECS
---
Before I start, while this will allow you to access your servers over a secure tunnel, this does not mean you should forego patching your systems.

Don’t be that kind of admin, install the patches, install the critical updates, do us all a favor and make your gear as secure as you can.

I know this is not a new topic, but it’s rather new to me. The university has decided to block RDP at the border after the latest RDP [exploit](http://support.microsoft.com/kb/2621440). For the record the university does provide a VPN which will work for most folks, but I don’t often have a machine that I can do that from. The nice thing about putty is it’s a simple download and you don’t have to install it, just download and go.

I’m not going to tell you how to setup an ssh server, mostly because it’s pretty straightforward.

Here we go

1. [Download](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) and start putty
2. Type in your connection information admin@server.company.com
3. Open Connections, SSH, Tunnels
4. Set the source port to be 3391
5. Set the destination port to be rdp-server.company.com:3389
6. Click add, and then open the connection
7. Start the RDP client
8. Make a connection to localhost:3391
9. You may be prompted for all that new connection stuff and then finally credentials

You should now have a connection established to your remote desktop server that is being tunneled through your SSH connection.
