---
title: "1025 - BookmarkScopeInitialized | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 63584434-e709-471d-9e96-97d3d99e70d6
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# 1025 - BookmarkScopeInitialized
## Propriétés  
  
|||  
|-|-|  
|ID|1025|  
|Mots clés|WFRuntime|  
|Niveau|Verbose|  
|Canal|Microsoft\-Windows\-Application Server\-Applications\/Débogage|  
  
## Description  
 Indique qu'un BookmarkScope a été initialisé.  
  
## Message  
 Le BookmarkScope avec TemporaryId : « %1 » a été initialisé avec l'ID : « %2 ».  
  
## Détails  
  
|Nom d'élément de données|Type d'élément de données|Description|  
|------------------------------|-------------------------------|-----------------|  
|TemporaryId|xs:string|ID temporaire du signet.|  
|InitializedId|xs:string|Identificateur initialisé du signet.|  
|AppDomain|xs:string|Chaîne retournée par AppDomain.CurrentDomain.FriendlyName.|