---
title: "CoordinatorRecoveryLogEntryCorrupt | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 3cd0c3e3-84c8-4d43-a561-a8851c78e565
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# CoordinatorRecoveryLogEntryCorrupt
ID : 139  
  
 Gravité : Erreur  
  
 Catégorie : TransactionBridge  
  
## Description  
 Cet événement indique qu'une entrée de journal de récupération de coordinateur a été endommagée et n'a pas pu être désérialisée.Cette erreur risque d'entraîner une perte de données.L'événement répertorie l'ID de transaction, les données de récupération \(encodage Base64\), l'exception, le nom de processus et l'ID de processus.  
  
## Voir aussi  
 [Journalisation des événements](../../../../../docs/framework/wcf/diagnostics/event-logging/index.md)   
 [Référence générale relative aux événements](../../../../../docs/framework/wcf/diagnostics/event-logging/events-general-reference.md)