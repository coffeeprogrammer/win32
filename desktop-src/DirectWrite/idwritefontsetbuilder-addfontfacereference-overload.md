---
title: IDWriteFontSetBuilder AddFontFaceReference methods (Dwrite\_3.h)
description: Adds a reference to a font to the set being built.
ms.assetid: 2543720f-1b5a-ca1d-9e24-3fcd61a4de37
keywords:
- AddFontFaceReference methods Direct Write
topic_type:
- apiref
api_location:
- dwrite_3.h
api_type:
- HeaderDef
ms.date: 07/02/2019
ms.topic: reference
---

# IDWriteFontSetBuilder::AddFontFaceReference methods

Adds a reference to a font to the set being built.

### Overload list



| Method                                                                                                                                           | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|:-------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**AddFontFaceReference(IDWriteFontFaceReference\*)**](https://msdn.microsoft.com/library/Dn933237(v=VS.85).aspx)                                         | Adds a reference to a font to the set being built. The necessary metadata will automatically be extracted from the font upon calling CreateFontSet.<br/>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| [**AddFontFaceReference(IDWriteFontFaceReference\*, const DWRITE\_FONT\_PROPERTY\*, UINT32)**](https://msdn.microsoft.com/library/Dn933238(v=VS.85).aspx) | Adds a reference to a font to the set being built. The caller supplies enough information to search on, avoiding the need to open the potentially non-local font. Any properties not supplied by the caller will be missing, and those properties will not be available as filters in GetMatchingFonts. GetPropertyValues for missing properties will return an empty string list. The properties passed should generally be consistent with the actual font contents, but they need not be. You could, for example, alias a font using a different name or unique identifier, or you could set custom tags not present in the actual font.<br/> |



## Requirements



|                   |                                                                                        |
|-------------------|----------------------------------------------------------------------------------------|
| Header<br/> | <dl> <dt>Dwrite\_3.h</dt> </dl> |



## See also

<dl> <dt>

[**IDWriteFontSetBuilder**](https://msdn.microsoft.com/library/Dn933236(v=VS.85).aspx)
</dt> </dl>

???

???





