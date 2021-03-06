---
title: "Data Binding in a Windows Forms Client | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a2a30b37-d6e2-4552-820e-e60b2bbe8829
caps.latest.revision: 9
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 9
---
# Data Binding in a Windows Forms Client
Cet exemple illustre comment créer une liaison avec les données retournées par un service [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] dans une application Windows Forms.  
  
> [!NOTE]
>  La procédure d'installation ainsi que les instructions de génération relatives à cet exemple figurent en fin de rubrique.  
  
 Cet exemple contient un service qui implémente un contrat définissant un modèle de communication demande\-réponse.L'exemple comprend une application Windows Forms de client \(.exe\) et un service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] hébergé par les services IIS.  
  
 Le contrat est défini par l'interface `IWeatherService`, laquelle expose une opération nommée `GetWeatherData`.Cette opération accepte un tableau de villes et retourne un tableau d'objets `WeatherData` qui représente les prévisions de températures maximales et minimales d'une ville.  
  
 La liaison de données est générée sur le client, dans l'application Windows Forms.L'affichage `DataGridView`, qui correspond à une représentation graphique des données, est défini dans le concepteur Windows Forms.Un intermédiaire nommé `BindingSource` est également créé.La source de données de `BindingSource` a pour valeur le tableau de données retourné par le service.L'objectif de `BindingSource` est de fournir une couche d'indirection entre les données et leur affichage.Tous les processus concernant les données, telles que la navigation, le tri, le filtrage et la mise à jour, sont effectués en appelant le composant `BindingSource`.Pour lier les données à `DataGridView`, l'objet `BindingSource` est affecté à la source `datasource` de l'affichage `DataGridView`.Toutes les données retournées depuis le service [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] s'affichent alors sous la forme d'une représentation graphique visible par l'utilisateur.Chaque fois que l'utilisateur clique sur le bouton, les données retournées sont automatiquement mises à jour dans l'affichage `DataGridView` lié aux données.  
  
### Pour configurer, générer et exécuter l'exemple  
  
1.  Assurez\-vous d'avoir effectué la procédure figurant à la section [Procédure d'installation unique pour les exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Pour générer l'édition C\# ou Visual Basic .NET de la solution, suivez les instructions indiquées dans [Génération des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Pour exécuter l'exemple dans une configuration à un ou plusieurs ordinateurs, conformez\-vous aux instructions figurant dans la rubrique [Exécution des exemples Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WCF\Scenario\DataBinding\WindowsForms`  
  
## Voir aussi