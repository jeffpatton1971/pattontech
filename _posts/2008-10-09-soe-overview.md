---
layout: post
title:  "SOE Overview"
date:   2008-10-09 11:18:00 -0600
categories: blog
tags: October 2008 KU SOECS Overview
comments: false
---
I have been with the School of Engineering since February of 2007 and in that time we have moved from technology that was 1995 centric to an almost current level of technology. This overview should provide some insight into how we have grown, what caused us to move in the direction we did, and where we see ourselves in the future. This document will forego an explanation of the eccentricities of KU as the author himself still struggles with them.

The School provides services to the 2000+ engineering students and staff/faculty at the University of Kansas. We provide Helpdesk support for the over 100 applications we deliver to a given student, as well as technical support in the case of hardware issues.

When I first started computers were “imaged” using Symantec Ghost product. I use the term imaged generously as the computers while imaged were never properly imaged. The School is primarily a Microsoft shop, so the standard desktop OS is Windows. To support that environment we have a separate AD infrastructure from the University at large.

In order for a computer to be properly joined to any Active Directory Domain it needs a unique SID. The tool used for this purpose is sysprep.exe, which basically forces the computer into a pre-setup mode in which everything can be recreated.

In the case of the School’s SOP, a “master” computer would be installed with all requisite software and verified to function. This would also entail joining said computer to the proper domain and verified that logons would also work as intended. Without any other configuration this machine would then be ghosted to a server, applications, current Windows settings (NETBios name) and all.

The image would then pushed to the lab in a very manual way. A cart would be wheeled into the lab complete with server, one or more switches and enough cables to get to each client. Each client would be booted into the server and imaging would commence once all clients were connected. After that the computers would reboot, the computer would be renamed and that would be it.

Literally.

The end result of this process was an environment in which any kind of management, remote or otherwise, was virtually impossible.

If a new application was requested to be available in the labs, that application would be rolled out manually. It’s unclear as to wether or not a given application underwent any sort of compatibility testing, but in looking at what historical data there is, it appears that the bulk of applications were just installed. This process would be carried out manually on each and every computer where the software was needed. If it was something that should be available to all computers that would require installing the application on over 250 separate computers.

Due to the nature of how things were configured problems would crop up on a regular basis. The majority of the fixes almost always entailed using a batch file which would call some arcane NT command or would modify the registry with the reg.exe command. While the batch files were more or less standardized the nature of things here was such that really only a handful of computers could successfully run them against computers.

Historically engineering students were provided logon accounts to the engineering domain and this practice ended long before I arrived. All students are provided with the ability to create a domain account in the University’s domain. It is these accounts that the students use to logon to the computers anywhere on campus. Under the previous administration a one-way trust was established between the University domain and the School domain, to provide authentication.

If you are keeping track of things this more or less places us into an NT master/slave multi-domain arrangement.

As the students can logon to most any computer in the School the need to centralize their profile became mandatory. It was understood at that time that the ability to have a roaming profile between two domains in different forest’s was impossible. So a login batch file was created that would map drives for the students, map printers (using reg.exe) and manage their “profile” using robocopy.exe.

The process went this way:

1. Student logs in
2. All drives are unmounted
3. If the user registry file exists on the network it is reg.exe imported
4. Robocopy.exe copies everything from the server to the local drive
5. Based on an Environment variable the printers are mapped using reg.exe add
6. Drives are mapped using net.exe /use
7. Student logs out
8. Robocopy.exe copies data back to the server
9. The HKEY_CU is reg.exe exported to a file and copied up to the server

I can give no good reason as to why everything did NOT blow up at this point other than freak luck. This is the environment that was in place for a significant amount of time. This is NOT the way things should be in any network and when I took over administration that Summer I worked on changing that.

Main campus IT provides a large number of services and my goal has been to avail ourselves of those services whenever possible. To that end as new services became available I express our interest in testing them out in our environment. My goal then is at some point move us into a position in which the only services we provide are services that are wholly unique to the School. In addition, I want the administration of this environment to be as automated as possible, leveraging all the technology that we currently posses to achieve this end.

The first order of business was to replace Ghost with a process that was potentially free of manual intervention. I make no excuses, I am most definitely a Microsoft fan and with that in mind it made sense to me that Remote Installation Services was exactly what we needed to have.

In most environments there are some infrastructure services that just go, namely DNS and DHCP. The requisite DNS structure was in place to allow RIS to work, but the campus had a homegrown DHCP implementation. This service was based soley on LDAP and for the most part, it just worked. But in order for RIS to be truly automated the DHCP service needed to provide PXE support, this was either not possible or not available to me. So I resorted to a rogue Microsoft DHCP server that was limited by MAC addresses.

The first round of imaging went surprisingly well, unlike Ghost, which in our environment if you choose the wrong option the network goes away, RIS was surprisingly light on the network. In fact during testing we were asked if we were even doing anything while in the middle of imaging 10 computers! There were some planning and logistical issues that I was unprepared for, but they were well documented and did not show up in subsequent imaging sessions.

With that process more or less in hand it was time to tackle application management. I started working on a process that would allow the staff/faculty to request software be installed at their leisure. The idea being that with proper planning and testing, we will deliver any application you want, wherever you need it. In that respect we are actually making headway within the School.

There was no built-in application that could do this for us from Microsoft, so I turned to AdminStudio. This product allows us to take a given application and create a custom installation package that can be either manually installed or pushed out over the network. This product is now in place and creating and we have significantly reduced the amount of help requests caused by poorly implemented software.

Getting us to that point was only half of the application management plan. We also needed the ability to deliver an application at will to a given client with no manual intervention. The first solution was Group Policies. GPO’s allowed us to dictate based on OU what computers had which applications installed. While we had no reporting capabilities this did provide us the ability to “push” software to clients.

A better solution was found through Microsoft System Center. We have used one product, Data Protection Manager, for quite some time. This software backs up our 7TB storage array to disk with an option for long-term storage to tape. We have been very pleased with this product as it is incredibly intuitive and easy to use. My only complaint is that outside of scripting, there is no way to manage it unless you RDP into the server.

System Center Configuration Manager was the next product we looked at. This allows us to push applications with much more granular detail. We now have the ability to push an application to one or more computers with absolutely no interaction at all. In addition we have replaced RIS/WDS with SCCM Operating System deployment. We can literally push out the OS to a client as though it were an application! We also now have metering data, and can tell management exactly how often a given application is run! We plan on using this data to help eliminate that vast software duplication that we currently experience within the School.

We are now in a current state of operation within the School. Most of the day-to-day administration is done using either VBscripts or Powershell scripts. Sane group policies are now in place to manage all aspects of our users experience.  We are very close to the idea of “Single Sign on” within the School, and this is actually very exciting for us!

In addition to everything has happened at more or less the Application Layer of the network, we have had significant modification to the underlying network infrastructure. We are still waiting for an underlying hardware overhaul to happen, but we have seen some major enhancements to other areas of the network.

When I first arrived we had at most a 100MB outbound connection to the main campus, this is now a 1GB connection. All the servers in the rack shared a single port out to the rest of the network, now each server has a dedicated 1GB port to the rest of the network, this alone has greatly reduced the strain that we have seen on our storage array.

The outdated DHCP service was replaced, with a marginally functional DHCP/DNS appliance. We have had very few problems with the DNS appliance, there has been some general silliness for having a very specific configuration, but we have been able to work within those rules and DNS for the most part just works. Currently our issue is with the DHCP appliance, historically most organizations within the University used DHCP Reservations for all of their hosts. In an effort to eliminate the amount of work required to setup and configure computers we have decided that we need to use DHCP and DDNS for the purposes for which they were intended.

We are struggling with a vendor that seems like they are using us for beta testing their product. Things that on any other DHCP implementation would just work, are just broken with this implementation. I hold out hope that at some point in the not-to-distant future this problem will be resolved, but I do have a backup plan ready to go if needed.

When I first arrived the School’s network was one large VLAN that had been assigned close to 2,000 IP addresses. We have now divided the School’s network into separate and distinct VLAN’s, each with a specific purpose and an appropriate amount of private IP’s. This has greatly reduced the amount of transient traffic we had been seeing for quite some time. This now gives us a much leaner network than we have ever had, which means it performs better and we are better able to actually manage it, to the extent we are able.

We now also have a firewall in place. This was something that we had not had until quite recently. The School had been very fortunate considering the only thing protecting the bulk of our computers had been the built-in Windows Firewall. The main campus IT had recently upgraded to a new firewall product which was the primary reason for why we needed VLAN’s. Now we have a perimeter defense as well as a much improved client firewall policy that locks out everything else.
