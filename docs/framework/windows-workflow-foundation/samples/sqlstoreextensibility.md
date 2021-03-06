---
title: "SQLStoreExtensibility | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 5da1b5a3-f144-41ba-b9c4-02818b28b15d
caps.latest.revision: 11
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 11
---
# SQLStoreExtensibility
Cet exemple illustre l'utilisation et la configuration de propriétés promues dans le magasin d'instances de workflow SQL.Le magasin d'instances de workflow SQL est une implémentation basée sur SQL d'un magasin d'instances.Il permet à une instance d'enregistrer et de charger son état vers et à partir d'une base de données SQL Server ou SQL Server Express.La fonctionnalité d'extensibilité de magasin permet à l'utilisateur de définir des propriétés qui sont stockées dans le magasin d'instances.Ces propriétés sont affichées dans une vue de propriétés promues qui permet à l'utilisateur de les rechercher.  
  
 Cet exemple est composé d'un workflow qui implémente un service de comptage.Une fois que la méthode de démarrage du service est appelée, le service compte de 0 à 29.Le compteur est incrémenté une fois toutes les 2 secondes.Après chaque compte, le workflow rend son état persistant.La valeur actuelle du compteur est stockée dans le magasin d'instances sous la forme d'une propriété promue.  
  
 Le workflow de comptage est auto\-hébergé par un hôte de service de workflow.La méthode `Main` du programme effectue les actions suivantes :  
  
-   Crée une instance de l'hôte du service du workflow qui héberge le workflow de comptage et définit les points de terminaison sous lesquels le workflow de comptage peut être atteint.  
  
-   Définit un comportement de magasin d'instances de workflow SQL, lequel est utilisé pour configurer le magasin d'instances de workflow SQL.Le magasin a pour instruction de traiter `CountStatus` comme une propriété promue.  
  
-   Crée un client qui appelle la méthode de démarrage du workflow de comptage.  
  
 Une fois le programme démarré, le compteur commence automatiquement le comptage.Notez que le chargement de l'instance et la configuration du magasin d'instances de workflow SQL peut prendre quelques secondes.  
  
 Pour promouvoir la valeur du compteur en tant que propriété personnalisée, les étapes suivantes doivent être effectuées :  
  
1.  La classe `CounterStatus` définit une extension d'instance de type <xref:System.Activities.Persistence.PersistenceParticipant>, qui est utilisée par les activités pour fournir les variables d'état.`Count` est défini comme une valeur en écriture seule.Lorsqu'une instance de workflow atteint un point de persistance, l'extension d'instance enregistre la propriété `Count` dans la collecte de données de persistance.  
  
2.  Lors de la création du magasin d'instances, une nouvelle propriété, `CountStatus`, est définie via la méthode `store.Promote()`.  
  
3.  L'activité `SaveCounter` du workflow affecte la valeur actuelle du compteur au champ d'état `Count`.  
  
### Pour utiliser cet exemple  
  
1.  Créez la base de données du magasin d'instances.  
  
    1.  Ouvrez une invite de commandes [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
    2.  Accédez au répertoire de l'exemple \(\\WF\\Basic\\Persistence\\SqlStoreExtensibility\\CS\) et exécutez CreateInstanceStore.cmd dans l'invite de commandes [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)].  
  
        > [!WARNING]
        >  Le script CreateInstanceStore.cmd tente de créer la base de données sur l'instance par défaut de SQL Server 2008 Express.Si vous voulez installer la base de données sur une instance différente, modifiez le script en conséquence.  
  
2.  Ouvrez [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)] et chargez la solution SqlStoreExtensibility.sln, puis générez\-la en appuyant sur Ctrl\+Maj\+B.  
  
    > [!WARNING]
    >  Si vous avez installé la base de données sur une instance autre que l'instance par défaut de SQL Server, mettez à jour la chaîne de connexion dans le code avant de générer la solution.  
  
3.  Exécutez l'exemple avec des privilèges d'administrateur. Pour ce faire, accédez au répertoire bin du projet \(\\WF\\Basic\\Persistence\\SqlStoreExtensibility\\bin\\Debug\) dans l'[!INCLUDE[fileExplorer](../../../../includes/fileexplorer-md.md)], cliquez avec le bouton droit sur SqlStoreExtensibility.exe, puis sélectionnez **Exécuter en tant qu'administrateur**.  
  
### Pour vérifier que l'exemple fonctionne correctement  
  
1.  Utilisez SQL Server Management Studio pour afficher le contenu de la table d'instances en sélectionnant **Bases de données**, **InstanceStore**, puis **System.ServiceModel.Activities.DurableInstancing.InstanceTable** dans l'Explorateur d'objets, cliquez avec le bouton droit sur **System.ServiceModel.Activities.DurableInstancing.InstanceTable**, puis sélectionnez **Sélectionner les 1000 lignes du haut**.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] SQL Server Management Studio, consultez [Présentation de SQL Server Management Studio](http://go.microsoft.com/fwlink/?LinkId=165645).  
  
2.  Observez les instances de workflow répertoriées.  
  
3.  Dans une invite de commandes [!INCLUDE[vs2010](../../../../includes/vs2010-md.md)], exécutez le script QueryInstanceStore.cmd situé dans le répertoire de l'exemple \(\\WF\\Basic\\Persistence\\SqlStoreExtensibility\).  
  
4.  Observez la valeur du compteur affichée sous **CountStatus**.  
  
5.  Exécutez le script plusieurs fois pour voir la valeur **CountStats** changer.  
  
6.  Appuyez sur ENTRÉE pour mettre fin à l'application de workflow.  
  
### Pour désinstaller l'exemple  
  
1.  Supprimez la base de données du magasin d'instances en exécutant le script RemoveInstanceStore.cmd situé dans le répertoire de l'exemple \(\\WF\\Basic\\Persistence\\SqlStoreExtensibility\).  
  
> [!IMPORTANT]
>  Les exemples peuvent déjà être installés sur votre ordinateur.Recherchez le répertoire \(par défaut\) suivant avant de continuer.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples`  
>   
>  Si ce répertoire n'existe pas, rendez\-vous sur la page \(éventuellement en anglais\) des [exemples Windows Communication Foundation \(WCF\) et Windows Workflow Foundation \(WF\) pour .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) pour télécharger tous les exemples [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] et [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Cet exemple se trouve dans le répertoire suivant.  
>   
>  `<LecteurInstall>:\WF_WCF_Samples\WF\Basic\Persistence\SQLStoreExtensibility`  
  
## Voir aussi  
 [Persistance du workflow](../../../../docs/framework/windows-workflow-foundation//workflow-persistence.md)   
 [Services de workflow](../../../../docs/framework/wcf/feature-details/workflow-services.md)   
 [Hébergement AppFabric et exemples de persistance](http://go.microsoft.com/fwlink/?LinkId=193961)