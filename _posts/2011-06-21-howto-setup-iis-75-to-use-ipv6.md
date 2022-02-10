---
layout: post
title:  "HOWTO: Setup IIS 7.5 to use IPv6"
date:   2011-06-14 13:10:00 -0600
categories: blog
tags: June 2011 Personal HOWTO
comments: false
---
I took the SANS 546 class today, and it got me thinking about setting up my server to respond to IPv6 hosts. Steps thus far are pretty straightforward:

1. Get an account with a tunnel broker
2. Configure your host
3. Test connectivity
4. Configure IIS
5. Create AAAA record on your DNS provider
6. Troubleshooting

Tunnel Broker Account

This is very easy and painless! There are several to choose from, but one that was mentioned by the lecturer was [Hurricane Electric](http://www.tunnelbroker.net/). Fill out the form and check your email for your password, the whole process takes about 1 minute. Once you login you will need to create your first tunnel:

1. Login to [http://www.tunnelbroker.net/](http://www.tunnelbroker.net/) with your username and password
2. Click “Create Regular Tunnel”
3. IPv4 endpoint is your webserver
4. They will find a tunnel closest to your IP
5. Click “Create Tunnel”

Configure your host

See? Pretty painless, now that you have your tunnel up you will need to configure your host, since I’m working with Windows 2008 R2, there is actually a set of netsh commands you run. They are specific to your configuration and you can access them by clicking, “Example Configurations” tab on the Tunnel Details page. From the dropdown select your Operating System, and it will give you the commands you need to set it up.

Test connectivity

Once everything is configured the easiest way is attempt to access an IPv6 host:

* Open your browser to [http://ipv6.google.com](http://ipv6.google.com/)
* Or ping –6 ipv6.google.com

Configure IIS

Configuring IIS is pretty simple as well, I found that I had some extra stuff that I didn’t think I should need to do though.

* Open IIS Manager
* Select the site you wish to enable IPv6 on
* From the Action pane choose “Bindings…”
* For basic web server
  * type = http
  * Ip address = ipv6 address
  * port = 80
  * host name = the name you want the server to respond to
* Click Ok

Create AAAA record on your DNS provider

I use [GoDaddy.com](http://www.godaddy.com/) for my dns, so you will just need to go into total dns manager and add the AAAA entry. This entry will need to be the same as the host name you specified in your IIS bindings

Troubleshooting

* The first thing you will want to do is make sure that you are able to ping your own ipv6 address
* Then try pinging your ipv6 address remotely
* Repeat these steps with the ipv6 hostname that you set in DNS.
* It may also be a good idea to visit [test-ipv6.com](http://test-ipv6.com/)
  * If all those tests fail you may have other issues, that I can’t really help you with

What I found is I was able to ping my ipv6 address locally and remotely, my name ipv6.patton-tech.com resolved locally and remotely, but when pointing a browser at that URL nothing showed up. It was the end of the day when I got this setup, and I had done some of the above basic troubleshooting that all returned successful. This morning I began again I ran the following command:

``` powershell
netstat –s
```

The output from this command showed several failed attempts over IPv6, this seemed to increase each time I attempted to open the website (could have been coincidence). Since I saw there were failures on IPv6, the next thing I did was run this command:

``` powershell
netstat –an
```

This should show what addresses are listening on what ports, I saw my IPv4 addresses, but no IPv6. That’s when I started browsing the forums looking for something useful, and I didn’t find much. Most of what I found talked about making sure you didn’t choose the temporary IPv6 address, but since ours is assigned statically via netsh I don’t think that was the problem. Running the command:

``` powershell
netsh interface ipv6 show address
```

Shows that my interface was a tunnel interface, which makes sense, but that got me spun off into checking the firewall, which wasn’t the problem at all. Finally I found a forum post on [iis.net](http://forums.iis.net/p/1177556/1979678.aspx) that was close enough to my issue that I was able to resolve it. One of the posters suggested running this command:

``` powershell
netsh http show iplisten
```

Unlike the original poster I was able to see my IPv4 public address but not my IPv6 address. The suggestion was to remove all iplisten entries which would force iis to listen on all ip addresses. Since I have several services running and listening on port 80, I couldn’t do that. But the syntax of that command led me to [TechNet](http://technet.microsoft.com/en-us/library/cc725882(WS.10).aspx#BKMK_1) for the proper syntax to add a listener:

``` powershell
netsh http add iplisten ipaddress=ipv6addy
```

I [posted](http://forums.iis.net/p/1179286/1986464.aspx#1986464) a question in the forums to ask if there is something I have done wrong, or if perhaps the default is to not add the listener but no answer yet. It seems to me that when I add a binding to iis, it should also allow the web server to listen on that address. I know there was nothing I had to do for IPv4, so it’s either a default (not likely) or the fact that this address is set statically (more likely).
