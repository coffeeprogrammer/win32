---
Description: Same as a D3DXVECTOR3, but it uses 16-bit floating point values for x, y, and z.
ms.assetid: b21676f1-5cff-4eef-bd60-5c09882283dc
title: D3DXVECTOR3_16F structure (D3DX10Math.h)
ms.topic: reference
ms.date: 05/31/2018
topic_type: 
- APIRef
- kbSyntax
api_name: 
- D3DXVECTOR3_16F
api_type: 
- HeaderDef
api_location: 
- D3DX10Math.h
---

# D3DXVECTOR3\_16F structure

Same as a [**D3DXVECTOR3**](d3d10-d3dxvector3.md), but it uses 16-bit floating point values for x, y, and z.

## Syntax


```C++
typedef struct D3DXVECTOR3_16F {
  FLOAT x;
  FLOAT y;
  FLOAT z;
} D3DXVECTOR3_16F, *LPD3DXVECTOR3_16F;
```



## Members

<dl> <dt>

**x**
</dt> <dd>

Type: **[**FLOAT**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

The x-component.

</dd> <dt>

**y**
</dt> <dd>

Type: **[**FLOAT**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

The y-component.

</dd> <dt>

**z**
</dt> <dd>

Type: **[**FLOAT**](https://msdn.microsoft.com/library/Aa383751(v=VS.85).aspx)**

</dd> <dd>

The z-component.

</dd> </dl>

## Remarks

**D3DXVECTOR3\_16F** has the following C++ extensions.

### D3DXVECTOR3\_16F Extensions


```

typedef struct D3DXVECTOR3_16F
{
#ifdef __cplusplus
public:
    D3DXVECTOR3_16F() {};
    D3DXVECTOR3_16F( CONST FLOAT * );
    D3DXVECTOR3_16F( CONST D3DVECTOR& );
    D3DXVECTOR3_16F( CONST D3DXFLOAT16 * );
    D3DXVECTOR3_16F( CONST D3DXFLOAT16 &x, CONST D3DXFLOAT16 &y, CONST D3DXFLOAT16 &z );

    // casting
    operator D3DXFLOAT16* ();
    operator CONST D3DXFLOAT16* () const;

    // binary operators
    BOOL operator == ( CONST D3DXVECTOR3_16F& ) const;
    BOOL operator != ( CONST D3DXVECTOR3_16F& ) const;

public:
#endif //__cplusplus
    D3DXFLOAT16 x, y, z;

} D3DXVECTOR3_16F, *LPD3DXVECTOR3_16F;
```



## Requirements



|                   |                                                                                         |
|-------------------|-----------------------------------------------------------------------------------------|
| Header<br/> | <dl> <dt>D3DX10Math.h</dt> </dl> |



## See also

<dl> <dt>

[D3DX Structures](d3d10-graphics-reference-d3dx10-structures.md)
</dt> </dl>

??

??




