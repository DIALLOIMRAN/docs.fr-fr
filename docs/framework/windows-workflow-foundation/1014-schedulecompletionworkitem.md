---
title: "1014 - ScheduleCompletionWorkItem | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 84203735-478d-42d8-a320-c175dbddcb38
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# 1014 - ScheduleCompletionWorkItem
## Propriétés  
  
|||  
|-|-|  
|ID|1014|  
|Mots clés|WFRuntime|  
|Niveau|Verbose|  
|Canal|Microsoft\-Windows\-Application Server\-Applications\/Débogage|  
  
## Description  
 Indique qu'un CompletionWorkItem a été planifié.  
  
## Message  
 CompletionWorkItem a été planifié pour l'activité parent « %1 », DisplayName : « %2 », InstanceId : « %3 ». Activité « %4 » terminée, DisplayName : « %5 », InstanceId : « %6 ».  
  
## Détails  
  
|Nom d'élément de données|Type d'élément de données|Description|  
|------------------------------|-------------------------------|-----------------|  
|ParentActivity|xs:string|Nom de type de l'activité parente.|  
|ParentDisplayName|xs:string|Nom complet de l'activité parente.|  
|ParentInstanceId|xs:string|ID d'instance de l'activité parente.|  
|CompletedActivity|xs:string|Nom de type de l'activité achevée.|  
|CompletedActivityDisplayName|xs:string|Nom complet de l'activité achevée.|  
|CompletedActivityInstanceId|xs:string|ID d'instance de l'activité achevée.|  
|AppDomain|xs:string|Chaîne retournée par AppDomain.CurrentDomain.FriendlyName.|