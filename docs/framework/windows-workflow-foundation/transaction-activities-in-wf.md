---
title: "Activit&#233;s de transaction dans WF | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: fb33378e-82c6-4ea0-870f-76dc77e7f0fe
caps.latest.revision: 10
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 10
---
# Activit&#233;s de transaction dans WF
[!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)] a plusieurs activités fournies par le système pour modéliser les transactions, la compensation et l'annulation.Ces modèles de programmation permettent au workflow de poursuivre la progression vers l'avant dans le cas de modifications de logique métier et de gestion des erreurs.[!INCLUDE[crabout](../../../includes/crabout-md.md)] les transactions, la compensation et l'annulation, consultez [Transactions](../../../docs/framework/windows-workflow-foundation//workflow-transactions.md), [Compensation](../../../docs/framework/windows-workflow-foundation//compensation.md) et [Annulation](../../../docs/framework/windows-workflow-foundation//modeling-cancellation-behavior-in-workflows.md).  
  
## Activités de transaction  
  
|||  
|-|-|  
|<xref:System.Activities.Statements.CancellationScope>|Associe la logique d'annulation, sous la forme d'une activité, à un chemin d'accès principal d'exécution, également exprimé sous la forme d'une activité.|  
|<xref:System.Activities.Statements.CompensableActivity>|Prend en charge la compensation de ses activités enfants.|  
|<xref:System.Activities.Statements.Compensate>|Appelle explicitement le gestionnaire de compensation d'un <xref:System.Activities.Statements.CompensableActivity>.|  
|<xref:System.Activities.Statements.Confirm>|Appelle explicitement le gestionnaire de confirmation d'un <xref:System.Activities.Statements.CompensableActivity>.|  
|<xref:System.Activities.Statements.TransactionScope>|Délimite une limite de transaction.|  
|<xref:System.ServiceModel.Activities.TransactedReceiveScope>|Définit la durée de vie d'une transaction créée par un message reçu.La transaction peut être transmise dans le workflow sur le message de départ ou peut être créée par un répartiteur lorsque le message est reçu. **Note:**  <xref:System.ServiceModel.Activities.TransactedReceiveScope> se trouve dans la section **Messagerie** de la **Boîte à outils**.|