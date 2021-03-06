---
title: "Utilisez &#39;FileGetObject&#39; &#224; la place de &#39;FileGet&#39; quand l’argument est de type &#39;Object&#39;. | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
ms.assetid: 090b8088-895a-482a-9362-606596bac304
caps.latest.revision: 9
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 9
---
# Utilisez &#39;FileGetObject&#39; &#224; la place de &#39;FileGet&#39; quand l’argument est de type &#39;Object&#39;.
La méthode `FileGet` comprend un argument de type `Object`.`FileGetObject` doit être utilisé à la place de `FileGet` pour éviter toute ambiguïté.  
  
 Notez que les fonctionnalités offertes par `My.Computer.Filesystem` proposent une plus grande facilité d’utilisation et des performances accrues par rapport à `FileGet` ou `FileGetObject`.  
  
### Pour corriger cette erreur  
  
1.  Remplacez `FileGet` par `FileGetObject`.  
  
2.  Convertissez l’argument `Object` en type plus spécifique.  
  
## Voir aussi  
 [NOT IN BUILD : Fonction FilePutObject](http://msdn.microsoft.com/fr-fr/3eda786b-d1ee-4b44-9dd7-0ea6bff072c0)   
 [My.Computer.FileSystem Object](../../visual-basic/language-reference/objects/my-computer-filesystem-object.md)