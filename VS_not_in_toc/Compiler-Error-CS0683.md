---
title: "Compiler Error CS0683"
ms.custom: na
ms.date: 10/02/2016
ms.devlang: 
  - CSharp
ms.prod: visual-studio-dev14
ms.reviewer: na
ms.suite: na
ms.technology: 
  - devlang-csharp
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04cca66d-8a0b-469a-b157-9c8ece368c48
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
# Compiler Error CS0683
"'explicitmethod' explicit method implementation cannot implement 'method' because it is an accessor  
  
 The following sample generates CS0683:  
  
```  
// CS0683.cs  
interface IExample  
{  
   int Test { get; }  
}  
  
class CExample : IExample  
{  
   int IExample.get_Test() { return 0; } // CS0683  
   int IExample.Test { get{ return 0; } } // correct  
}  
```