---
title: "CA2141:Transparent methods must not satisfy LinkDemands"
ms.custom: na
ms.date: 10/03/2016
ms.prod: visual-studio-dev14
ms.reviewer: na
ms.suite: na
ms.technology: 
  - vs-devops-test
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2957f5f7-c511-4180-ba80-752034f10a77
caps.latest.revision: 14
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
# CA2141:Transparent methods must not satisfy LinkDemands
|||  
|-|-|  
|TypeName|TransparentMethodsMustNotSatisfyLinkDemands|  
|CheckId|CA2141|  
|Category|Microsoft.Security|  
|Breaking Change|Breaking|  
  
## Cause  
 A security transparent method calls a method in an assembly that is not marked with the <xref:System.Security.AllowPartiallyTrustedCallersAttribute?qualifyHint=False> (APTCA) attribute, or a security transparent method satisfies a <xref:System.Security.Permissions.SecurityAction?qualifyHint=False>`.LinkDemand` for a type or a method.  
  
## Rule Description  
 Satisfying a LinkDemand is a security sensitive operation which can cause unintentional elevation of privilege. Security transparent code must not satisfy LinkDemands, because it is not subject to the same security audit requirements as security critical code. Transparent methods in security rule set level 1 assemblies will cause all LinkDemands they satisfy to be converted to full demands at run time, which can cause performance problems. In security rule set level 2 assemblies, transparent methods will fail to compile in the just-in-time (JIT) compiler if they attempt to satisfy a LinkDemand.  
  
 In assemblies that usee Level 2 security, attempts by a security transparent method to satisfy a LinkDemand or call a method in a non-APTCA assembly raises a <xref:System.MethodAccessException?qualifyHint=False>; in Level 1 assemblies the LinkDemand becomes a full Demand.  
  
## How to Fix Violations  
 To fix a violation of this rule, mark the accessing method with the <xref:System.Security.SecurityCriticalAttribute?qualifyHint=False> or <xref:System.Security.SecuritySafeCriticalAttribute?qualifyHint=False> attribute, or remove the LinkDemand from the accessed method.  
  
## When to Suppress Warnings  
 Do not suppress a warning from this rule.  
  
## Example  
 In this example, a transparent method attempts to call a method that has a LinkDemand. This rule will fire on this code.  
  
 [!CODE [FxCop.Security.CA2141.TransparentMethodsMustNotSatisfyLinkDemands#1](../CodeSnippet/VS_Snippets_CodeAnalysis/fxcop.security.ca2141.transparentmethodsmustnotsatisfylinkdemands#1)]