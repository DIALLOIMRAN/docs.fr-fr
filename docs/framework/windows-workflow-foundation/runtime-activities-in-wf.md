---
title: "Activit&#233;s d&#39;ex&#233;cution dans WF | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e5ae8c31-19bc-4655-88b3-6b75cd6a1bd5
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# Activit&#233;s d&#39;ex&#233;cution dans WF
[!INCLUDE[netfx_current_short](../../../includes/netfx-current-short-md.md)] offre plusieurs activités fournies par le système pour l'accès aux fonctionnalités de l'exécution du workflow, telles que la persistance et l'arrêt de workflow.  
  
|Activité|Description|  
|--------------|-----------------|  
|<xref:System.Activities.Statements.Persist>|Demande de façon explicite à ce que le workflow rende son état persistant.|  
|<xref:System.Activities.Statements.TerminateWorkflow>|Arrête l'instance de workflow en cours d'exécution.|  
|<xref:System.Activities.Statements.NoPersistScope>|Activité de conteneur qui empêche la persistance des activités enfants.|