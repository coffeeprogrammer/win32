---
Description: When an application calls the lineClose function with one or more outstanding asynchronous operations, TAPI may direct the service provider to terminate asynchronous operations associated with a call.
ms.assetid: b26ce074-76dc-4a50-8c17-d3412c336d59
title: Termination of Asynchronous Operations
ms.topic: article
ms.date: 05/31/2018
---

# Termination of Asynchronous Operations

When an application calls the [**lineClose**](https://msdn.microsoft.com/library/ms735573(v=VS.85).aspx) function with one or more outstanding asynchronous operations, TAPI may direct the service provider to terminate asynchronous operations associated with a call, depending on whether the application is the sole owner of the call and has the only handle to the call.

If the application is the sole owner of a call, TAPI calls [**TSPI\_lineDropOnClose**](https://msdn.microsoft.com/library/ms725545(v=VS.85).aspx) or [**TSPI\_lineDrop**](https://msdn.microsoft.com/library/ms725543(v=VS.85).aspx) (depending on the provider) for each such call. The service provider should check for any pending asynchronous operations associated with the call being dropped. If a pending operation exists, the service provider should take the appropriate action, possibly terminating the operation in progress.

If the application has the only handle to a call (that is, there are no other owners or monitors), TAPI calls the [**TSPI\_lineCloseCall**](https://msdn.microsoft.com/library/ms725532(v=VS.85).aspx) function. The service provider must consider this call to be the absolute indication that pending asynchronous operations must be abandoned. The service provider must ensure an orderly completion of the call, and must call the [**ASYNC\_COMPLETION**](https://msdn.microsoft.com/library/ms725180(v=VS.85).aspx) callback function for all outstanding asynchronous operations, specifying the LINEERR\_OPERATIONFAILED error value. If TAPI previously called the [**TSPI\_lineDropOnClose**](https://msdn.microsoft.com/library/ms725545(v=VS.85).aspx) or [**TSPI\_lineDrop**](https://msdn.microsoft.com/library/ms725543(v=VS.85).aspx) function, it calls **TSPI\_lineCloseCall** immediately after the service provider returns from the other call; it does not wait for the asynchronous operation associated with the **TSPI\_lineDrop** function to complete.

If the application is not the sole owner of the call, TAPI does not call [**TSPI\_lineDropOnClose**](https://msdn.microsoft.com/library/ms725545(v=VS.85).aspx) or [**TSPI\_lineDrop**](https://msdn.microsoft.com/library/ms725543(v=VS.85).aspx). If the application does not have the only handle to the call, TAPI does not call the [**TSPI\_lineCloseCall**](https://msdn.microsoft.com/library/ms725532(v=VS.85).aspx) function. If the application is neither the sole owner nor the sole possessor of a handle to the call, TAPI sends no notification to the service provider, and therefore, any pending asynchronous operations remain intact. This means that such applications cannot terminate asynchronous operations that they have started by using the [**lineClose**](https://msdn.microsoft.com/library/ms735573(v=VS.85).aspx) function. However, because every asynchronous operation requires the invoking application to be an owner of the call, the likelihood of an application not being able to terminate asynchronous operations is very rare. If it does occur, the other owner(s) of the call(s) must take responsibility for the disposition of the call(s).

If TAPI calls [**TSPI\_lineDropOnClose**](https://msdn.microsoft.com/library/ms725545(v=VS.85).aspx) or [**TSPI\_lineDrop**](https://msdn.microsoft.com/library/ms725543(v=VS.85).aspx), the service provider must eventually send a LINECALLSTATE\_IDLE message for the associated call (unless [**TSPI\_lineCloseCall**](https://msdn.microsoft.com/library/ms725532(v=VS.85).aspx) is called first) so that monitors on the call can clean up. When the last monitor has called the [**lineDeallocateCall**](https://msdn.microsoft.com/library/ms735599(v=VS.85).aspx) function, TAPI calls the **TSPI\_lineCloseCall** function; any pending operations must be completed or terminated and [**ASYNC\_COMPLETION**](https://msdn.microsoft.com/library/ms725180(v=VS.85).aspx) called, as described earlier.

??

??



