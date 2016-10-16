---
title: "How to: Support Toolbox Drag-and-Drop Functionality"
ms.custom: na
ms.date: 10/01/2016
ms.prod: visual-studio-dev14
ms.reviewer: na
ms.suite: na
ms.technology: 
  - devlang-csharp
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f73a03c-1eb1-4b88-9c1b-d63ba0e2ac90
caps.latest.revision: 18
manager: douge
translation.priority.ht: 
  - de-de
  - es-es
  - fr-fr
  - it-it
  - ja-jp
  - ko-kr
  - ru-ru
  - zh-cn
  - zh-tw
translation.priority.mt: 
  - cs-cz
  - pl-pl
  - pt-br
  - tr-tr
---
# How to: Support Toolbox Drag-and-Drop Functionality
> [!NOTE]
>  The recommended way to add custom controls to the Toolbox is to use the Toolbox Control templates that come with the Visual Studio 10 SDK, which include drag and drop support. This topic is retained only for backward compatibility, and for working with existing controls.  
>   
>  For more information on creating toolbox controls by using the templates, see [How to: Create a Toolbox Control That Uses Windows Forms](../VS_not_in_toc/How-to--Create-a-Toolbox-Control-That-Uses-Windows-Forms.md) and [Creating a WPF Toolbox Control](../Topic/Creating%20a%20WPF%20Toolbox%20Control.md).  
  
 VSPackages must implement drag-and-drop support if they are to use **Toolbox** controls on document views, such as editors or designers.  
  
 By default, all .NET Framework objects derived from <xref:System.Windows.Forms.Control?qualifyHint=True> automatically and transparently provide support for consuming **Toolbox** controls, and the procedures described below are unnecessary. The basic functionality can be extended by creating a designer.  
  
 For more information, see [Windows Forms Overview](../Topic/Windows%20Forms%20Overview.md) and [Extending Design-Time Support](../Topic/Extending%20Design-Time%20Support.md).  
  
### To implement basic drag-and-drop functionality  
  
1.  Provide drag-and-drop support by implementing <xref:Microsoft.VisualStudio.OLE.Interop.IDropTarget?qualifyHint=False> on a view object. This provides the view with OLE drag-and-drop functionality and serialization of the control.  
  
     For more information on implementing drag-and-drop functionality, see [Drag and Drop (OLE)](../Topic/Drag%20and%20Drop%20\(OLE\).md).  
  
     Drops can either be Clipboard items or controls being dropped on a designer. For information on standard Clipboard formats supported by the Visual Studio **Toolbox**, see [Extending the Toolbox](../VS_not_in_toc/Extending-the-Toolbox.md).  
  
2.  Provide a basic implementation of the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser?qualifyHint=False> interface on a document view.  
  
     When a user attempts to drag a toolbox control to a view, the Visual Studio shell queries the view's VSPackage to see if it implements the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser?qualifyHint=False> interface.  
  
    1.  Implement <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser.IsSupported?qualifyHint=False> to return <xref:Microsoft.VisualStudio.VSConstants.S_OK?qualifyHint=False> for those **Toolbox** formats the drop target supports. The example below checks to see if the data object is in custom Clipboard format (`CF_CUSTOM_FORMAT`) and whether the object is an ActiveX control.  
  
        ```cpp#  
        STDMETHODIMP CustTbxUser::IsSupported(IDataObject* pDO)  
        {  
            HRESULT hr;  
            STGMEDIUM stm;  
            if (!pDO)  
            return E_POINTER;  
            // Determine if the data object is in the Custom clip board format  
            // fetc is the dialog editor toolbox item template.  
            FORMATETC fetc = {   
              m_CF_CUSTOM_FORMAT, //Value set with RegisterClipboardFormat  
              NULL,   
              DVASPECT_CONTENT, // DVASCPECT_ICON might be better  
              -1,   
              TYMED_HGLOBAL   
            };  
            if (FAILED(hr = pDO->GetData(&fetc, &stm)))  
            {  
                // Determine if the object is in the class-id clipboard format ... this  
                // would be the case if the control is an activeX toolbox item.  
                FORMATETC xfetc =   
                {   
                  m_CF_CLSID,  
                  NULL,   
                  DVASPECT_CONTENT, // DVASCPECT_ICON might be better  
                  -1,   
                  TYMED_HGLOBAL   
                };  
                if (SUCCEEDED(hr = pDO->GetData(&xfetc,&stm)))  
                {  
                    USES_CONVERSION;  
                    GUID guid;  
                    LPSTR pData = (LPSTR)GlobalLock(stm.hGlobal);  
                    if (pData)  
                    {  
                        // Convert from the string format to a binary GUID.  
                        if (CLSIDFromString(A2W(pData), &guid) != S_OK)  
                            return E_FAIL;  
  
                        // HTML data objects could have CLSID format ... so any data object could   
                        // be returned as a NULL guid ... fail on null guid, obviously they are   
                        // not active X controls.  
                        if (guid == GUID_NULL)  
                            return E_FAIL;  
                    }  
                }      
            }  
  
            return hr;  
        }  
        ```  
  
         The IDE checks for this information when a view's window first loads, and for every activation of a view's window following a reset of the **Toolbox** by a user through the IDE's **CustomizeToolbox** dialog box. Typically, the **Toolbox** does not display unsupported **Toolbox** items.  
  
         Users can set an option to display all Toolbox pages at all times. In this case, the environment does not query the editor for <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser.IsSupported?qualifyHint=False>.  
  
    2.  After an <xref:Microsoft.VisualStudio.OLE.Interop.IDropTarget?qualifyHint=False> implementation (such as the one described above) successfully handles a dropped component, the view object must notify the Visual Studio environment of this by calling <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolbox2.DataUsed?qualifyHint=False>  
  
     If desired, a VSPackage can extend its drag-and-drop support by extending its <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser?qualifyHint=False> implementation.  
  
3.  An <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser?qualifyHint=False> implementation can support the dragging of **Toolbox** items to a window by selection rather than mouse action. That is, dragging an item when a user either double-clicks on a **Toolbox** item or single-clicks and then presses ENTER. To do this:  
  
    1.  Implement the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser.ItemPicked?qualifyHint=False> method.  
  
         This method, called by the IDE is selected through a click, or by pressing ENTER.  
  
         The method should insert the **Toolbox** item into the target window.  
  
    2.  If you do not wish to support implementation by selection, the method should return <xref:Microsoft.VisualStudio.VSConstants.S_FALSE?qualifyHint=False>.  
  
         In the example below, the implementation of the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxUser.ItemPicked?qualifyHint=False> method checks if the object selected is supported, and if so deserializes it into the code:  
  
        ```cpp#  
        STDMETHODIMP CustTbxUser::ItemPicked(IDataObject* pDataObject)  
        {  
            if (!pDataObject)  
                return E_POINTER;  
  
            UINT nIDTool;  
            if (IsSupported(pDataObject) == S_OK)  
            {  
                SetToolCursor();  
                m_pDataObject = pDataObject;  
                nIDTool = DeserializeObject();  
                // Get the focus back  
                m_pResObject->m_spWndFrame->Show();  
                return S_OK;  
            }  
        }  
        ```  
  
4.  Carry out the following steps to ensure that proper focus is maintained for your application:  
  
    1.  If your editor window implements two different panes, not two different views, then call the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolbox2.UpdateToolboxUI?qualifyHint=False> method when you switch pane activations within your editor window. Only you know when activation changes occur within your window.  
  
    2.  To properly activate the window and to ensure that command routing is updated properly, you must call the <xref:Microsoft.VisualStudio.Shell.Interop.IVsWindowFrame.Show?qualifyHint=False> method on the component. This action must be taken when you are setting focus to a component window, such as an editor, created or modified by a drag-and-drop operation involving the toolbox.  
  
 A VSPackage may need to intervene in a drag operation and modify the dropped item through the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxActiveUserHook?qualifyHint=False> interface.  
  
 For instance, rather than adding a new **Toolbox** control explicitly to the **Toolbox**, a VSPackage might intercept a standard **Toolbox** as it is dropped and replace it with a custom implementation.  
  
### To dynamically modify toolbox controls  
  
1.  Implement the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxActiveUserHook?qualifyHint=False> method.  
  
     Whenever a **Toolbox** item is selected or dropped, the **Toolbox** queries the drop target to see if it supports the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxActiveUserHook?qualifyHint=False> interface, and if it does calls the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxActiveUserHook.InterceptDataObject?qualifyHint=False> method.  
  
2.  The <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxActiveUserHook.InterceptDataObject?qualifyHint=False> method must return a new <xref:Microsoft.VisualStudio.OLE.Interop.IDataObject?qualifyHint=False> object to be used rather than the original <xref:Microsoft.VisualStudio.OLE.Interop.IDataObject?qualifyHint=False>.  
  
     If the drop target does not need to override the data object, it should return its input.  
  
 A VSPackage can allow users to cycle through the contents of the Clipboard by pressing CTRL+SHIFT+V.  
  
### To support a Clipboard ring  
  
1.  Implement a handler for the `CMDIDPasteNextTBXCBItem` command using:  
  
    -   <xref:Microsoft.VisualStudio.OLE.Interop.IOleCommandTarget?qualifyHint=False> for interop-assembly-based VSPackages.  
  
    -   <xref:Microsoft.VisualStudio.Shell.OleMenuCommandService?qualifyHint=False> for Managed Package Framework–based VSPackages. <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxClipboardCycler?qualifyHint=False> interface.  
  
2.  In the command handler, implement the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxClipboardCycler.AreDataObjectsAvailable?qualifyHint=False> method to determine whether there are any Clipboard objects to cycle through.  
  
    1.  If there are no items on the Toolbox Clipboard, then the environment checks the system Clipboard to see if there are any items on it.  
  
    2.  If there are items on the system Clipboard, but not on the Toolbox Clipboard, then the Clipboard ring is populated with system items.  
  
    3.  To select the next item in the list, the implementation calls the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxClipboardCycler.GetAndSelectNextDataObject?qualifyHint=False> method.  
  
    4.  To return to the beginning of the Clipboard cycle, call the <xref:Microsoft.VisualStudio.Shell.Interop.IVsToolboxClipboardCycler.BeginCycle?qualifyHint=False> method.  
  
## See Also  
 [Extending the Toolbox](../VS_not_in_toc/Extending-the-Toolbox.md)   
 [How to: Provide Custom Toolbox Items By Using Interop Assemblies](../VS_not_in_toc/How-to--Provide-Custom-Toolbox-Items-By-Using-Interop-Assemblies.md)   
 [Advanced Toolbox Control Development](../VS_not_in_toc/Advanced-Toolbox-Control-Development.md)   
 [Registering Toolbox Support Features](../VS_not_in_toc/Registering-Toolbox-Support-Features.md)   
 [Managing the Toolbox](../VS_not_in_toc/Managing-the-Toolbox.md)