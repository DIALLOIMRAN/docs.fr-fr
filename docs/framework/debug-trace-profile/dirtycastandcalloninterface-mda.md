---
title: "dirtyCastAndCallOnInterface MDA | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "managed debugging assistants (MDAs), early bound calls AutoDispatch"
  - "dispatch-only mode"
  - "dirtyCastAndCallOnInterface"
  - "early-bound managed classes"
  - "early bound calls on AutoDispatch"
  - "MDAs (managed debugging assistants), early bound calls AutoDispatch"
  - "EarlyBoundCallOnAutorDispatchClassInteface MDA"
ms.assetid: aa388ed3-7e3d-48ea-a0b5-c47ae19cec38
caps.latest.revision: 17
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 17
---
# dirtyCastAndCallOnInterface MDA
L'Assistant Débogage managé \(MDA\) `dirtyCastAndCallOnInterface` est activé quand une tentative d'appel à liaison anticipée par un vtable est effectuée sur une interface de classes qui a été marquée comme étant à liaison tardive uniquement.  
  
## Symptômes  
 Une application lève une violation d'accès ou a un comportement inattendu quand un appel à liaison anticipée est placé via COM dans le CLR.  
  
## Cause  
 Le code tente un appel à liaison anticipée par un vtable via une interface de classes qui est à liaison tardive uniquement.  Notez que, par défaut, les interfaces de classes sont identifiées comme étant à liaison tardive uniquement.  Elles peuvent également être identifiées comme étant à liaison tardive à l'aide de l'attribut <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> défini à la valeur <xref:System.Runtime.InteropServices.ClassInterfaceType> \(`[ClassInterface(ClassInterfaceType.AutoDispatch)]`\).  
  
## Résolution  
 La résolution recommandée consiste à définir une interface explicite destinée à être utilisée par COM et à faire appeler les clients COM via cette interface plutôt que via l'interface de classes générée automatiquement.  L'appel de COM peut aussi être changé en appel à liaison tardive via `IDispatch`.  
  
 Enfin, il est possible d'identifier la classe comme <xref:System.Runtime.InteropServices.ClassInterfaceType> \(`[ClassInterface(ClassInterfaceType.AutoDual)]`\) pour permettre aux appels à liaison anticipée d'être placés par COM. L'utilisation de <xref:System.Runtime.InteropServices.ClassInterfaceType> est toutefois fortement déconseillée en raison des limitations du contrôle de version décrites dans <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>.  
  
## Effet sur le runtime  
 Cet Assistant Débogage managé n'a aucun effet sur le CLR.  Il fournit uniquement des données relatives aux appels à liaison anticipée qui sont effectués sur les interfaces à liaison tardive.  
  
## Sortie  
 Nom de la méthode ou nom du champ qui fait l'objet d'un accès à liaison anticipée.  
  
## Configuration  
  
```  
<mdaConfig>  
  <assistants>  
    <dirtyCastAndCallOnInterface />  
  </assistants>  
</mdaConfig>  
```  
  
## Voir aussi  
 <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>   
 [Diagnosing Errors with Managed Debugging Assistants](../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)