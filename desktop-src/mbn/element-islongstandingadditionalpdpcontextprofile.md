---
Description: IsLongStandingAdditionalPdpContextProfile
MS-HAID: WWAN\_profile\_v4.element\_IsLongStandingAdditionalPdpContextProfile
MSHAttr:
- PreferredSiteName:MSDN
- PreferredLib:/library/windows/desktop
title: IsLongStandingAdditionalPdpContextProfile
ms.topic: reference
ms.date: 05/31/2018
---

# <span id="WWAN_profile_v4.element_IsLongStandingAdditionalPdpContextProfile"></span>IsLongStandingAdditionalPdpContextProfile

Specifies that this profile is a long-standing additional PDP context profile.If the value of this element is **true**, then [**IsAdditionalPdpContextProfile**](https://msdn.microsoft.com/library/Mt156987(v=VS.85).aspx) must also be set to **true**. This is a new element for v4.

## Element hierarchy

[<MBNProfileExt>](element-mbnprofileext.md)  
**<IsLongStandingAdditionalPdpContextProfile>**

## Syntax

``` syntax
<IsLongStandingAdditionalPdpContextProfile>

  boolean

</IsLongStandingAdditionalPdpContextProfile>
```

## <span id="Attributes_and_Elements"></span><span id="attributes_and_elements"></span><span id="ATTRIBUTES_AND_ELEMENTS"></span>Attributes and Elements

### <span id="attributes"></span><span id="ATTRIBUTES"></span>Attributes

None.

### <span id="Child_Elements"></span><span id="child_elements"></span><span id="CHILD_ELEMENTS"></span>Child Elements

None.

### <span id="parent_elements"></span><span id="PARENT_ELEMENTS"></span>Parent Elements

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Parent Element</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="element-mbnprofileext.md">MBNProfileExt</a></td>
<td><p>The <strong>MBNProfileExt</strong> element is an extension of the earlier MBNProfile element. It identifies a Mobile Broadband profile with a richer set of options than the MBNProfile element.</p>
<p>There can be more than one MbnProfileExt element in a profile, describing profile settings for a particular set of operating conditions. Use the <a href="element-profileconditionedon.md"><strong>ProfileConditionedOn</strong></a> child element of <strong>MBNProfileExt</strong> to specify which operating conditions make a particular profile the active profile.</p></td>
</tr>
</tbody>
</table>

??

## Requirements

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Namespace</p></td>
<td><p>https://www.microsoft.com/networking/WWAN/profile/v4</p></td>
</tr>
</tbody>
</table>

??

??



