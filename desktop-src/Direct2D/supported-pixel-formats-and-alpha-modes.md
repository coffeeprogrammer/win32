---
title: Supported Pixel Formats and Alpha Modes
description: Describes the pixel formats and alpha modes supported by each render target type.
ms.assetid: 09b1f9c6-1780-4733-ac22-9e8c21466b67
keywords:
- Direct2D,pixel formats
- Direct2D,alpha modes
ms.topic: article
ms.date: 05/31/2018
ms.custom: "seodec18"
---

# Supported Pixel Formats and Alpha Modes

This topic describes the pixel formats and alpha modes supported by the various parts of Direct2D, including each render target type, the [**ID2D1Bitmap**](https://msdn.microsoft.com/library/Dd371109(v=VS.85).aspx), and [**ID2D1ImageSource**](https://msdn.microsoft.com/library/Dn900413(v=VS.85).aspx). It contains the following sections.

-   [Supported YUV Formats for DXGI Image Source](#supported-yuv-formats-for-dxgi-image-source)
-   [Specifying a Pixel Format for a Render Target](#specifying-a-pixel-format-for-a-render-target)
-   [Supported Formats for ID2D1HwndRenderTarget](#supported-formats-for-id2d1hwndrendertarget)
-   [Supported formats for ID2D1DeviceContext](#supported-formats-for-id2d1devicecontext)
-   [Supported Formats for Compatible Render Target](#supported-formats-for-compatible-render-target)
-   [Supported Formats for DXGI Surface Render Target](#supported-formats-for-dxgi-surface-render-target)
-   [Supported Formats for WIC Bitmap Render Target](#supported-formats-for-wic-bitmap-render-target)
-   [Supported Formats for ID2D1DCRenderTarget](#supported-formats-for-id2d1dcrendertarget)
-   [Specifying a Pixel Format for an ID2D1Bitmap](#specifying-a-pixel-format-for-an-id2d1bitmap)
    -   [Supported WIC Formats](#supported-wic-formats)
-   [Using an Unsupported Format](#using-an-unsupported-format)
-   [About Alpha Modes](#about-alpha-modes)
    -   [About Premultiplied and Straight Alpha Modes](#about-premultiplied-and-straight-alpha-modes)
    -   [The Differences Between Straight and Premultiplied Alpha](#the-differences-between-straight-and-premultiplied-alpha)
    -   [Alpha Mode for Render Targets](#alpha-mode-for-render-targets)
    -   [ClearType and Alpha Modes](#cleartype-and-alpha-modes)
-   [Related topics](#related-topics)

## Supported YUV Formats for DXGI Image Source

An [**ID2D1ImageSource**](https://msdn.microsoft.com/library/Dn900413(v=VS.85).aspx) is an abstracted provider of pixels. It can be instantiated from either WIC ([**CreateImageSourceFromWic**](/windows/desktop/api/d2d1_3/nf-d2d1_3-id2d1devicecontext2-createimagesourcefromwic(iwicbitmapsource_id2d1imagesourcefromwic)) or an [**IDXGISurface**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) ([**CreateImageSourceFromDxgi**](https://msdn.microsoft.com/library/Dn890791(v=VS.85).aspx)).

[**ID2D1ImageSourceFromWic**](https://msdn.microsoft.com/library/Dn900414(v=VS.85).aspx) supports the same set of pixel formats and alpha modes as [**ID2D1Bitmap**](https://msdn.microsoft.com/library/Dd371109(v=VS.85).aspx).

In addition to the above, an [**ID2D1ImageSource**](https://msdn.microsoft.com/library/Dn900413(v=VS.85).aspx) that is instantiated from [**IDXGISurface**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface) also supports some YUV pixel formats, including planar data split into multiple surfaces. See [**CreateImageSourceFromDxgi**](https://msdn.microsoft.com/library/Dn890791(v=VS.85).aspx) for more information about requirements for each pixel format.



| Format                    |
|---------------------------|
| DXGI\_FORMAT\_AYUV        |
| DXGI\_FORMAT\_NV12        |
| DXGI\_FORMAT\_YUY2        |
| DXGI\_FORMAT\_P208        |
| DXGI\_FORMAT\_V208        |
| DXGI\_FORMAT\_V408        |
| DXGI\_FORMAT\_R8\_UNORM   |
| DXGI\_FORMAT\_R8G8\_UNORM |



??

## Specifying a Pixel Format for a Render Target

When you create a render target, you must specify its pixel format. To specify the pixel format, you use a [**D2D1\_PIXEL\_FORMAT**](/windows/desktop/api/dcommon/ns-dcommon-d2d1_pixel_format) structure to set the **pixelFormat** member of a [**D2D1\_RENDER\_TARGET\_PROPERTIES**](/windows/desktop/api/d2d1/ns-d2d1-d2d1_render_target_properties) structure. Then, you pass that structure to the appropriate Create method, such as [**ID2D1Factory::CreateHwndRenderTarget**](https://msdn.microsoft.com/library/Dd371275(v=VS.85).aspx).

The [**D2D1\_PIXEL\_FORMAT**](/windows/desktop/api/dcommon/ns-dcommon-d2d1_pixel_format) structure has two fields:

-   **format**, a [DXGI\_FORMAT](https://msdn.microsoft.com/library/bb173059(VS.85).aspx) value that describes the size and arrangement of channels in each pixel, and
-   **alpha**, a [**D2D1\_ALPHA\_MODE**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) value that describes how alpha information is interpreted.

The following example creates a [**D2D1\_PIXEL\_FORMAT**](/windows/desktop/api/dcommon/ns-dcommon-d2d1_pixel_format) structure and uses it to specify the pixel format and alpha mode of an [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx).


```C++
RECT rc;
GetClientRect(m_hwnd, &rc);

D2D1_SIZE_U size = D2D1::SizeU(
    rc.right - rc.left,
    rc.bottom - rc.top
    );

// Create a pixel format and initial its format
// and alphaMode fields.
D2D1_PIXEL_FORMAT pixelFormat = D2D1::PixelFormat(
    DXGI_FORMAT_B8G8R8A8_UNORM,
    D2D1_ALPHA_MODE_IGNORE
    );

D2D1_RENDER_TARGET_PROPERTIES props = D2D1::RenderTargetProperties();
props.pixelFormat = pixelFormat;

// Create a Direct2D render target.
hr = m_pD2DFactory->CreateHwndRenderTarget(
    props,
    D2D1::HwndRenderTargetProperties(m_hwnd, size),
    &m_pRT
    );

```



Different render targets support different format and alpha mode combinations. The following sections list the format and alpha combinations supported by each render target.

## Supported Formats for ID2D1HwndRenderTarget

The supported formats for an [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx) depend on whether it renders by using hardware or software, or whether Direct2D handles the rendering mode automatically by default.

> [!Note]  
> We recommend that you use [**DXGI\_FORMAT\_B8G8R8A8\_UNORM**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) as the pixel format for better performance. This is particularly helpful for software render targets. BGRA format targets perform better than RGBA formats.

??

When you create an [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx), you use the [**D2D1\_RENDER\_TARGET\_PROPERTIES**](/windows/desktop/api/d2d1/ns-d2d1-d2d1_render_target_properties) structure to specify rendering options. The options include the pixel format, as noted in the previous section. The type field of this structure enables you to specify whether the render target renders to hardware or software, or whether Direct2D should automatically determine the rendering mode.

To enable Direct2D to determine whether the render target uses hardware or software rendering, use the [**D2D1\_RENDER\_TARGET\_TYPE\_DEFAULT**](https://docs.microsoft.com/windows/desktop/api/d2d1/ne-d2d1-d2d1_render_target_type) setting.

The following table lists the supported formats for [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx) objects that are created by using the [**D2D1\_RENDER\_TARGET\_TYPE\_DEFAULT**](https://docs.microsoft.com/windows/desktop/api/d2d1/ne-d2d1-d2d1_render_target_type) setting.



| Format                        | Alpha mode                       |
|-------------------------------|----------------------------------|
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_UNKNOWN       |



??

To force a render target to use hardware rendering, use the [**D2D1\_RENDER\_TARGET\_TYPE\_HARDWARE**](https://docs.microsoft.com/windows/desktop/api/d2d1/ne-d2d1-d2d1_render_target_type) setting. The following table lists the supported formats for [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx) objects that explicitly use hardware rendering.



| Format                        | Alpha mode                       |
|-------------------------------|----------------------------------|
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_R8G8B8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_R8G8B8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_R8G8B8A8\_UNORM | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_UNKNOWN       |



??

To force a render target to use software rendering, use the [**D2D1\_RENDER\_TARGET\_TYPE\_SOFTWARE**](https://docs.microsoft.com/windows/desktop/api/d2d1/ne-d2d1-d2d1_render_target_type) setting. The following table lists the supported formats for [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx) objects that explicitly use software rendering.



| Format                        | Alpha mode                       |
|-------------------------------|----------------------------------|
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_UNKNOWN       |



??

Regardless of whether the [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx) is hardware accelerated, the [DXGI\_FORMAT\_UNKNOWN](https://msdn.microsoft.com/library/bb173059(VS.85).aspx) format uses [DXGI\_FORMAT\_B8G8R8A8](https://msdn.microsoft.com/library/bb173059(VS.85).aspx) by default and the [**D2D1\_ALPHA\_MODE\_UNKNOWN**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) alpha mode uses **D2D1\_ALPHA\_MODE\_IGNORE** by default.

## Supported formats for ID2D1DeviceContext

Starting with Windows??8 the [**device context**](https://msdn.microsoft.com/library/Hh404479(v=VS.85).aspx) takes advantage of more of the [**Direct3D high color formats**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) like:

-   DXGI\_FORMAT\_B8G8R8A8\_UNORM\_SRGB
-   DXGI\_FORMAT\_R8G8B8A8\_UNORM\_SRGB
-   DXGI\_FORMAT\_R16G16B16A16\_UNORM
-   DXGI\_FORMAT\_R16G16B16A16\_FLOAT
-   DXGI\_FORMAT\_R32G32B32A32\_FLOAT

Use the [**ID2D1DeviceContext::IsDxgiFormatSupported**](https://msdn.microsoft.com/library/Hh847982(v=VS.85).aspx) method to see if a format works on a particular device context. These formats may also work on an [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx).

These formats are in addition to the formats supported by the [**ID2D1HwndRenderTarget**](https://msdn.microsoft.com/library/Dd371461(v=VS.85).aspx) interface in Windows??7. See [Devices and Device Contexts](devices-and-device-contexts.md) for more information.

## Supported Formats for Compatible Render Target

A compatible render target (an [**ID2D1BitmapRenderTarget**](https://msdn.microsoft.com/library/Dd371146(v=VS.85).aspx) that is created by one of the [**ID2D1RenderTarget::CreateCompatibleRenderTarget**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-createcompatiblerendertarget(id2d1bitmaprendertarget)) methods) inherits the supported formats and alpha modes of the render target that created it. A compatible render target also supports the following format and alpha mode combinations, regardless of what its parent supports.



| Format                  | Alpha mode                       |
|-------------------------|----------------------------------|
| DXGI\_FORMAT\_A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_A8\_UNORM | D2D1\_ALPHA\_MODE\_STRAIGHT      |
| DXGI\_FORMAT\_UNKNOWN   | D2D1\_ALPHA\_MODE\_UNKNOWN       |



??

The [DXGI\_FORMAT\_UNKNOWN](https://msdn.microsoft.com/library/bb173059(VS.85).aspx) format uses the parent render target format by default and the [**D2D1\_ALPHA\_MODE\_UNKNOWN**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) alpha mode uses **D2D1\_ALPHA\_MODE\_PREMULTIPLIED** by default.

## Supported Formats for DXGI Surface Render Target

A DXGI render target is an [**ID2D1RenderTarget**](https://msdn.microsoft.com/library/Dd371766(v=VS.85).aspx) that is created by one of the [**ID2D1Factory::CreateDxgiSurfaceRenderTarget**](https://msdn.microsoft.com/library/Dd371264(v=VS.85).aspx) methods. It supports the following format and alpha mode combinations.



| Format                        | Alpha mode                       |
|-------------------------------|----------------------------------|
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_R8G8B8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_R8G8B8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_A8\_UNORM       | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_A8\_UNORM       | D2D1\_ALPHA\_MODE\_STRAIGHT      |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_IGNORE        |



??

> [!Note]  
> The format must match the format of the DXGI surface to which the DXGI surface render target draws.

??

The [DXGI\_FORMAT\_UNKNOWN](https://msdn.microsoft.com/library/bb173059(VS.85).aspx) format uses the DXGI surface format by default. Do not use the [**D2D1\_ALPHA\_MODE\_UNKNOWN**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) alpha mode with a DXGI surface render target. It has no default value and will cause the DXGI surface render target creation to fail.

## Supported Formats for WIC Bitmap Render Target

A WIC bitmap render target is an [**ID2D1RenderTarget**](https://msdn.microsoft.com/library/Dd371766(v=VS.85).aspx) that is created by one of the [**ID2D1Factory::CreateWicBitmapRenderTarget**](https://msdn.microsoft.com/library/Dd371309(v=VS.85).aspx) methods. It supports the following format and alpha mode combinations.



| Format                        | Alpha mode                       |
|-------------------------------|----------------------------------|
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_A8\_UNORM       | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_A8\_UNORM       | D2D1\_ALPHA\_MODE\_STRAIGHT      |
| DXGI\_FORMAT\_A8\_UNORM       | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_UNKNOWN         | D2D1\_ALPHA\_MODE\_UNKNOWN       |



??

The pixel format of the WIC bitmap target must match the pixel format of the WIC bitmap.

The[DXGI\_FORMAT\_UNKNOWN](https://msdn.microsoft.com/library/bb173059(VS.85).aspx) format uses the WIC bitmap format by default and the [**D2D1\_ALPHA\_MODE\_UNKNOWN**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) alpha mode uses WIC bitmap alpha mode by default.

## Supported Formats for ID2D1DCRenderTarget

An [**ID2D1DCRenderTarget**](https://msdn.microsoft.com/library/Dd371213(v=VS.85).aspx) supports the following format and alpha mode combinations.



| Format                        | Alpha mode                       |
|-------------------------------|----------------------------------|
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE        |



??

Do not use the [DXGI\_FORMAT\_UNKNOWN](https://msdn.microsoft.com/library/bb173059(VS.85).aspx) format or the [**D2D1\_ALPHA\_MODE\_UNKNOWN**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) alpha mode with an [**ID2D1DCRenderTarget**](https://msdn.microsoft.com/library/Dd371213(v=VS.85).aspx). It has no default value and will cause the **ID2D1DCRenderTarget** creation to fail.

## Specifying a Pixel Format for an ID2D1Bitmap

Generally, [**ID2D1Bitmap**](https://msdn.microsoft.com/library/Dd371109(v=VS.85).aspx) objects support the following formats and alpha modes (with some restrictions, described in the paragraphs that follow.)



| Format                                                      | Alpha mode                       |
|-------------------------------------------------------------|----------------------------------|
| DXGI\_FORMAT\_B8G8R8A8\_UNORM                               | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM                               | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_B8G8R8A8\_UNORM                               | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_A8\_UNORM                                     | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_A8\_UNORM                                     | D2D1\_ALPHA\_MODE\_STRAIGHT      |
| DXGI\_FORMAT\_A8\_UNORM                                     | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_UNKNOWN                                       | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_UNKNOWN                                       | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_UNKNOWN                                       | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_B8G8R8X8\_UNORM (Windows??8.1 and later, only) | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_BC1\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_BC1\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_BC1\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_BC2\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_BC2\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_BC2\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_UNKNOWN       |
| DXGI\_FORMAT\_BC3\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| DXGI\_FORMAT\_BC3\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_IGNORE        |
| DXGI\_FORMAT\_BC3\_UNORM (Windows??8.1 and later, only)      | D2D1\_ALPHA\_MODE\_UNKNOWN       |



??

When you use the [**ID2D1RenderTarget::CreateSharedBitmap**](https://msdn.microsoft.com/library/Dd371865(v=VS.85).aspx) method, you use the **pixelFormat** field of a [**D2D1\_BITMAP\_PROPERTIES**](/windows/desktop/api/d2d1/ns-d2d1-d2d1_bitmap_properties) structure to specify the pixel format of the new render target. It must match the pixel format of the [**ID2D1Bitmap**](https://msdn.microsoft.com/library/Dd371109(v=VS.85).aspx) source.

When you use the [**CreateBitmapFromWicBitmap**](https://msdn.microsoft.com/library/Dd371797(v=VS.85).aspx) method, you use the **pixelFormat** field of a [**D2D1\_BITMAP\_PROPERTIES**](/windows/desktop/api/d2d1/ns-d2d1-d2d1_bitmap_properties) structure (instead of the **pixelFormat** member of a [**D2D1\_RENDER\_TARGET\_PROPERTIES**](/windows/desktop/api/d2d1/ns-d2d1-d2d1_render_target_properties) structure) to specify the pixel format of the new render target. It must match the pixel format of the WIC bitmap source.

> [!Note]  
> For more information about support for block compressed (BC???) pixel formats, see [Block compression](block-compression.md).

??

### Supported WIC Formats

When you use the [**CreateBitmapFromWicBitmap**](https://msdn.microsoft.com/library/Dd371797(v=VS.85).aspx) method to create a bitmap from a WIC bitmap, or when you use the [**CreateSharedBitmap**](https://msdn.microsoft.com/library/Dd371865(v=VS.85).aspx) method with an [**IWICBitmapLock**](https://msdn.microsoft.com/library/Ee690161(v=VS.85).aspx), the WIC source must be in a format supported by Direct2D.



| WIC format                     | Corresponding DXGI format     | Corresponding alpha mode                                        |
|--------------------------------|-------------------------------|-----------------------------------------------------------------|
| GUID\_WICPixelFormat8bppAlpha  | DXGI\_FORMAT\_A8\_UNORM       | D2D1\_ALPHA\_MODE\_STRAIGHT or D2D1\_ALPHA\_MODE\_PREMULTIPLIED |
| GUID\_WICPixelFormat32bppPRGBA | DXGI\_FORMAT\_R8G8B8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED or D2D1\_ALPHA\_MODE\_IGNORE   |
| GUID\_WICPixelFormat32bppBGR   | DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_IGNORE                                       |
| GUID\_WICPixelFormat32bppPBGRA | DXGI\_FORMAT\_B8G8R8A8\_UNORM | D2D1\_ALPHA\_MODE\_PREMULTIPLIED                                |



??

For an example that shows how to convert a WIC bitmap to a supported format, see [How to Load a Bitmap from a File](how-to-load-a-direct2d-bitmap-from-a-file.md).

## Using an Unsupported Format

Using any combination other than the pixel formats and alpha modes that are listed in the earlier tables results in a [**D2DERR\_UNSUPPORTED\_PIXEL\_FORMAT**](direct2d-error-codes.md) or an **E\_INVALIDARG** error.

## About Alpha Modes

### About Premultiplied and Straight Alpha Modes

The [**D2D1\_ALPHA\_MODE**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) enumeration indicates whether the alpha channel uses premultiplied alpha, straight alpha, or should be ignored and considered opaque. With straight alpha, the alpha channel indicates a value that corresponds to how transparent a color is.

Colors are always treated as straight alpha by Direct2D drawing commands and brushes, regardless of the destination format.

With premultiplied alpha, each color channel is scaled by the alpha value. Typically, no color channel value is greater than the alpha channel value. If a color channel value in a pre-multiplied format is greater than the alpha channel, the standard source-over blending math creates an additive blend.

The value of the alpha channel itself is the same in both straight and pre-multiplied alpha.

### The Differences Between Straight and Premultiplied Alpha

When describing an RGBA color by using straight alpha, the alpha value of the color is stored in the alpha channel. For example, to describe a red color that is 60% opaque, use the following values: (255, 0, 0, 255 \* 0.6) = (255, 0, 0, 153). The 255 value indicates full red, and 153 (which is 60 percent of 255) indicates that the color should have an opacity of 60 percent.

When describing an RGBA color by using premultiplied alpha, each color is multiplied by the alpha value: (255 \* 0.6, 0 \* 0.6, 0 \* 0.6, 255 \* 0.6) = (153, 0, 0, 153).

Regardless of the alpha mode of the render target, [**D2D1\_COLOR\_F**](d2d1-color-f.md) values are always interpreted as straight alpha. For example, when specifying the color of an [**ID2D1SolidColorBrush**](https://msdn.microsoft.com/library/Dd372207(v=VS.85).aspx) for use with a render target that uses the premultiplied alpha mode, specify the color just as you would if the render target used straight alpha. When you paint with the brush, Direct2D translates the color to the destination format for you.

### Alpha Mode for Render Targets

Regardless of the alpha mode setting, a render target's contents support transparency. For example, if you draw a partly transparent red rectangle with a render target with an alpha mode of [**D2D1\_ALPHA\_MODE\_IGNORE**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode), the rectangle will appear pink (if the background is white).

If you draw a partly transparent red rectangle when the alpha mode is [**D2D1\_ALPHA\_MODE\_PREMULTIPLIED**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode), the rectangle will appear pink (assuming the background is white) and you can see through it to whatever is behind the render target. This is useful when you use a [**ID2D1DCRenderTarget**](https://msdn.microsoft.com/library/Dd371213(v=VS.85).aspx) to render to a transparent window or when you use an compatible render target (a render targeted created by the [**CreateCompatibleRenderTarget**](/windows/desktop/api/d2d1/nf-d2d1-createcompatiblerendertarget) method) to create a bitmap that supports transparency.

### ClearType and Alpha Modes

If you specify an alpha mode other than [**D2D1\_ALPHA\_MODE\_IGNORE**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode) for a render target, the text antialiasing mode automatically changes from [**D2D1\_TEXT\_ANTIALIAS\_MODE CLEARTYPE**](/windows/desktop/api/d2d1/ne-d2d1-d2d1_text_antialias_mode) to **D2D1\_TEXT\_ANTIALIAS\_MODE GRAYSCALE**. (When you specify an alpha mode of **D2D1\_ALPHA\_MODE\_UNKNOWN**, Direct2D sets the alpha for you, depending on the kind of render target.)

You can use the [**SetTextAntialiasMode**](https://msdn.microsoft.com/library/Dd316897(v=VS.85).aspx) method to change the text antialias mode back to [**D2D1\_TEXT\_ANTIALIAS\_MODE CLEARTYPE**](/windows/desktop/api/d2d1/ne-d2d1-d2d1_text_antialias_mode), but rendering ClearType text to a transparent surface can create unpredictable results. If you want to render ClearType text to an transparent render target, we recommend that you use one of the following two techniques.

-   Use the [**PushAxisAlignedClip**](https://msdn.microsoft.com/library/Dd742855(v=VS.85).aspx) method to clip the render target to the area where the text will be rendered, then call the [**Clear**](id2d1rendertarget-clear.md) method and specify an opaque color, then render your text.
-   Use [**DrawRectangle**](https://msdn.microsoft.com/library/Dd742846(v=VS.85).aspx) to draw an opaque rectangle behind the area where the text will be rendered.

## Related topics

<dl> <dt>

[**D2D1\_PIXEL\_FORMAT**](/windows/desktop/api/dcommon/ns-dcommon-d2d1_pixel_format)
</dt> <dt>

[**D2D1\_ALPHA\_MODE**](/windows/desktop/api/dcommon/ne-dcommon-d2d1_alpha_mode)
</dt> <dt>

[DXGI\_FORMAT](https://msdn.microsoft.com/library/bb173059(VS.85).aspx)
</dt> </dl>

??

??




