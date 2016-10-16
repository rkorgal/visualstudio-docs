---
title: "&#39;&lt;method1&gt;&#39; and &#39;&lt;method2&gt;&#39; cannot overload each other because they differ only by parameters declared &#39;ParamArray&#39;"
ms.custom: na
ms.date: 10/01/2016
ms.prod: visual-studio-dev14
ms.reviewer: na
ms.suite: na
ms.technology: 
  - devlang-csharp
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6111df0c-fc3e-40b2-b536-effbd132ef72
caps.latest.revision: 8
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
# &#39;&lt;method1&gt;&#39; and &#39;&lt;method2&gt;&#39; cannot overload each other because they differ only by parameters declared &#39;ParamArray&#39;
You have attempted to overload two methods that differ from each other only by a `ParamArray` parameter or parameters. The compiler considers a procedure with a `ParamArray` parameter to have an infinite number of overloads differing from each other in what is passed to the parameter array.  
  
 **Error ID:** BC30368  
  
### To correct this error  
  
-   Make sure the methods are differentiated by more than the `ParamArray` parameters.  
  
## See Also  
 [Considerations in Overloading Procedures](../Topic/Considerations%20in%20Overloading%20Procedures%20\(Visual%20Basic\).md)   
 [Parameter Arrays](../Topic/Parameter%20Arrays%20\(Visual%20Basic\).md)