---
Description: Windows GDI+ draws lines, rectangles, and other figures on a coordinate system.
ms.assetid: a43bcb27-473b-4ca2-a937-2b54384ab86b
title: Overview of Vector Graphics
ms.topic: article
ms.date: 05/31/2018
---

# Overview of Vector Graphics

Windows GDI+ draws lines, rectangles, and other figures on a coordinate system. You can choose from a variety of coordinate systems, but the default coordinate system has the origin in the upper left corner with the x-axis pointing to the right and the y-axis pointing down. The unit of measure in the default coordinate system is the pixel.

![illustration of a coordinate system with the x-axis extending to the right and the y-axis extending down](images/aboutgdip02-art01.png)

A computer monitor creates its display on a rectangular array of dots called picture elements or pixels. The number of pixels appearing on the screen varies from one monitor to the next, and the number of pixels appearing on an individual monitor can usually be configured to some extent by the user.

![illustration of a rectangular grid, with three cells in that grid labeled by their coordinates](images/aboutgdip02-art02.png)

When you use GDI+ to draw a line, rectangle, or curve, you provide certain key information about the item to be drawn. For example, you can specify a line by providing two points, and you can specify a rectangle by providing a point, a height, and a width. GDI+ works in conjunction with the display driver software to determine which pixels must be turned on to show the line, rectangle, or curve. The following illustration shows the pixels that are turned on to display a line from the point (4, 2) to the point (12, 8).

![illustration showing a rectangular grid with cells filled to indicate a line between two endpoints](images/aboutgdip02-art03.png)

Over time, certain basic building blocks have proven to be the most useful for creating two-dimensional pictures. These building blocks, which are all supported by GDI+, are given in the following list:

-   Lines
-   Rectangles
-   Ellipses
-   Arcs
-   Polygons
-   Cardinal splines
-   B??zier splines

The [**Graphics**](/windows/desktop/api/gdiplusgraphics/nl-gdiplusgraphics-graphics) class in GDI+ provides the following methods for drawing the items in the previous list: [DrawLine](https://msdn.microsoft.com/library/ms535748(v=VS.85).aspx), [DrawRectangle](https://msdn.microsoft.com/library/ms535755(v=VS.85).aspx), [DrawEllipse](https://msdn.microsoft.com/library/ms535744(v=VS.85).aspx), [DrawPolygon](https://msdn.microsoft.com/library/ms535753(v=VS.85).aspx), [DrawArc](https://msdn.microsoft.com/library/ms535733(v=VS.85).aspx), [DrawCurve](https://msdn.microsoft.com/library/ms535742(v=VS.85).aspx) (for cardinal splines), and [DrawBezier](https://msdn.microsoft.com/library/ms535734(v=VS.85).aspx). Each of these methods is overloaded; that is, each method comes in several variations with different parameter lists. For example, one variation of the DrawLine method receives the address of a [**Pen**](/windows/desktop/api/gdipluspen/nl-gdipluspen-pen) object and four integers, while another variation of the DrawLine method receives the address of a **Pen** object and two [**Point**](/windows/desktop/api/gdiplustypes/nl-gdiplustypes-point) object references.

The methods for drawing lines, rectangles, and B??zier splines have plural companion methods that draw several items in a single call: [DrawLines](https://msdn.microsoft.com/library/ms535749(v=VS.85).aspx), [DrawRectangles](https://msdn.microsoft.com/library/ms535757(v=VS.85).aspx), and [DrawBeziers](https://msdn.microsoft.com/library/ms535738(v=VS.85).aspx). Also, the [DrawCurve](https://msdn.microsoft.com/library/ms535742(v=VS.85).aspx) method has a companion method, [DrawClosedCurve](https://msdn.microsoft.com/library/ms535740(v=VS.85).aspx), that closes a curve by connecting the ending point of the curve to the starting point.

All the drawing methods of the [**Graphics**](/windows/desktop/api/gdiplusgraphics/nl-gdiplusgraphics-graphics) class work in conjunction with a [**Pen**](/windows/desktop/api/gdipluspen/nl-gdipluspen-pen) object. Thus, in order to draw anything, you must create at least two objects: a **Graphics** object and a **Pen** object. The **Pen** object stores attributes of the item to be drawn, such as line width and color. The address of the **Pen** object is passed as one of the arguments to the drawing method. For example, one variation of the [DrawRectangle](https://msdn.microsoft.com/library/ms535755(v=VS.85).aspx) method receives the address of a **Pen** object and four integers as shown in the following code, which draws a rectangle with a width of 100, a height of 50 and an upper-left corner of (20, 10).


```
myGraphics.DrawRectangle(&myPen, 20, 10, 100, 50);
```



??

??



