---
Description: Helper structure for managing a shader constant table. This can also be done using ID3DXConstantTable.
ms.assetid: cc6d66e4-c600-420b-b7b5-1bd10ecb22f9
title: D3DXSHADER_CONSTANTTABLE structure (D3dx9shader.h)
ms.topic: reference
ms.date: 05/31/2018
topic_type: 
- APIRef
- kbSyntax
api_name: 
- D3DXSHADER_CONSTANTTABLE
api_type: 
- HeaderDef
api_location: 
- d3dx9shader.h
---

# D3DXSHADER\_CONSTANTTABLE structure

Helper structure for managing a shader constant table. This can also be done using [**ID3DXConstantTable**](id3dxconstanttable.md).

## Syntax


```C++
typedef struct D3DXSHADER_CONSTANTTABLE {
  DWORD Size;
  DWORD Creator;
  DWORD Version;
  DWORD Constants;
  DWORD ConstantInfo;
  DWORD Flags;
  DWORD Target;
} D3DXSHADER_CONSTANTTABLE, *LPD3DXSHADER_CONSTANTTABLE;
```



## Members

<dl> <dt>

**Size**
</dt> <dd>

Type: **[**DWORD**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

Size of the structure. See Remarks.

</dd> <dt>

**Creator**
</dt> <dd>

Type: **[**DWORD**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

Offset from the beginning of this structure, in bytes, to the string that contains the name of the creator.

</dd> <dt>

**Version**
</dt> <dd>

Type: **[**DWORD**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

Shader version.

</dd> <dt>

**Constants**
</dt> <dd>

Type: **[**DWORD**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

Number of constants.

</dd> <dt>

**ConstantInfo**
</dt> <dd>

Type: **[**DWORD**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

Array of constant information, D3DXSHADER\_CONSTANTINFO\[*Constants*\]. See [**D3DXSHADER\_CONSTANTINFO**](d3dxshader-constantinfo.md).

</dd> <dt>

**Flags**
</dt> <dd>

Type: **[**DWORD**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

The [D3DXSHADER Flags](d3dxshader-flags.md) flags used to compile the shader.

</dd> <dt>

**Target**
</dt> <dd>

Type: **[**DWORD**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

Offset into the string that contains the target.

</dd> </dl>

## Remarks

Shader constant information is included in a tab-delimited table of comments. All offsets are measured in bytes from the beginning of the structure. Entries in the constant table are sorted by Creator in ascending order.

A shader constant table can be managed with the [**ID3DXConstantTable**](id3dxconstanttable.md) interfaces. Alternatively, you can manage the constant table with **D3DXSHADER\_CONSTANTTABLE**.

This size member is often initialized using the following:


```
D3DXSHADER_CONSTANTTABLE constantTable;
constantTable.Size = sizeof(D3DXSHADER_CONSTANTTABLE)
```



## Requirements



|                   |                                                                                          |
|-------------------|------------------------------------------------------------------------------------------|
| Header<br/> | <dl> <dt>D3dx9shader.h</dt> </dl> |



## See also

<dl> <dt>

[D3DX Structures](dx9-graphics-reference-d3dx-structures.md)
</dt> <dt>

[**D3DXGetShaderConstantTable**](d3dxgetshaderconstanttable.md)
</dt> </dl>

??

??




