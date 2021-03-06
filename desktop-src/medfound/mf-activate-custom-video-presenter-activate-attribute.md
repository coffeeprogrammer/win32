---
Description: Specifies an activation object that creates a custom video presenter for the enhanced video renderer (EVR) media sink.
ms.assetid: 65d88832-0969-4d85-bee2-fd0aa68e9f3b
title: MF_ACTIVATE_CUSTOM_VIDEO_PRESENTER_ACTIVATE attribute (Mfidl.h)
ms.topic: reference
ms.date: 05/31/2018
---

# MF\_ACTIVATE\_CUSTOM\_VIDEO\_PRESENTER\_ACTIVATE attribute

Specifies an activation object that creates a custom video presenter for the enhanced video renderer (EVR) media sink.

## Data type

**IUnknown\***

## Remarks

If you are creating the EVR through an activation object, you can use this attribute to set a custom video presenter on the EVR. Use this attribute as follows:

1.  Call the [**MFCreateVideoRendererActivate**](/windows/desktop/api/mfidl/nf-mfidl-mfcreatevideorendereractivate) function to create an activation object for the EVR. The function returns a pointer to the [**IMFActivate**](/windows/desktop/api/mfobjects/nn-mfobjects-imfactivate) interface.
2.  Set this attribute on the [**IMFActivate**](/windows/desktop/api/mfobjects/nn-mfobjects-imfactivate) pointer by calling [**IMFAttributes::SetUnknown**](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setunknown). The value of the attribute is a pointer to an activation object implemented by the caller. The caller's activation object must expose the **IMFActivate** interface.

If you set this attribute, the EVR calls [**IMFActivate::ActivateObject**](/windows/desktop/api/mfobjects/nf-mfobjects-imfactivate-activateobject) to create the custom video presenter. The video presenter must expose the [**IMFVideoPresenter**](/windows/desktop/api/evr/nn-evr-imfvideopresenter) interface.

The GUID constant for this attribute is exported from mfuuid.lib.

## Requirements



|                                     |                                                                                    |
|-------------------------------------|------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows??Vista \[desktop apps only\]<br/>                                     |
| Minimum supported server<br/> | Windows Server??2008 \[desktop apps only\]<br/>                               |
| Header<br/>                   | <dl> <dt>Mfidl.h</dt> </dl> |



## See also

<dl> <dt>

[Alphabetical List of Media Foundation Attributes](alphabetical-list-of-media-foundation-attributes.md)
</dt> <dt>

[Enhanced Video Renderer Attributes](enhanced-video-renderer-attributes.md)
</dt> <dt>

[**IMFAttributes::GetUnknown**](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-getunknown)
</dt> <dt>

[**IMFAttributes::SetUnknown**](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setunknown)
</dt> <dt>

[**IMFActivate**](/windows/desktop/api/mfobjects/nn-mfobjects-imfactivate)
</dt> <dt>

[Activation Objects](activation-objects.md)
</dt> <dt>

[How to Write an EVR Presenter](how-to-write-an-evr-presenter.md)
</dt> </dl>

??

??




