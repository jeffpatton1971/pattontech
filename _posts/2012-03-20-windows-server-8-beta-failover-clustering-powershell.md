---
layout: post
title:  "Windows Server 8 Beta Failover Clustering and PowerShell"
date:   2011-03-20 15:07:00 -0600
categories: blog
tags: March 2012 KU SOECS
comments: false
---
So the last two posts ([one]({% post_url 2012-03-20-windows-server-8-beta-failover-clustering-part1 %}), [two]({% post_url 2012-03-20-windows-server-8-failover-clustering-part2 %})) were just some screenshots and comments as I went through and created a failover cluster. To be fair this wasn’t the first go round with the cluster I created one earlier with just one computer so I could see the PowerShell stuff.

I must say, 81 PowerShell commands to handle clustering, not too shabby. The first cluster I created was with the New-Cluster cmdlet.

``` powershell
New-Cluster -Name win8-hv -Node win8-hv1 -NoStorage -Verbose
Name
----
win8-hv
```

The progress bar flashed for a bit as it did stuff and then there was a cluster. It didn’t take a long time but it was rather hot I must say.

Here are all the new commands

``` powershell
Get-Command |Where-Object {$_.ModuleName -eq 'FailoverClusters'} | Format-Table -Property Capability, Name -AutoSize

Capability Name
---------- ----
    Cmdlet Add-VMToCluster
    Cmdlet Remove-VMFromCluster
    Cmdlet Add-ClusterCheckpoint
    Cmdlet Add-ClusterDisk
    Cmdlet Add-ClusterFileServerRole
    Cmdlet Add-ClusterGenericApplicationRole
    Cmdlet Add-ClusterGenericScriptRole
    Cmdlet Add-ClusterGenericServiceRole
    Cmdlet Add-ClusterGroup
    Cmdlet Add-ClusteriSCSITargetServerRole
    Cmdlet Add-ClusterNode
    Cmdlet Add-ClusterPrintServerRole
    Cmdlet Add-ClusterResource
    Cmdlet Add-ClusterResourceDependency
    Cmdlet Add-ClusterResourceType
    Cmdlet Add-ClusterScaleOutFileServerRole
    Cmdlet Add-ClusterServerRole
    Cmdlet Add-ClusterSharedVolume
    Cmdlet Add-ClusterVirtualMachineRole
    Cmdlet Add-ClusterVMMonitoredItem
    Cmdlet Block-ClusterAccess
    Cmdlet Clear-ClusterDiskReservation
    Cmdlet Clear-ClusterNode
    Cmdlet Get-Cluster
    Cmdlet Get-ClusterAccess
    Cmdlet Get-ClusterAvailableDisk
    Cmdlet Get-ClusterCheckpoint
    Cmdlet Get-ClusterGroup
    Cmdlet Get-ClusterLog
    Cmdlet Get-ClusterNetwork
    Cmdlet Get-ClusterNetworkInterface
    Cmdlet Get-ClusterNode
    Cmdlet Get-ClusterOwnerNode
    Cmdlet Get-ClusterParameter
    Cmdlet Get-ClusterQuorum
    Cmdlet Get-ClusterResource
    Cmdlet Get-ClusterResourceDependency
    Cmdlet Get-ClusterResourceDependencyReport
    Cmdlet Get-ClusterResourceType
    Cmdlet Get-ClusterSharedVolume
    Cmdlet Get-ClusterVMMonitoredItem
    Cmdlet Grant-ClusterAccess
    Cmdlet Move-ClusterGroup
    Cmdlet Move-ClusterResource
    Cmdlet Move-ClusterSharedVolume
    Cmdlet Move-ClusterVirtualMachineRole
    Cmdlet New-Cluster
    Cmdlet Remove-Cluster
    Cmdlet Remove-ClusterAccess
    Cmdlet Remove-ClusterCheckpoint
    Cmdlet Remove-ClusterGroup
    Cmdlet Remove-ClusterNode
    Cmdlet Remove-ClusterResource
    Cmdlet Remove-ClusterResourceDependency
    Cmdlet Remove-ClusterResourceType
    Cmdlet Remove-ClusterSharedVolume
    Cmdlet Remove-ClusterVMMonitoredItem
    Cmdlet Repair-ClusterSharedVolume
    Cmdlet Reset-ClusterVMMonitoredState
    Cmdlet Resume-ClusterNode
    Cmdlet Resume-ClusterResource
    Cmdlet Set-ClusterLog
    Cmdlet Set-ClusterOwnerNode
    Cmdlet Set-ClusterParameter
    Cmdlet Set-ClusterQuorum
    Cmdlet Set-ClusterResourceDependency
    Cmdlet Start-Cluster
    Cmdlet Start-ClusterGroup
    Cmdlet Start-ClusterNode
    Cmdlet Start-ClusterResource
    Cmdlet Stop-Cluster
    Cmdlet Stop-ClusterGroup
    Cmdlet Stop-ClusterNode
    Cmdlet Stop-ClusterResource
    Cmdlet Suspend-ClusterNode
    Cmdlet Suspend-ClusterResource
    Cmdlet Test-Cluster
    Cmdlet Test-ClusterResourceFailure
    Cmdlet Update-ClusterIPResource
    Cmdlet Update-ClusterNetworkNameResource
    Cmdlet Update-ClusterVirtualMachineConfiguration
```

So let’s play a little.

``` powershell
jeffpatton.admin@WIN8-HV1 | 12:56:01 | 03-20-2012 | C:Usersjeffpatton.admin #
Get-ClusterGroup

Name              OwnerNode State
----              --------- -----
Available Storage win8-hv2  Offline
broker            win8-hv2  Online
Cluster Group     win8-hv2  Online


jeffpatton.admin@WIN8-HV1 | 12:56:04 | 03-20-2012 | C:Usersjeffpatton.admin #
Remove-ClusterGroup -Name broker -RemoveResources

Remove-ClusterGroup
Are you sure that you want to remove the clustered role 'broker'? The resources will be taken offline.
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
jeffpatton.admin@WIN8-HV1 | 12:56:20 | 03-20-2012 | C:Usersjeffpatton.admin #
Remove-Cluster -Name win8-hv

Remove-Cluster
Are you sure you want to completely remove the cluster win8-hv?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
jeffpatton.admin@WIN8-HV1 | 12:56:45 | 03-20-2012 | C:Usersjeffpatton.admin #
Get-Cluster
Get-Cluster : The cluster service is not running.  Make sure that the service is running on all nodes in the cluster.
    There are no more endpoints available from the endpoint mapper
At line:1 char:1
+ Get-Cluster
+ ~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Get-Cluster], ClusterCmdletException
    + FullyQualifiedErrorId : Get-Cluster,Microsoft.FailoverClusters.PowerShell.GetClusterCommand
```

So I just dumped the cluster, I think I’ll create the cluster again with a single node, and then add a node after the fact, since there is a cmdlet for that.

``` powershell
New-Cluster -Name win8-cluster -Node win8-hv1 -NoStorage -Verbose

Name
----
win8-cluster
Let’s confirm that it’s there.

Get-Cluster |Format-List -Property *


Domain                                  : soecs.ku.edu
Name                                    : win8-cluster
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
RouteHistoryLength                      : 0
SameSubnetDelay                         : 1000
SameSubnetThreshold                     : 5
SecurityLevel                           : 1
SharedVolumeCompatibleFilters           : {}
SharedVolumeIncompatibleFilters         : {}
SharedVolumesRoot                       : C:ClusterStorage
SharedVolumeSecurityDescriptor          : {1, 0, 4, 128...}
ShutdownTimeoutInMinutes                : 20
UseNetftForSharedVolumes                : 1
UseClientAccessNetworksForSharedVolumes : 0
SharedVolumeBlockCacheSizeInMB          : 0
WitnessDatabaseWriteTimeout             : 300
WitnessRestartInterval                  : 15
EnableSharedVolumes                     : Enabled
DynamicQuorum                           : 1
Id                                      : d4e05676-cf3d-4814-a828-f32e106bb1c0
```

Let’s see some information about the node.

``` powershell
Get-ClusterNode |Format-List *


Cluster            : win8-cluster
State              : Up
Id                 : 1
Name               : win8-hv1
NodeName           : win8-hv1
NodeHighestVersion : 467002
NodeLowestVersion  : 467002
MajorVersion       : 6
MinorVersion       : 2
BuildNumber        : 8250
CSDVersion         :
NodeInstanceID     : 00000000-0000-0000-0000-000000000001
Description        :
DrainStatus        : NotInitiated
DrainTarget        : 4294967295
DynamicWeight      : 1
NodeWeight         : 1
```

Okay, let’s add a node now. I chopped off the crazy long report filename.

``` powershell
Add-ClusterNode -Name win8-hv2 -Cluster win8-cluster -NoStorage -Verbose
Report file location: C:WindowsclusterReportsAdd Node Wizard
Verify

Get-ClusterNode

Name                 ID    State
----                 --    -----
win8-hv1             1     Up
win8-hv2             2     Up
```

How many cluster do I have? Seems like a lot, but the Windows 8, and dev-cluster aren’t actually there anymore.

``` powershell
Get-Cluster -Domain soecs.ku.edu

Name
----
CLUSTER
DEV-CLUSTER
HYPER-V
WIN8-CLUSTER
WIN8-HV
```

Let’s add server role, this is basically a cluster end-point

``` powershell
Add-ClusterServerRole -Name Win8ServerRole -Cluster win8-cluster -Verbose

Name           OwnerNode State
----           --------- -----
Win8ServerRole win8-hv1  Online
```

How about some details on that role. I cut out the Type property to keep it readable.

``` powershell
Get-ClusterResource -Name win8serverrole |Get-ClusterParameter

Object         Name                   Value
------         ----                   -----
win8serverrole Name                   WIN8SERVERROLE
win8serverrole DnsName                Win8ServerRole
win8serverrole Aliases
win8serverrole RemapPipeNames         0
win8serverrole HostRecordTTL          1200
win8serverrole RegisterAllProvidersIP 0
win8serverrole PublishPTRRecords      0
win8serverrole ResourceData           {1, 0, 0, 0...}
win8serverrole StatusNetBIOS          0
win8serverrole StatusDNS              0
win8serverrole StatusKerberos         0
win8serverrole CreatingDC             \DC1.soecs.ku.edu
win8serverrole LastDNSUpdateTime      3/20/2012 6:17:30 PM
win8serverrole ObjectGUID             e3fbfe6ba596a447a09fd4e117...
```
