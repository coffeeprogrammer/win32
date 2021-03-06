---
title: control attribute
description: The \ control\ attribute identifies a coclass or library as a COM control, from which a container site will derive additional type libraries or component object classes.
ms.assetid: c39dd4b6-743f-4888-8954-8b83584bdea5
keywords:
- control attribute MIDL
topic_type:
- apiref
api_name:
- control
api_type:
- NA
ms.topic: reference
ms.date: 05/31/2018
---

# control attribute

The **\[control\]** attribute identifies a [**coclass**](coclass.md) or [**library**](library.md) as a COM control, from which a container site will derive additional type libraries or component object classes.

``` syntax
[
    uuid, 
    control 
    [, attribute-list]
] 
library | coclass lib-or-coclassname 
{ 
    definitions 
}
```

## Parameters

<dl> <dt>

*attribute-list* 
</dt> <dd>

Specifies zero or more attributes that apply to the [**library**](library.md) or [**coclass**](coclass.md) statement. Separate multiple attributes with commas.

</dd> <dt>

*lib-or-coclassname* 
</dt> <dd>

Specifies the name of the [**library**](library.md) or [**coclass**](coclass.md).

</dd> <dt>

*definitions* 
</dt> <dd>

MIDL statements that specify the members of the [**library**](library.md) or [**coclass**](coclass.md).

</dd> </dl>

## Remarks

This attribute allows you to mark type libraries that describe controls so they will not be displayed in type browsers intended for nonvisual objects.

### Flags

TYPEFLAG\_FCONTROL, LIBFLAG\_FCONTROL

## Examples

``` syntax
[
    uuid(12345678-1234-1234-1234-123456789ABC),
    helpstring("Hello 2.1 COM Control Library"), 
    control,version(2.1)
] 
library Hello 
{ 
    /* library definitions */
}
```

## See also

<dl> <dt>

[ODL File Syntax](https://msdn.microsoft.com/library/ms221683(v=VS.71).aspx)
</dt> <dt>

[ODL File Example](https://msdn.microsoft.com/library/ms221308(v=VS.71).aspx)
</dt> <dt>

[Generating a Type Library With MIDL](generating-a-type-library-with-midl-2.md)
</dt> <dt>

[TYPEFLAGS](https://msdn.microsoft.com/library/ms221509(v=VS.71).aspx)
</dt> <dt>

[**coclass**](coclass.md)
</dt> <dt>

[**library**](library.md)
</dt> </dl>

??

??




