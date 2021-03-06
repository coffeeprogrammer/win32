---
title: DMTF Profile Discovery Through Association Traversal
description: A key component of the Windows Management Instrumentation (WMI) infrastructure is an object-oriented model of the manageable entities in a system.
ms.assetid: 21e03d78-bce1-471e-a826-e676d32990ba
ms.tgt_platform: multiple
ms.topic: article
ms.date: 05/31/2018
---

# DMTF Profile Discovery Through Association Traversal

A key component of the Windows Management Instrumentation (WMI) infrastructure is an object-oriented model of the manageable entities in a system. The model conforms to a standard maintained by the Desktop Management Task Force ([DMTF](https://www.dmtf.org/standards/wsman)) and is known as the Common Information Model (CIM). Some classes in the model, such as [CIM\_DataFile](https://msdn.microsoft.com/library/aa387236.aspx) or [Win32\_Process](https://msdn.microsoft.com/library/aa394372.aspx), correspond directly to manageable entities. Other classes in the model, such as [Win32\_SystemServices](https://msdn.microsoft.com/library/aa394484.aspx), represent relationships between manageable entities. These relationship-modeling classes are known as Association classes.

Using the WMI-specific query language, WQL, you can retrieve instances of classes that represent manageable entities or instances of Association classes. But WQL is implementation specific. It works only with the Windows implementation of the DMTF standard (WMI). In addition, the WQL syntax for retrieving Association classes is rather complicated.

The Windows Remote Management (WinRM) infrastructure provides an excellent way to leverage the functionality of WMI. Early versions of WinRM had to use WQL to retrieve instances of Association classes. WinRM 2.0 includes a new feature known as DMTF profile discovery through association traversal. Association traversal enables a user of WinRM to retrieve instances of Association classes by using a standard filtering mechanism, the AssociationFilter dialect, defined in the DMTF CIM binding specification. For more information on association traversal, see the WS-Management CIM Binding specification ([https://www.dmtf.org/standards/wsman]( https://go.microsoft.com/fwlink/p/?linkid=142365)).

The winrm utility provides a simple mechanism to traverse through the appropriate association and retrieve a device profile.

## Configuration Implementation Details

The winrm utility now supports a dialect for the association request. Either the URI or the alias can be specified using the winrm utility.



| Alias       | URI                                                               |
|-------------|-------------------------------------------------------------------|
| association | https://schemas.dmtf.org/wbem/wsman/1/cimbinding/AssociationFilter |



??

## Retrieving Instances of an Association Class by Using the AssociationFilter Dialect

The winrm utility can retrieve WMI association class instances of a particular source instance. The following command demonstrates how to use the winrm utility to retrieve instances of [Win32\_Service](https://msdn.microsoft.com/library/aa394418.aspx) association classes. The switch "-associations" must be used to return association classes.

**winrm enumerate wmicimv2/\* -dialect:association -associations -filter:{object=win32\_service?name=winrm;resultclassname=win32\_dependentservice;role=dependent}**

The following text-based snippet is the output of the above command:

``` syntax
Win32_DependentService
    Antecedent
        Address = https://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous
        ReferenceParameters
            ResourceURI = http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service
            SelectorSet
                Selector: Name = RpcSs
    Dependent
        Address = https://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous
        ReferenceParameters
            ResourceURI = http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service
            SelectorSet
                Selector: Name = WinRM
    TypeOfDependency = null

Win32_DependentService
    Antecedent
        Address = https://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous
        ReferenceParameters
            ResourceURI = http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_SystemDriver
            SelectorSet
                Selector: Name = HTTP
    Dependent
        Address = https://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous
        ReferenceParameters
            ResourceURI = http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_Service
            SelectorSet
                Selector: Name = WinRM
    TypeOfDependency = null
```

## Retrieving Instances of an Associated Class by Using the AssociationFilter Dialect

The winrm utility can retrieve WMI class instances that are associated with a particular source instance. The following command demonstrates how to use the winrm utility to retrieve instances of [Win32\_Service](https://msdn.microsoft.com/library/aa394418.aspx) associated classes.

**winrm enumerate wmicimv2/\* -dialect:association -filter:{object=win32\_service?name=winrm;associationclassname=win32\_dependentservice;resultclassname=win32\_service;resultrole=antecedent;role=dependent}**

The following text-based snippet is the output of the above command:

``` syntax
Win32_Service
    AcceptPause = false
    AcceptStop = false
    Caption = Remote Procedure Call (RPC)
    CheckPoint = 0
    CreationClassName = Win32_Service
    Description = The RPCSS service is the Service Control Manager for COM and DCOM servers. It performs object activations requests, object exporter resolutions and distributed garbage collection for COM and DCOM servers. If this service is stopped or disabled, programs using COM or DCOM will not function properly. It is strongly recommended that you have the RPCSS service running DesktopInteract = false
    DisplayName = Remote Procedure Call (RPC)
    ErrorControl = Normal
    ExitCode = 0
    InstallDate = null
    Name = RpcSs
    PathName = C:\Windows\system32\svchost.exe -k rpcss
    ProcessId = 716
    ServiceSpecificExitCode = 0
    ServiceType = Share Process
    Started = true
    StartMode = Auto
    StartName = NT AUTHORITY\NetworkService
    State = Running
    Status = OK
    SystemCreationClassName = Win32_ComputerSystem
    SystemName = myComputer
    TagId = 0
    WaitHint = 0
```

??

??




