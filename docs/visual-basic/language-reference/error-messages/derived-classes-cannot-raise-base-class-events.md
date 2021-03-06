---
title: "Derived classes cannot raise base class events | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
f1_keywords: 
  - "vbc30029"
  - "bc30029"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "BC30029"
ms.assetid: 63afa1c6-2f93-4512-a2f0-372455979771
caps.latest.revision: 9
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 9
---
# Derived classes cannot raise base class events
[!INCLUDE[vs2017banner](../../../visual-basic/includes/vs2017banner.md)]

Un événement peut être déclenché uniquement à partir de l'espace de déclaration dans lequel il est déclaré.  En conséquence, une classe ne peut pas déclencher d'événements à partir d'une autre classe, même d'une classe dont elle est dérivée.  
  
 **ID d'erreur :** BC30029  
  
### Pour corriger cette erreur  
  
-   Déplacez l'instruction `Event` ou l'instruction `RaiseEvent` de manière à ce qu'elles soient dans la même classe.  
  
## Voir aussi  
 [Event Statement](../../../visual-basic/language-reference/statements/event-statement.md)   
 [RaiseEvent Statement](../../../visual-basic/language-reference/statements/raiseevent-statement.md)