---
title: "&#39;&lt;member&gt;&#39;, implicitly defined for &#39;&lt;eventname&gt;&#39;, cannot shadow a &#39;MustOverride&#39; method in the base &lt;class&gt; &#39;&lt;classname&gt;&#39;"
ms.custom: na
ms.date: 10/01/2016
ms.prod: visual-studio-dev14
ms.reviewer: na
ms.suite: na
ms.technology: 
  - devlang-csharp
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 071706ce-a394-48b6-9afa-751cb50b2576
caps.latest.revision: 11
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
# &#39;&lt;member&gt;&#39;, implicitly defined for &#39;&lt;eventname&gt;&#39;, cannot shadow a &#39;MustOverride&#39; method in the base &lt;class&gt; &#39;&lt;classname&gt;&#39;
The specified event implicitly declares a member with the same name as a method declared with the `MustOverride` modifier.  
  
 **Error ID:** BC31413  
  
### To correct this error  
  
-   Remove the `MustOverride` modifier from the method in the base class or give the property or method a unique name.  
  
## See Also  
 [MustOverride](../Topic/MustOverride%20\(Visual%20Basic\).md)   
 [Events](../Topic/Events%20\(Visual%20Basic\).md)