---
Description: The capabilities of the Shell can be extended with registry entries and .ini files.
ms.assetid: '74a81e4f-7357-4901-a118-ba44e8892f25'
title: Creating Shell Extension Handlers
ms.topic: article
ms.date: 05/31/2018
---

# Creating Shell Extension Handlers

The capabilities of the Shell can be extended with registry entries and .ini files. While this approach to extending the Shell is simple, and adequate for many purposes, it is limited. For example, if you use the registry to specify a custom icon for a file type, the same icon will appear for every file of that type. Extending the Shell with the registry does not allow you to vary the icon for different files of the same type. Other aspects of the Shell, such as the **Properties** property sheet that can be displayed when a file is right-clicked, cannot be modified at all with the registry.

A more powerful and flexible approach to extending the Shell is to implement *shell extension handlers*. These handlers can be implemented for a variety of actions that the Shell can perform. Before taking the action, the Shell queries the extension handler, giving it the opportunity to modify the action. A common example is a shortcut menu extension handler. If one is implemented for a file type, it will be queried every time one of the files is right-clicked. The handler can then specify additional menu items on a file-by-file basis, rather than having the same set for the entire file type.

This document discusses how to implement the extension handlers that allow you to modify a variety of Shell actions. The following handlers are associated with a particular file type and allow you to specify on a file-by-file basis:



| Handler                                               | Description                                                                                                                                                                |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Shortcut menu handler](context-menu-handlers.md)    | Called before a file's shortcut menu is displayed. It enables you to add items to the shortcut menu on a file-by-file basis.                                               |
| [Data handler](how-to-create-data-handlers.md)       | Called when a drag-and-drop operation is performed on dragShell objects. It enables you to provide additional clipboard formats to the drop target.                        |
| [Drop handler](how-to-create-drop-handlers.md)       | Called when a data object is dragged over or dropped on a file. It enables you to make a file into a drop target.                                                          |
| [Icon handler](how-to-create-icon-handlers.md)       | Called before a file's icon is displayed. It enables you to replace the file's default icon with a custom icon on a file-by-file basis.                                    |
| [Property sheet handler](propsheet-handlers.md)      | Called before an object's **Properties** property sheet is displayed. It enables you to add or replace pages.                                                              |
| [**Thumbnail Image handler**](/windows/desktop/api/Thumbcache/nn-thumbcache-ithumbnailprovider) | Provides an image to represent the item.                                                                                                                                   |
| [**Infotip handler**](https://msdn.microsoft.com/library/Bb761359(v=VS.85).aspx)                 | Provides pop-up text when the user hovers the mouse pointer over the object.                                                                                               |
| [**Metadata handler**](https://msdn.microsoft.com/library/Bb761474(v=VS.85).aspx)     | Provides read and write access to metadata (properties) stored in a file. This can be used to extend the Details view, infotips, the property page, and grouping features. |



??

Other handlers are not associated with a particular file type but are called before some Shell operations:



| Handler                                                            | Description                                                                                                                                  |
|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [Column handler](https://msdn.microsoft.com/library/Bb776831(v=VS.85).aspx)                             | Called by Windows Explorer before it displays the Details view of a folder. It enables you to add custom columns to the Details view.        |
| [Copy hook handler](how-to-create-copy-hook-handlers.md)          | Called when a folder or printer object is about to be moved, copied, deleted, or renamed. It enables you to approve or veto the operation.   |
| [Drag-and-drop handler](context-menu-handlers.md)                 | Called when a file is dragged with the right mouse button. It enables you to modify the shortcut menu that is displayed.                     |
| [Icon Overlay handler](how-to-implement-icon-overlay-handlers.md) | Called before a file's icon is displayed. It enables you to specify an overlay for the file's icon.                                          |
| [Search handler](https://msdn.microsoft.com/library/Bb776834(v=VS.85).aspx)                             | Called to launch a search engine. It enables you to implement a custom search engine accessible from the **Start** menu or Windows Explorer. |



??

The details of how to implement specific extension handlers are covered in the sections listed above. The remainder of this document covers some implementation issues that are common to all Shell extension handlers.

-   [Implementing Shell Extension Handlers](#implementing-shell-extension-handlers)
    -   [Implementing IPersistFile](#implementing-ipersistfile)
    -   [Implementing IShellExtInit](#implementing-ishellextinit)
    -   [Infotip Customization](#infotip-customization)
-   [Enhancing Windows Search with Shell Extension Handlers](#enhancing-windows-search-with-shell-extension-handlers)
-   [Registering Shell Extension Handlers](#registering-shell-extension-handlers)
    -   [Handler Names](#handler-names)
    -   [Predefined Shell Objects](#predefined-shell-objects)
    -   [Example of an Extension Handler Registration](#example-of-an-extension-handler-registration)
-   [Related topics](#related-topics)

## Implementing Shell Extension Handlers

Much of the implementation of a Shell extension handler object depends on its type. There are, however, some common elements. This section discusses those aspects of implementation that are shared by all Shell extension handlers.

All Shell extension handlers are in-process Component Object Model (COM) objects. They must be assigned a GUID and registered as described in Registering Shell Extension Handlers. They are implemented as DLLs and must export the following standard functions:

-   [**DllMain**](https://msdn.microsoft.com/library/ms682583(v=VS.85).aspx). The standard entry point to the DLL.
-   [**DllGetClassObject**](https://msdn.microsoft.com/library/ms680760(v=VS.85).aspx). Exposes the object's class factory.
-   [**DllCanUnloadNow**](https://msdn.microsoft.com/library/ms690368(v=VS.85).aspx). COM calls this function to determine whether the object is serving any clients. If not, the system can unload the DLL and free the associated memory.

Like all COM objects, Shell extension handlers must implement an [**IUnknown**](https://msdn.microsoft.com/library/ms680509(v=VS.85).aspx) interface and a [class factory](https://msdn.microsoft.com/library/ms690074(v=VS.85).aspx). Most extension handlers must also implement either an [**IPersistFile**](https://msdn.microsoft.com/library/ms687223(v=VS.85).aspx) or [**IShellExtInit**](https://msdn.microsoft.com/library/Bb775096(v=VS.85).aspx) interface in Windows??XP or earlier. These were replaced by [**IInitializeWithStream**](/windows/desktop/api/Propsys/nn-propsys-iinitializewithstream), [**IInitializeWithItem**](https://msdn.microsoft.com/library/Bb761814(v=VS.85).aspx) and [**IInitializeWithFile**](/windows/desktop/api/Propsys/nn-propsys-iinitializewithfile) in Windows??Vista. The Shell uses these interfaces to initialize the handler.

The [**IPersistFile**](https://msdn.microsoft.com/library/ms687223(v=VS.85).aspx) interface must be implemented by the following:

-   Data handlers
-   Drop handlers

In the past, icon handlers were also required to implement [**IPersistFile**](https://msdn.microsoft.com/library/ms687223(v=VS.85).aspx), but this is no longer true. For icon handlers, **IPersistFile** is now optional and other interfaces such as [**IInitializeWithItem**](https://msdn.microsoft.com/library/Bb761814(v=VS.85).aspx) are preferred.

The [**IShellExtInit**](https://msdn.microsoft.com/library/Bb775096(v=VS.85).aspx) interface must be implemented by the following:

-   Shortcut menu handlers
-   Drag-and-drop handlers
-   Property sheet handlers

### Implementing IPersistFile

The [**IPersistFile**](https://msdn.microsoft.com/library/ms687223(v=VS.85).aspx) interface is intended to permit an object to be loaded from or saved to a disk file. It has six methods in addition to [**IUnknown**](https://msdn.microsoft.com/library/ms680509(v=VS.85).aspx), five of its own, and the [**GetClassID**](https://msdn.microsoft.com/library/ms688664(v=VS.85).aspx) method that it inherits from [**IPersist**](https://msdn.microsoft.com/library/ms688695(v=VS.85).aspx). With Shell extensions, **IPersist** is used only to initialize a Shell extension handler object. Because there is typically no need to read from or write to the disk, only the **GetClassID** and [**Load**](https://msdn.microsoft.com/library/ms687284(v=VS.85).aspx) methods require a nontoken implementation.

The Shell calls [**GetClassID**](https://msdn.microsoft.com/library/ms688664(v=VS.85).aspx) first, and the function returns the class identifier (CLSID) of the extension handler object. The Shell then calls [**Load**](https://msdn.microsoft.com/library/ms687284(v=VS.85).aspx) and passes in two values. The first, *pszFileName*, is a Unicode string with the name of the file or folder that Shell is about to operate on. The second is *dwMode*, which indicates the file access mode. Because there is typically no need to access files, *dwMode* is usually zero. The method stores these values as needed for later reference.

The following code fragment illustrates how a typical Shell extension handler implements the [**GetClassID**](https://msdn.microsoft.com/library/ms688664(v=VS.85).aspx) and [**Load**](https://msdn.microsoft.com/library/ms687284(v=VS.85).aspx) methods. It is designed to handle either ANSI or Unicode. CLSID\_SampleExtHandler is the extension handler object's GUID, and CSampleExtHandler is the name of the class used to implement the interface. The **m\_szFileName** and **m\_dwMode** variables are private variables that are used to store the file's name and access flags.


```C++
TCHAR m_szFileName[MAX_PATH];    // The file name
DWORD m_dwMode;                  // The file access mode

CSampleExtHandler::GetClassID(CLSID *pCLSID)
{
    *pCLSID = CLSID_SampleExtHandler;
}

CSampleExtHandler::Load(PCWSTR pszFile, DWORD dwMode)
{
    m_dwMode = dwMode;
    return StringCchCopy(_szFileName, ARRAYSIZE(m_szFileName), pszFile);
}
```



### Implementing IShellExtInit

The [**IShellExtInit**](https://msdn.microsoft.com/library/Bb775096(v=VS.85).aspx) interface has only one method, [**IShellExtInit::Initialize**](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ishellextinit-initialize), in addition to [**IUnknown**](https://msdn.microsoft.com/library/ms680509(v=VS.85).aspx). The method has three parameters that the Shell can use to pass in various types of information. The values passed in depend on the type of handler, and some can be set to **NULL**.

-   *pIDFolder* holds a folder's pointer to an item identifier list (PIDL). For property sheet extensions, it is **NULL**. For shortcut menu extensions, it is the PIDL of the folder that contains the item whose shortcut menu is being displayed. For nondefault drag-and-drop handlers, it is the PIDL of the target folder.
-   *pDataObject* holds a pointer to a data object's [**IDataObject**](https://msdn.microsoft.com/library/ms688421(v=VS.85).aspx) interface. The data object holds one or more file names in [CF\_HDROP](dragdrop.md) format.
-   *hRegKey* holds a registry key for the file object or folder type.

The [**IShellExtInit::Initialize**](/windows/desktop/api/shobjidl_core/nf-shobjidl_core-ishellextinit-initialize) method stores the file name, [**IDataObject**](https://msdn.microsoft.com/library/ms688421(v=VS.85).aspx) pointer, and registry key as needed for later use. The following code fragment illustrates an implementation of **IShellExtInit::Initialize**. For simplicity, this example assumes that the data object contains only a single file. In general, it might contain multiple files that will each need to be extracted.


```C++
LPCITEMIDLIST  m_pIDFolder;           //The folder's PIDL
TCHAR          m_szFile[MAX_PATH];    //The file name
IDataObject   *m_pDataObj;            //The IDataObject pointer
HKEY           m_hRegKey;             //The file or folder's registry key

STDMETHODIMP CShellExt::Initialize(LPCITEMIDLIST pIDFolder, 
                                   IDataObject *pDataObj, 
                                   HKEY hRegKey) 
{ 
    // If Initialize has already been called, release the old PIDL
    ILFree(m_pIDFolder);
    m_pIDFolder = NULL;

    //Store the new PIDL.
    if(pIDFolder)
    {
        m_pIDFolder = ILClone(pIDFolder);
    }
    
    // If Initialize has already been called, release the old
    // IDataObject pointer.
    if (m_pDataObj)
    { 
        m_pDataObj->Release(); 
    }
     
    // If a data object pointer was passed in, save it and
    // extract the file name. 
    if (pDataObj) 
    { 
        m_pDataObj = pDataObj; 
        pDataObj->AddRef(); 
      
        STGMEDIUM   medium;
        FORMATETC   fe = {CF_HDROP, NULL, DVASPECT_CONTENT, -1, TYMED_HGLOBAL};
        UINT        uCount;

        if(SUCCEEDED(m_pDataObj->GetData(&fe, &medium)))
        {
            // Get the count of files dropped.
            uCount = DragQueryFile((HDROP)medium.hGlobal, (UINT)-1, NULL, 0);

            // Get the first file name from the CF_HDROP.
            if(uCount)
                DragQueryFile((HDROP)medium.hGlobal, 0, m_szFile, 
                              sizeof(m_szFile)/sizeof(TCHAR));

            ReleaseStgMedium(&medium);
        }
    }

    // Duplicate the registry handle. 
    if (hRegKey) 
        RegOpenKeyEx(hRegKey, 
                     NULL, 
                     0L, 
                     MAXIMUM_ALLOWED, 
                     &m_hRegKey); 
    return S_OK; 
}
```



CSampleExtHandler is the name of the class used to implement the interface. The **m\_pIDFolder**, **m\_pDataObject**, **m\_szFileName**, and **m\_hRegKey** variables are private variables used to store the information that is passed in. For simplicity, this example assumes that only one file name will be held by the data object. After the [**FORMATETC**](https://msdn.microsoft.com/library/ms682177(v=VS.85).aspx) structure is retrieved from the data object, [**DragQueryFile**](/windows/desktop/api/Shellapi/nf-shellapi-dragqueryfilea) is used to extract the file name from the **FORMATETC** structure's **medium.hGlobal** member. If a registry key is passed in, the method uses [**RegOpenKeyEx**](https://msdn.microsoft.com/library/ms724897(v=VS.85).aspx) to open the key and assigns the handle to **m\_hRegKey**.

### Infotip Customization

There are two ways to customize infotips:

-   Implement an object that supports [**IQueryInfo**](https://msdn.microsoft.com/library/Bb761359(v=VS.85).aspx) and then register that object under the proper subkey in the registry (see [Registering Shell Extension Handlers](#registering-shell-extension-handlers) below).
-   Specify a fixed string or a list of specific file properties to be displayed.

To display a fixed string for a namespace extension, create an entry called `InfoTip` in the *{CLSID}* key of your namespace extension. Set the value of that entry to be either the literal string you want to display, as shown in this example, or an indirect string that specifies a resource and index within that resource (for localization purposes).

```
HKEY_CLASSES_ROOT
??????CLSID
????????????{CLSID}
??????????????????InfoTip = InfoTip string for your namespace extension
```

To display a fixed string for a file type, create an entry called `InfoTip` in the *ProgID* key of that file type. Set the value of that entry to be either the literal string you want to display or an indirect string that specifies a resource and index within that resource (for localization purposes), as shown in this example.

```
HKEY_CLASSES_ROOT
??????ProgID
????????????InfoTip = Resource.dll, 3
```

If you want the Shell to display specific file properties in the infotip for a specific file type, create an entry called `InfoTip` in the *ProgID* key for that file type. Set the value of that entry to be a semicolon-delineated list of canonical property names, format identifier (FMTID)/property identifier (PID) pairs, or both. This value must begin with "prop:" to identify it as a property list string. If you omit "prop:", the value is seen as a literal string and displayed as such.

In the following example, *propname* is a canonical property name (such as System.Date) and *{fmtid},pid* is an [**FMTID/PID**](https://msdn.microsoft.com/library/Bb759748(v=VS.85).aspx) pair.

```
HKEY_CLASSES_ROOT
??????ProgID
????????????InfoTip = prop:propname;propname;{fmtid},pid;{fmtid},pid
```

The following property names can be used:



| Property Name    | Description                   | Retrieved From                                                                             |
|------------------|-------------------------------|--------------------------------------------------------------------------------------------|
| Author           | Author of the document        | [**PIDSI\_AUTHOR**](https://msdn.microsoft.com/library/Aa380376(v=VS.85).aspx)                              |
| Title            | Title of the document         | [**PIDSI\_TITLE**](https://msdn.microsoft.com/library/Aa380376(v=VS.85).aspx)                               |
| Subject          | Subject summary               | [**PIDSI\_SUBJECT**](https://msdn.microsoft.com/library/Aa380376(v=VS.85).aspx)                             |
| Comment          | Document comments             | [**PIDSI\_COMMENT**](https://msdn.microsoft.com/library/Aa380376(v=VS.85).aspx) or folder/driver properties |
| PageCount        | Number of pages               | [**PIDSI\_PAGECOUNT**](https://msdn.microsoft.com/library/Aa380376(v=VS.85).aspx)                           |
| Name             | Friendly name                 | Standard folder view                                                                       |
| OriginalLocation | Location of original file     | Briefcase folder and Recycle Bin folder                                                    |
| DateDeleted      | Date file was deleted         | Recycle Bin folder                                                                         |
| Type             | Type of file                  | Standard folder details view                                                               |
| Size             | Size of file                  | Standard folder details view                                                               |
| SyncCopyIn       | Same as OriginalLocation      | Same as OriginalLocation                                                                   |
| Modified         | Date last modified            | Standard folder details view                                                               |
| Created          | Date created                  | Standard folder details view                                                               |
| Accessed         | Date last accessed            | Standard folder details view                                                               |
| InFolder         | Directory containing the file | Document search results                                                                    |
| Rank             | Quality of search match       | Document search results                                                                    |
| FreeSpace        | Available storage space       | Disk drives                                                                                |
| NumberOfVisits   | Number of visits              | Favorites folder                                                                           |
| Attributes       | File Attributes               | Standard folder details view                                                               |
| Company          | Company name                  | [**PIDDSI\_COMPANY**](https://msdn.microsoft.com/library/Aa380374(v=VS.85).aspx)    |
| Category         | Document category             | [**PIDDSI\_CATEGORY**](https://msdn.microsoft.com/library/Aa380374(v=VS.85).aspx)   |
| Copyright        | Media copyright               | [**PIDMSI\_COPYRIGHT**](https://msdn.microsoft.com/library/Aa380374(v=VS.85).aspx)  |
| HTMLInfoTipFile  | HTML InfoTip file             | Desktop.ini file for folder                                                                |



??

## Enhancing Windows Search with Shell Extension Handlers

Shell extension handlers may be used to enhance the user experience provided by a Windows Search protocol handler. To enable such enhancements, the supporting Shell extension handler must be designed to integrate with the search protocol handler as a data source. For information about how to enhance a Windows Search protocol handler through integration with a Shell extension handler, see [Adding Icons, Previews and Shortcut Menus](https://msdn.microsoft.com/library/Bb266530(v=VS.85).aspx). For more information about Windows Search protocol handlers, see [Developing Protocol Handlers](https://msdn.microsoft.com/library/Bb266531(v=VS.85).aspx).

## Registering Shell Extension Handlers

A Shell extension handler object must be registered before the Shell can use it. This section is a general discussion of how to register a Shell extension handler.

Any time you create or change a Shell extension handler, it is important to notify the system that you have made a change with [**SHChangeNotify**](/windows/desktop/api/shlobj_core/nf-shlobj_core-shchangenotify), specifying the **SHCNE\_ASSOCCHANGED** event. If you do not call **SHChangeNotify**, the change might not be recognized until the system is rebooted.

As with all COM objects, you must create a GUID for the handler using a tool such as UUIDGEN.exe. Create a key under **HKEY\_CLASSES\_ROOT**\\**CLSID** whose name is the string form of the GUID. Because Shell extension handlers are in-process servers, you must create an **InProcServer32** key under the GUID key with the default value set to the path of the handler's DLL. Use the Apartment threading model.

Any time the Shell takes an action that can involve a Shell extension handler, it checks the appropriate registry key. The key under which an extension handler is registered thus controls when it will be called. For instance, it is a common practice to have a shortcut menu handler called when the Shell displays a shortcut menu for a member of a [file type](fa-file-types.md). In this case, the handler must be registered under the file type's **ProgID** key.

### Handler Names

To enable a Shell extension handler, create a subkey with the handler subkey name (see below) under the **ShellEx** subkey of either the **ProgID** (for file types) or the Shell object type name (for [Predefined Shell Objects](#predefined-shell-objects)).

For example, if you wanted to register a shortcut menu extension handler for MyProgram.1, you would begin by creating the following subkey:

```
HKEY_CLASSES_ROOT
??????MyProgram.1
????????????ShellEx
??????????????????ContextMenuHandlers
```

For the following handlers, create a subkey underneath the "Handler Subkey name" key whose name is the string version of the CLSID of the Shell extension. Multiple extensions can be registered under the handler subkey name key by creating multiple subkeys.



| Handler                                               | Interface          | Handler Subkey Name       |
|-------------------------------------------------------|--------------------|---------------------------|
| Shortcut menu handler                                 | IContextMenu       | **ContextMenuHandlers**   |
| Copyhook handler                                      | ICopyHook          | **CopyHookHandlers**      |
| Drag-and-drop handler                                 | IContextMenu       | **DragDropHandlers**      |
| Property sheet handler                                | IShellPropSheetExt | **PropertySheetHandlers** |
| Column provider handler (deprecated in Windows??Vista) | IColumnProvider    | **ColumnHandlers**        |



??

For the following handlers, the default value of the "Handler Subkey Name" key is the string version of the CLSID of the Shell extension. Only one extension can be registered for these handlers.



| Handler                 | Interface                                         | Handler Subkey Name                        |
|-------------------------|---------------------------------------------------|--------------------------------------------|
| Data handler            | IDataObject                                       | **DataHandler**                            |
| Drop handler            | IDropTarget                                       | **DropHandler**                            |
| Icon handler            | IExtractIconA/W                                   | **IconHandler**                            |
| Image handler           | IExtractImage                                     | **{BB2E617C-0920-11d1-9A0B-00C04FC2D6C1}** |
| Thumbnail image handler | IThumbnailProvider                                | **{E357FCCD-A995-4576-B01F-234630154E96}** |
| Infotip handler         | IQueryInfo                                        | **{00021500-0000-0000-C000-000000000046}** |
| Shell link (ANSI )      | IShellLinkA                                       | **{000214EE-0000-0000-C000-000000000046}** |
| Shell link (UNICODE)    | IShellLinkW                                       | **{000214F9-0000-0000-C000-000000000046}** |
| Structured storage      | IStorage                                          | **{0000000B-0000-0000-C000-000000000046}** |
| Metadata                | IPropertyStore                                    | **PropertyHandler**                        |
| Metadata                | IPropertySetStorage (deprecated in Windows??Vista) | **PropertyHandler**                        |
| Pin to Start Menu       | IStartMenuPinnedList                              | **{a2a9545d-a0c2-42b4-9708-a0b2badd77c8}** |
| Pin to Taskbar          |                                                   | **{90AA3A4E-1CBA-4233-B8BB-535773D48449}** |



??

The subkeys specified to add **Pin to Start Menu** and **Pin to Taskbar** to an item's shortcut menu are only required for file types that include the [IsShortCut](https://msdn.microsoft.com/library/Bb776891(v=VS.85).aspx) entry.

Support for column provider handlers was removed in Windows??Vista. Also, as of Windows??Vista, [**IPropertySetStorage**](https://msdn.microsoft.com/library/Aa379840(v=VS.85).aspx) has been deprecated in favor of [**IPropertyStore**](https://msdn.microsoft.com/library/Bb761474(v=VS.85).aspx).

While [**IExtractImage**](/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iextractimage) remains supported, [**IThumbnailProvider**](/windows/desktop/api/Thumbcache/nn-thumbcache-ithumbnailprovider) is preferred for Windows??Vista and later.

### Predefined Shell Objects

The Shell defines additional objects under **HKEY\_CLASSES\_ROOT** which can be extended in the same way as file types. For example, to add a property sheet handler for all files, you can register under the **PropertySheetHandlers** key.

```
HKEY_CLASSES_ROOT
??????*
????????????shellex
??????????????????PropertySheetHandlers
```

The following table gives the various subkeys of **HKEY\_CLASSES\_ROOT** under which extension handlers can be registered. Note that many extension handlers cannot be registered under all of the listed subkeys. For further details, see the specific handler's documentation.



| Subkey                    | Description                                                          | Possible Handlers                                | Version |
|---------------------------|----------------------------------------------------------------------|--------------------------------------------------|---------|
| **\***                    | All files                                                            | Shortcut Menu, Property Sheet, Verbs (see below) | All     |
| **AllFileSystemObjects**  | All files and file folders                                           | Shortcut Menu, Property Sheet, Verbs             | 4.71    |
| **Folder**                | All folders                                                          | Shortcut Menu, Property Sheet, Verbs             | All     |
| **Directory**             | File folders                                                         | Shortcut Menu, Property Sheet, Verbs             | All     |
| **Directory\\Background** | File folder background                                               | Shortcut Menu only                               | 4.71    |
| **Drive**                 | All drives in MyComputer, such as "C:\\"                             | Shortcut Menu, Property Sheet, Verbs             | All     |
| **Network**               | Entire network (under My Network Places)                             | Shortcut Menu, Property Sheet, Verbs             | All     |
| **Network\\Type\\\#**     | All objects of type \# (see below)                                   | Shortcut menu, Property Sheet, Verbs             | 4.71    |
| **NetShare**              | All network shares                                                   | Shortcut menu, Property Sheet, Verbs             | 4.71    |
| **NetServer**             | All network servers                                                  | Shortcut menu, Property Sheet, Verbs             | 4.71    |
| *network\_provider\_name* | All objects provided by network provider "*network\_provider\_name*" | Shortcut menu, Property Sheet, Verbs             | All     |
| **Printers**              | All printers                                                         | Shortcut Menu, Property Sheet                    | All     |
| **AudioCD**               | Audio CD in CD drive                                                 | Verbs only                                       | All     |
| **DVD**                   | DVD drive (Windows 2000)                                             | Shortcut Menu, Property Sheet, Verbs             | 4.71    |



??

Notes:

-   The file folder background shortcut menu is accessed by right-clicking within a file folder, but not over any of the folder's contents.
-   "Verbs" are special commands registered under **HKEY\_CLASSES\_ROOT**\\*Subkey*\\**Shell**\\**Verb** .
-   For **Network**\\**Type**\\**\#** , "\#" is a network provider type code in decimal. The network provider type code is the high word of a network type. The list of network types is given in the Winnetwk.h header file (WNNC\_NET\_\* values). For example, WNNC\_NET\_SHIVA is 0x00330000, so the corresponding type key would be **HKEY\_CLASSES\_ROOT**\\**Network**\\**Type**\\**51** .
-   "*network\_provider\_name*" is a network provider name as specified by [**WNetGetProviderName**](https://msdn.microsoft.com/library/Aa385464(v=VS.85).aspx), with the spaces converted into underscores. For example, if the Microsoft Networking network provider is installed, its provider name is "Microsoft Windows Network", and the corresponding *network\_provider\_name* is **Microsoft\_Windows\_Network**.

### Example of an Extension Handler Registration

To enable a particular handler, create a subkey under the extension handler type key with the name of the handler. The Shell does not use the handler's name, but it must be different from all other names under that type subkey. Set the default value of the name subkey to the string form of the handler's GUID.

The following example illustrates registry entries that enable shortcut menu and property sheet extension handlers, using an example .myp file type:

```
HKEY_CLASSES_ROOT
??????.myp
????????????(Default) = MyProgram.1
??????CLSID
????????????{00000000-1111-2222-3333-444444444444}
??????????????????InProcServer32
????????????????????????(Default) = C:\MyDir\MyCommand.dll
????????????????????????ThreadingModel = Apartment
????????????{11111111-2222-3333-4444-555555555555}
??????????????????InProcServer32
????????????????????????(Default) = C:\MyDir\MyPropSheet.dll
????????????????????????ThreadingModel = Apartment
??????MyProgram.1
????????????(Default) = MyProgram Application
????????????Shellex
??????????????????ContextMenuHandler
????????????????????????MyCommand
??????????????????????????????(Default) = {00000000-1111-2222-3333-444444444444}
??????????????????PropertySheetHandlers
????????????????????????MyPropSheet
??????????????????????????????(Default) = {11111111-2222-3333-4444-555555555555}
```

The registration procedure discussed in this section must be followed for all Windows systems.

## Related topics

<dl> <dt>

[Guidance for Implementing In-Process Extensions](shell-and-managed-code.md)
</dt> </dl>

??

??



