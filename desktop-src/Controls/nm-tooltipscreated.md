---
title: NM\_TOOLTIPSCREATED notification code
description: Notifies a control's parent window that the control has created a tooltip control. This notification code is sent in the form of a WM\_NOTIFY message.
ms.assetid: '8108f084-212d-4a16-b604-1ec134b1bb43'
keywords: ["NM_TOOLTIPSCREATED notification code Windows Controls"]
topic_type:
- apiref
api_name:
- NM_TOOLTIPSCREATED
api_location:
- Commctrl.h
api_type:
- HeaderDef
---

# NM\_TOOLTIPSCREATED notification code

Notifies a control's parent window that the control has created a tooltip control. This notification code is sent in the form of a [**WM\_NOTIFY**](wm-notify.md) message.


```C++
NM_TOOLTIPSCREATED

    lpnmttc = (LPNMHDR) lParam; 
```



## Parameters

<dl> <dt>

*lParam* 
</dt> <dd>

A pointer to an [**NMTOOLTIPSCREATED**](nmtooltipscreated.md) structure that contains additional information about this notification.

</dd> </dl>

## Return value

Unless otherwise specified, the control ignores the return value from this notification code.

## Requirements



|                                     |                                                                                       |
|-------------------------------------|---------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows�Vista \[desktop apps only\]<br/>                                        |
| Minimum supported server<br/> | Windows Server�2003 \[desktop apps only\]<br/>                                  |
| Header<br/>                   | <dl> <dt>Commctrl.h</dt> </dl> |



�

�




