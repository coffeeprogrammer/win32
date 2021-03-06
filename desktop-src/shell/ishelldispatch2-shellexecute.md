---
Description: Performs a specified operation on a specified file.
ms.assetid: a55e804c-ed7c-4b22-b86f-8e5653976654
title: IShellDispatch2.ShellExecute method (Shldisp.h)
ms.topic: reference
ms.date: 05/31/2018
topic_type: 
- APIRef
- kbSyntax
api_name: 
- IShellDispatch2.ShellExecute
api_type: 
- COM
api_location: 
- Shell32.dll
---

# IShellDispatch2.ShellExecute method

Performs a specified operation on a specified file.

## Syntax


```JScript
iRetVal = IShellDispatch2.ShellExecute(
  sFile,
  [ vArguments ],
  [ vDirectory ],
  [ vOperation ],
  [ vShow ]
)
```


```VB

IShellDispatch2.ShellExecute( _
  ByVal sFile As BSTR, _
  [ ByVal vArguments As Variant ], _
  [ ByVal vDirectory As Variant ], _
  [ ByVal vOperation As Variant ], _
  [ ByVal vShow As Variant ] _
) As Integer
```





## Parameters

<dl> <dt>

*sFile* \[in\]
</dt> <dd>

Type: **[**BSTR**](https://msdn.microsoft.com/library/ms221069(v=VS.71).aspx)**

A **String** that contains the name of the file on which **ShellExecute** will perform the action specified by *vOperation*.

</dd> <dt>

*vArguments* \[in, optional\]
</dt> <dd>

Type: **Variant**

A string that contains parameter values for the operation.

</dd> <dt>

*vDirectory* \[in, optional\]
</dt> <dd>

Type: **Variant**

The fully qualified path of the directory that contains the file specified by *sFile*. If this parameter is not specified, the current working directory is used.

</dd> <dt>

*vOperation* \[in, optional\]
</dt> <dd>

Type: **Variant**

The operation to be performed. This value is set to one of the verb strings that is supported by the file. For a discussion of verbs, see the Remarks section. If this parameter is not specified, the default operation is performed.

</dd> <dt>

*vShow* \[in, optional\]
</dt> <dd>

Type: **Variant**

A recommendation as to how the application window should be displayed initially. The application can ignore this recommendation. This parameter can be one of the following values. If this parameter is not specified, the application uses its default value.



| Value                                                                                                                               | Meaning                                                                                                                                                  |
|-------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| <dl> <dt></dt> <dt>0</dt> </dl>  | Open the application with a hidden window.<br/>                                                                                                    |
| <dl> <dt></dt> <dt>1</dt> </dl>  | Open the application with a normal window. If the window is minimized or maximized, the system restores it to its original size and position.<br/> |
| <dl> <dt></dt> <dt>2</dt> </dl>  | Open the application with a minimized window.<br/>                                                                                                 |
| <dl> <dt></dt> <dt>3</dt> </dl>  | Open the application with a maximized window.<br/>                                                                                                 |
| <dl> <dt></dt> <dt>4</dt> </dl>  | Open the application with its window at its most recent size and position. The active window remains active.<br/>                                  |
| <dl> <dt></dt> <dt>5</dt> </dl>  | Open the application with its window at its current size and position.<br/>                                                                        |
| <dl> <dt></dt> <dt>7</dt> </dl>  | Open the application with a minimized window. The active window remains active.<br/>                                                               |
| <dl> <dt></dt> <dt>10</dt> </dl> | Open the application with its window in the default state specified by the application.<br/>                                                       |



??

</dd> </dl>

## Remarks

This method is implemented and accessed through the [**Shell.ShellExecute**](https://msdn.microsoft.com/library/Gg537745(v=VS.85).aspx) method.

This method is equivalent to launching one of the commands associated with a file's shortcut menu. Each command is represented by a verb string. The set of supported verbs varies from file to file. The most commonly supported verb is "open", which is also usually the default verb. Other verbs might be supported by only certain types of files. For further discussion of Shell verbs, see [Launching Applications](launch.md) or [Extending Shortcut Menus](context.md).

This method is not currently available in Microsoft Visual Basic.

## Examples

The following examples show the use of **ShellExecute** to open Notepad. Usage is shown for JScript and VBScript.

JScript:


```JScript
<script language="JScript">
    function fnShellExecuteJ()
    {
        var objShell = new ActiveXObject("shell.application");
        
        objShell.ShellExecute("notepad.exe", "", "", "open", 1);
    }
</script>
```



VBScript:


```VB
<script language="VBScript">
    function fnShellExecuteVB()
        dim objShell

        set objShell = CreateObject("shell.application")

        objShell.ShellExecute "notepad.exe", "", "", "open", 1

        set objShell = nothing
    end function
</script>
```



## Requirements



|                                     |                                                                                                               |
|-------------------------------------|---------------------------------------------------------------------------------------------------------------|
| Minimum supported client<br/> | Windows??2000 Professional, Windows??XP \[desktop apps only\]<br/>                                        |
| Minimum supported server<br/> | Windows Server??2003 \[desktop apps only\]<br/>                                                          |
| Header<br/>                   | <dl> <dt>Shldisp.h</dt> </dl>                          |
| IDL<br/>                      | <dl> <dt>Shldisp.idl</dt> </dl>                        |
| DLL<br/>                      | <dl> <dt>Shell32.dll (version 5.0 or later)</dt> </dl> |



??

??




