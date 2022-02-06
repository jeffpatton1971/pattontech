---
layout: post
title:  "Fix Windows Server 2012 R2 DFSR EventID 4614"
date:   2017-01-12 15:01:00 -0600
categories: blog
tags: January 2017 Rackspace
---
Recently had a ticket come in where a newly created Domain with two DC’s was not replicating properly. Upon logging into the DC’s I noted the following log entry in the DFS Replication Log

``` text
The DFS Replication service initialized SYSVOL at local path S:\SYSVOL\domain and is waiting to perform initial replication. The replicated folder will remain in the initial synchronization state until it has replicated with its partner . If the server was in the process of being promoted to a domain controller, the domain controller will not advertize and function as a domain controller until this issue is resolved. This can occur if the specified partner is also in the initial synchronization state, or if sharing violations are encountered on this server or the synchronization partner. If this event occurred during the migration of SYSVOL from File Replication service (FRS) to DFS Replication, changes will not replicate out until this issue is resolved. This can cause the SYSVOL folder on this server to become out of sync with other domain controllers.

Additional Information:
Replicated Folder Name: SYSVOL Share
Replicated Folder ID: C0037E37-EF20-4CDF-968A-932E669ED810
Replication Group Name: Domain System Volume
Replication Group ID: 35F23B97-543A-4310-A08E-5F28D6342C18
Member ID: 41E8F809-5114-48DC-8297-B7E866502101
Read-Only: 0
```

Working backwards through the log I found this entry

``` text
The DFS Replication service encountered an error communicating with partner AD-02 for replication group Domain System Volume.

Partner DNS address: AD-02.contoso.com

Optional data if available:
Partner WINS Address: AD-02
Partner IP Address: 192.168.1.4

The service will retry the connection periodically.

Additional Information:
Error: 1726 (The remote procedure call failed.)
Connection ID: 3E337B81-109F-4A97-880C-63E30F52E63F
Replication Group ID: 35F23B97-543A-4310-A08E-5F28D6342C18
```

I didn’t see that error on AD-02 but I did see several alerts like this one leading up to when the above alert fired on AD-01.

``` text
The DNS server is waiting for Active Directory Domain Services (AD DS) to signal that the initial synchronization of the directory has been completed. The DNS server service cannot start until the initial synchronization is complete because critical DNS data might not yet be replicated onto this domain controller. If events in the AD DS event log indicate that there is a problem with DNS name resolution, consider adding the IP address of another DNS server for this domain to the DNS server list in the Internet Protocol properties of this computer. This event will be logged every two minutes until AD DS has signaled that the initial synchronization has successfully completed.
```

As both DC’s are also DNS servers I configured the binding to be the statically set IP address. Which should once and for all rule out any sort of weird resolution issues. This had no affect either. There is not a lot else to go on from either server around the date where I think things went sideways. There is an odd 15 day gap in the DFSR Log, but maybe that’s normal? I don’t really see any indication of a problem in the Directory Services, System or Application logs around that time.

I then installed the [Active Directory Replication Status Tool](https://www.microsoft.com/en-us/download/details.aspx?id=30005), while it was interesting to run there were no issues reported. The next thing I ran was the health report from the DFS Management console, which told me more or less what I already knew, DFS was waiting on initial synchronization.

So the next thing to try was to figure out how to force that synchronization to happen. I found the following [article](https://support.microsoft.com/en-us/kb/2218556) on Microsoft’s Support site. I ran through the Authoritative Synchronization steps and after going through that the servers were happy. I was able to drop a test file in SYSVOL and see it replicate to the other server, delete that file on the server and see it drop off the other server.

While I don’t really know what the root cause for this issue was, hopefully the next time it crops up I’ll be able to figure it out a lot more quickly. As a side note to the steps I followed below I would tell you that I restarted the DFSR service after making changes in ADSI, each time.

Prior to running dfsrdiag, you may need to install that feature

``` powershell
Add-WindowsFeature -Name RSAT-DFS-Mgmt-Con
```

In order to force Active Director Replication throughout the domain you will run this command

``` powershell
repadmin /syncall /APed
```

``` text
You want to force the non-authoritative synchronization of SYSVOL on a domain controller. In the File Replication Service (FRS), this was controlled through the D2 and D4 data values for the Burflags registry values, but these values do not exist for the Distributed File System Replication (DFSR) service. You cannot use the DFS Management snap-in (Dfsmgmt.msc) or the Dfsradmin.exe command-line tool to achieve this. Unlike custom DFSR replicated folders, SYSVOL is intentionally protected from any editing through its management interfaces to prevent accidents.
```

### How to perform a non-authoritative synchronization of DFSR-replicated SYSVOL (like “D2” for FRS)

1. In the ADSIEDIT.MSC tool modify the following distinguished name (DN) value and attribute on each of the domain controllers that you want to make non-authoritative:
   1. CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=servername,OU=Domain Controllers,DC=domainname
   2. msDFSR-Enabled=FALSE
2. Force Active Directory replication throughout the domain.
3. Run the following command from an elevated command prompt on the same servers that you set as non-authoritative:
   1. DFSRDIAG POLLAD
4. You will see Event ID 4114 in the DFSR event log indicating SYSVOL is no longer being replicated.
5. On the same DN from Step 1, set:
   1. msDFSR-Enabled=TRUE
6. Force Active Directory replication throughout the domain.
7. Run the following command from an elevated command prompt on the same servers that you set as non-authoritative:
   1. DFSRDIAG POLLAD
8. You will see Event ID 4614 and 4604 in the DFSR event log indicating SYSVOL has been initialized. That domain controller has now done a “D2” of SYSVOL.

### How to perform an authoritative synchronization of DFSR-replicated SYSVOL (like “D4” for FRS)

1. Stop the DFSR service on all domain controllers
2. In the ADSIEDIT.MSC tool, modify the following DN and two attributes on the domain controller you want to make authoritative (preferrably the PDC Emulator, which is usually the most up to date for SYSVOL contents):
   1. CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=servername,OU=Domain Controllers,DC=domainname
   2. msDFSR-Enabled=FALSE
   3. msDFSR-options=1
3. Modify the following DN and single attribute on all other domain controllers in that domain:
   1. CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=servername,OU=Domain Controllers,DC=domainname
   2. msDFSR-Enabled=FALSE
4. Force Active Directory replication throughout the domain and validate its success on all DCs.
5. Start the DFSR service set as authoritative:
6. You will see Event ID 4114 in the DFSR event log indicating SYSVOL is no longer being replicated.
7. On the same DN from Step 1, set:
   1. msDFSR-Enabled=TRUE
8. Force Active Directory replication throughout the domain and validate its success on all DCs.
9. Run the following command from an elevated command prompt on the same server that you set as authoritative:
   1. DFSRDIAG POLLAD
10. You will see Event ID 4602 in the DFSR event log indicating SYSVOL has been initialized. That domain controller has now done a “D4” of SYSVOL.
11. Start the DFSR service on the other non-authoritative DCs. You will see Event ID 4114 in the DFSR event log indicating SYSVOL is no longer being replicated on each of them.
12. Modify the following DN and single attribute on all other domain controllers in that domain:
    1. CN=SYSVOL Subscription,CN=Domain System Volume,CN=DFSR-LocalSettings,CN=servername,OU=Domain Controllers,DC=domainname
    2. msDFSR-Enabled=TRUE
13. Run the following command from an elevated command prompt on all non-authoritative DCs (i.e. all but the formerly authoritative one):
    1. DFSRDIAG POLLAD
