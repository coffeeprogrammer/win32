---
Description: The Win32\_SystemLoadOrderGroups association WMI class relates a computer system and a load order group.
ms.assetid: fb637300-0f70-465a-a72b-f0ab3f246790
ms.tgt_platform: multiple
title: Win32_SystemLoadOrderGroups class
ms.topic: reference
ms.date: 05/31/2018
topic_type: 
- APIRef
- kbSyntax
api_name: 
- Win32_SystemLoadOrderGroups
- Win32_SystemLoadOrderGroups.GroupComponent
- Win32_SystemLoadOrderGroups.PartComponent
api_type: 
- DllExport
api_location: 
- CIMWin32.dll
---

# Win32\_SystemLoadOrderGroups class

The **Win32\_SystemLoadOrderGroups** association [WMI class](https://msdn.microsoft.com/library/Aa393244(v=VS.85).aspx) relates a computer system and a load order group.

The following syntax is simplified from Managed Object Format (MOF) code and includes all of the inherited properties. Properties and methods are in alphabetic order, not MOF order.

## Syntax

``` syntax
[Dynamic, Provider("CIMWin32"), UUID("{8502C503-5FBB-11D2-AAC1-006008C78BC7}"), AMENDMENT]
class Win32_SystemLoadOrderGroups : CIM_SystemComponent
{
  Win32_ComputerSystem REF GroupComponent;
  Win32_LoadOrderGroup REF PartComponent;
};
```

## Members

The **Win32\_SystemLoadOrderGroups** class has these types of members:

-   [Properties](#properties)

### Properties

The **Win32\_SystemLoadOrderGroups** class has these properties.

<dl> <dt>

**GroupComponent**
</dt> <dd> <dl> <dt>

Data type: **Win32\_ComputerSystem**
</dt> <dt>

Access type: Read-only
</dt> <dt>

Qualifiers: [**key**](https://msdn.microsoft.com/library/Aa392157(v=VS.85).aspx), [**Override**](https://msdn.microsoft.com/library/Aa393650(v=VS.85).aspx) ("GroupComponent"), [**MappingStrings**](https://msdn.microsoft.com/library/Aa393650(v=VS.85).aspx) ("WMI\|Win32\_ComputerSystem")
</dt> </dl>

Reference to the instance representing the computer system where the load order group exists.

</dd> <dt>

**PartComponent**
</dt> <dd> <dl> <dt>

Data type: **Win32\_LoadOrderGroup**
</dt> <dt>

Access type: Read-only
</dt> <dt>

Qualifiers: [**key**](https://msdn.microsoft.com/library/Aa392157(v=VS.85).aspx), [**Override**](https://msdn.microsoft.com/library/Aa393650(v=VS.85).aspx) ("PartComponent"), [**MappingStrings**](https://msdn.microsoft.com/library/Aa393650(v=VS.85).aspx) ("WMI\|Win32\_LoadOrderGroup")
</dt> </dl>

Reference to the instance representing the load order group existing on the computer system.

</dd> </dl>

## Remarks

The **Win32\_SystemLoadOrderGroups** class is derived from [**CIM\_SystemComponent**](cim-systemcomponent.md).

## Requirements



|                                     |                                                                                         |
|-------------------------------------|-----------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows??Vista<br/>                                                                |
| Minimum supported server<br/> | Windows Server??2008<br/>                                                          |
| Namespace<br/>                | Root\\CIMV2<br/>                                                                  |
| MOF<br/>                      | <dl> <dt>CIMWin32.mof</dt> </dl> |
| DLL<br/>                      | <dl> <dt>CIMWin32.dll</dt> </dl> |



## See also

<dl> <dt>

[**CIM\_SystemComponent**](cim-systemcomponent.md)
</dt> <dt>

[Operating System Classes](https://msdn.microsoft.com/library/Dn792258(v=VS.85).aspx)
</dt> </dl>

??

??




