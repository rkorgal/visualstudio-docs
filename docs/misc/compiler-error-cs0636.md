---
title: "Compiler Error CS0636"
ms.custom: na
ms.date: "10/13/2016"
ms.prod: "visual-studio-dev14"
ms.reviewer: na
ms.suite: na
ms.technology: 
  - "devlang-csharp"
ms.tgt_pltfrm: na
ms.topic: "article"
f1_keywords: 
  - "CS0636"
dev_langs: 
  - "CSharp"
helpviewer_keywords: 
  - "CS0636"
ms.assetid: 47597f89-fbe6-4708-9493-3c86c6f0ce56
caps.latest.revision: 6
ms.author: "billchi"
manager: "douge"
translation.priority.ht: 
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "ru-ru"
  - "zh-cn"
  - "zh-tw"
translation.priority.mt: 
  - "cs-cz"
  - "pl-pl"
  - "pt-br"
  - "tr-tr"
---
# Compiler Error CS0636
The FieldOffset attribute can only be placed on members of types marked with the StructLayout(LayoutKind.Explicit)  
  
 You must use the **StructLayout(LayoutKind.Explicit)** attribute on the struct declaration, if it contains any members marked with the **FieldOffset** attribute. For more information, see [FieldOffset](frlrfsystemruntimeinteropservicesfieldoffsetattributeclasstopic).  
  
 The following sample generates CS0636:  
  
```  
// CS0636.cs  
using System;  
using System.Runtime.InteropServices;  
  
// To resolve the error, uncomment the following line:  
// [StructLayout(LayoutKind.Explicit)]  
struct Worksheet  
{  
   [FieldOffset(4)]public int i;   // CS0636   
}  
  
public class MainClass  
{  
   public static void Main ()  
   {  
   }  
}  
```