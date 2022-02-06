---
layout: post
title:  "Windows Server 2012 Single Node Cluster"
date:   2013-04-17 11:01:00 -0600
categories: blog
tags: April 2013 KU IT
---
So this posting is a re-hash of a post that I did over a year ago when Windows 2012 was still Windows Server 8.

``` powershell
PS C:Windowssystem32> Add-WindowsFeature Failover-Clustering

Success Restart Needed Exit Code      Feature Result
------- -------------- ---------      --------------
True    No             Success        {Failover Clustering}
PS C:Windowssystem32> Add-WindowsFeature RSAT-Clustering-PowerShell

Success Restart Needed Exit Code      Feature Result
------- -------------- ---------      --------------
True    No             Success        {Remote Server Administration Tools, Failo...
PS C:Windowssystem32> Import-Module FailoverClusters
PS C:Windowssystem32> Update-Help –Module FailoverClusters
PS C:Windowssystem32> New-Cluster -Name sql -Node sql-om12 -StaticAddress 192.168.1.230
PS C:Windowssystem32> Get-Cluster sql |Format-List *


Domain                                  : company.com
Name                                    : sql
AddEvictDelay                           : 60
BackupInProgress                        : 0
ClusSvcHangTimeout                      : 60
ClusSvcRegroupOpeningTimeout            : 5
ClusSvcRegroupPruningTimeout            : 5
ClusSvcRegroupStageTimeout              : 5
ClusSvcRegroupTickInMilliseconds        : 300
ClusterGroupWaitDelay                   : 120
MinimumNeverPreemptPriority             : 3000
MinimumPreemptorPriority                : 1
ClusterEnforcedAntiAffinity             : 0
ClusterLogLevel                         : 3
ClusterLogSize                          : 300
CrossSubnetDelay                        : 1000
CrossSubnetThreshold                    : 5
DefaultNetworkRole                      : 2
Description                             :
FixQuorum                               : 0
HangRecoveryAction                      : 3
IgnorePersistentStateOnStartup          : 0
LogResourceControls                     : 0
PlumbAllCrossSubnetRoutes               : 0
PreventQuorum                           : 0
QuorumArbitrationTimeMax                : 20
RequestReplyTimeout                     : 60
RootMemoryReserved                      : 4294967295
RouteHistoryLength                      : 10
SameSubnetDelay                         : 1000
SameSubnetThreshold                     : 5
SecurityLevel                           : 1
SharedVolumeCompatibleFilters           : {}
SharedVolumeIncompatibleFilters         : {}
SharedVolumesRoot                       : C:ClusterStorage
SharedVolumeSecurityDescriptor          : {1, 0, 4, 128...}
ShutdownTimeoutInMinutes                : 20
SharedVolumeVssWriterOperationTimeout   : 1800
UseClientAccessNetworksForSharedVolumes : 0
SharedVolumeBlockCacheSizeInMB          : 0
WitnessDatabaseWriteTimeout             : 300
WitnessRestartInterval                  : 15
RecentEventsResetTime                   : 4/17/2013 9:38:07 PM
EnableSharedVolumes                     : Enabled
DynamicQuorum                           : 1
Id                                      : 954f3834-595a-410f-8b2b-67648864d08
```
