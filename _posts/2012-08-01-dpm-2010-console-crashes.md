---
layout: post
title:  "DPM 2010 console crashes when pushing an agent install"
date:   2012-08-01 15:23:00 -0600
categories: blog
tags: August 2012 KU IT
comments: false
---
This is a new one for me, I've been running DPM for quite a while now and I've not seen this behavior. In a recent staff meeting it came up that the DPM server was having some RPC issues, so since I'm jonesing for stuff to do I said I wouldn't mind taking a look at it.

When you open the DPM Management Console, click the Management tab and then Agents you are presented with all the servers that have the DPM agent installed. From here you are also able to install/uninstall/update the agent. Working through the Agent Install wizard, I selected the server to be backed up, entered my credentials and within a minute received a nasty error message.

``` xml
<FatalServiceError>
    <__System>
        <ID>19</ID>
        <Seq>0</Seq>
        <TimeCreated>8/1/2012 3:12:18 PM</TimeCreated>
        <Source>DpmThreadPool.cs</Source>
        <Line>163</Line>
        <HasError>True</HasError>
    </__System>
    <ExceptionType>ArgumentException</ExceptionType>
    <ExceptionMessage>Value does not fall within the expected range.</ExceptionMessage>
    <ExceptionDetails>
    System.ArgumentException: Value does not fall within the expected range.
    at System.Management.ManagementScope.Initialize()
    at Microsoft.Internal.EnterpriseStorage.Dls.UI.InstallAgentsWizard.Win32Cluster.GetNodeClusterState(String nodeName, ConnectionOptions options, UInt32&amp; clusterState)
    at Microsoft.Internal.EnterpriseStorage.Dls.UI.InstallAgentsWizard.CredentialsPage.CheckForCluster(ProductionServerCollection errorNodesAccessDenied, ProductionServerCollection errorNodesClusterDetectionFailed, ProductionServerCollection errorNodesDRDetectionFailed)
    at Microsoft.Internal.EnterpriseStorage.Dls.UI.InstallAgentsWizard.CredentialsPage.FormListOfTargetServers(WindowsIdentity runAsIdentity)
    at Microsoft.Internal.EnterpriseStorage.Dls.UI.InstallAgentsWizard.CredentialsPage.OnLeavePage(LeavePageEventArgs e)
    at Microsoft.Internal.EnterpriseStorage.UI.WizardFramework.WizardPage.RaiseLeavePage(LeavePageEventArgs e)
    at Microsoft.Internal.EnterpriseStorage.UI.WizardFramework.WizardForm.ValidateAndLeavePage(WizardPage page, LeavePageEventArgs e)
    at Microsoft.Internal.EnterpriseStorage.UI.WizardFramework.WizardForm.TraversePagesToTarget(WizardPage startPage, WizardPage targetPage, NavigationDirection direction)
    at Microsoft.Internal.EnterpriseStorage.UI.WizardFramework.WizardForm.InternalNavigateToPage(WizardPage targetPage, NavigateEventArgs e)
    at Microsoft.Internal.EnterpriseStorage.UI.WizardFramework.WizardForm.NextPage()
    at System.Windows.Forms.Control.OnClick(EventArgs e)
    at System.Windows.Forms.Button.WndProc(Message&amp; m)
    at System.Windows.Forms.Control.ControlNativeWindow.WndProc(Message&amp; m)
    at System.Windows.Forms.NativeWindow.Callback(IntPtr hWnd, Int32 msg, IntPtr wparam, IntPtr lparam)
    </ExceptionDetails>
</FatalServiceError>
```

I know, nasty right? At any rate we ran through several different things, making sure the server we wanted to get at had the proper firewall rules, could we access the admin hidden share, were the groups there and so on. We even fired up netmon and reproduced the problem just to make sure they were talking. Everything seemed ok, so we called up Microsoft and opened a ticket.

After talking with one of the DPM support tech's we found that it was an issue with the remote server we were attempting to connect to. While everything appeared to be ok, there was a problem with the RPC settings in the registry. At some point all the entries in the Internet subkey of RPC were removed. Turns out it's OK if the entire key is missing, or if the key is there and has the proper settings in it, but if it's there and empty that's hurty.

Here is some information he pasted over to me about this key:

> With Registry Editor, you can modify the following parameters for RPC. The RPC Port key values discussed below are all located in the following key in the registry: HKEY_LOCAL_MACHINESoftwareMicrosoftRpcInternet Key Data Type
>
> Ports REG_MULTI_SZ Specifies a set of IP port ranges consisting of either all the ports available from the Internet or all the ports not available from the Internet. Each string represents a single port or an inclusive set of ports.
>
> For example, a single port may be represented by 5984, and a set of ports may be represented by 5000-5100. If any entries are outside the range of 0 to 65535, or if any string cannot be interpreted, the RPC runtime treats the entire configuration as invalid.
>
> PortsInternetAvailable REG_SZ Y or N (not case-sensitive) If Y, the ports listed in the Ports key are all the Internet-available ports on that computer. If N, the ports listed in the Ports key are all those ports that are not Internet-available.
>
> UseInternetPorts REG_SZ ) Y or N (not case-sensitive Specifies the system default policy. If Y, the processes using the default will be assigned ports from the set of Internet-available ports, as defined previously. If N, the processes using the default will be assigned ports from the set of intranet-only ports. > Example:
>
> In this example ports 5000 through 5100 inclusive have been arbitrarily selected to help illustrate how the new registry key can be configured. This is not a recommendation of a minimum number of ports needed for any particular system.
>
> 1. Add the Internet key under: HKEY_LOCAL_MACHINESoftwareMicrosoftRpc
> 2. Under the Internet key, add the values "Ports" (MULTI_SZ), "PortsInternetAvailable" (REG_SZ), and "UseInternetPorts" (REG_SZ).
> 3. Restart the server. All applications that use RPC dynamic port allocation use ports 5000 through 5100, inclusive. In most environments, a minimum of 100 ports should be opened, because several system services rely on these RPC ports to communicate with each other.
>
> For example, the new registry key appears as follows: Ports: REG_MULTI_SZ: 5000-5100 PortsInternetAvailable: REG_SZ: Y UseInternetPorts: REG_SZ: Y
>

The solution was very easy, simply delete (or correct) the malformed entry and reboot. Worked like a charm!
