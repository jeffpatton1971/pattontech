---
layout: post
title:  "DNS and DHCP Implementation"
date:   2008-10-16 11:20:00 -0600
categories: blog
tags: October 2008 KU SOECS
comments: false
---
The School hosts its own Active Directory Infrastructure, this infrastructure depends on a DNS server that fully supports RFC 2136. Our administrative model relies on the ability of our clients to receive their network address from a DHCP server that fully supports RFC 2131. In a production environment we require that the services we rely upon do not implement “draft” or “beta” code.

This is not the state we find ourselves in while using Central IT’s implementation of DHCP and DNS. Their appliance has two serious flaws:

1. TTL’s for all records do not decrement, resulting in stale forward and reverse lookups.
2. The appliance relies on expired DHCP draft to handle failover between servers, which results in the lease files never being updated.

In order to provide a reliable and stable network environment we have decided to provide the requisite services ourselves. We will continue to troubleshoot the various issues with Central IT, but must move forward before the issues get worse. We have also provided a path to migrate back once a stable environment can be provided to the School.

Several steps need to be accomplished before we can roll these services out.

1. Build two servers to host DNS and DHCP and provide redundancy[^1]
   1. Assign IP’s to authorized MAC addresses
   2. Create scopes based on MAC addresses
2. Provide services to each VLAN[^2]
3. Configure DHCP
4. Perform zone transfer for DNS
5. Disable scopes on Central IT appliances
6. Delegate DNS from Central IT down to our DNS server

[^1]:DNS and DHCP services will be provided by two RedHat EL 5.1 virtual machines. We settled on Linux as the built-in services offered by Microsoft did not offer the flexibility we needed.
[^2]:Our virtual servers have all the VLAN’s that comprise the School trunked into their switch. This trunk allows us to deliver an interface into each VLAN for the two virtual servers.