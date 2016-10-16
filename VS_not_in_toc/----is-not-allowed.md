---
title: "&#39;:&#39; is not allowed"
ms.custom: na
ms.date: 10/01/2016
ms.prod: visual-studio-dev14
ms.reviewer: na
ms.suite: na
ms.technology: 
  - devlang-csharp
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 14436245-1b2e-4ca9-a139-bd1d79ed9d72
caps.latest.revision: 6
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
# &#39;:&#39; is not allowed
':' is not allowed. XML qualified names cannot be used in this context.  
  
 An XML literal has been specified with a qualified name in an invalid location, for example, a target in a processing instruction literal.  
  
 **Error ID:** BC31186  
  
### To correct this error  
  
-   Remove the XML literal with the qualified name or move it to a valid location.  
  
     For information about which characters are valid in XML names, see [Extensible Markup Language (XML) 1.0](http://go.microsoft.com/fwlink/?LinkId=73927).  
  
## See Also  
 [XML Literals](../Topic/XML%20Literals%20\(Visual%20Basic\).md)   
 [XML](../Topic/XML%20in%20Visual%20Basic.md)