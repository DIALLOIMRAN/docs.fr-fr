---
title: "Utilisation d&#39;actions pour impl&#233;menter le comportement c&#244;t&#233; serveur | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-oob"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 11a372db-7168-498b-80d2-9419ff557ba5
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# Utilisation d&#39;actions pour impl&#233;menter le comportement c&#244;t&#233; serveur
Les actions OData permettent d'implémenter un comportement qui agit sur une ressource extraite d'un service OData.  Par exemple, prenez un film numérique comme ressource. Vous pouvez effectuer plusieurs opérations avec celui\-ci : extraction, évaluation\/commentaire ou archivage.  Ce sont des exemples d'actions pouvant être implémentées par un service de données WCF qui gère les films numériques.  Les actions sont écrites dans une réponse OData qui contient une ressource sur laquelle l'action peut être appelée.  Lorsqu'un utilisateur demande une ressource qui représente un film numérique, la réponse retournée par le service de données WCF contient des informations sur les actions disponibles pour cette ressource.  La disponibilité d'une action dépend de l'état du service de données ou de la ressource.  Par exemple, une fois qu'un film numérique est extrait, il ne peut pas être extrait par un autre utilisateur.  Les clients peuvent appeler une action en indiquant simplement une URL.  Par exemple, http:\/\/MyServer\/MovieService.svc\/Movies\(6\) identifie un film numérique spécifique et http:\/\/MyServer\/MovieService.svc\/Movies\(6\)\/Checkout appelle l'action sur le film spécifié.  Les actions vous permettent d'exposer votre modèle de service sans exposer votre modèle de données.  Poursuivons avec l'exemple de service de film, vous pouvez autoriser un utilisateur à évaluer un film, mais pas à exposer directement les données d'évaluation en tant que ressource.  Vous pouvez implémenter une action d'évaluation pour autoriser l'utilisateur à évaluer un film, sans accéder directement aux données d'évaluation en tant que ressource.  
  
 Pour un exemple complet d'implémentation d'une action de service de données WCF, consultez [Fournisseur d'actions pour les services de données WCF pour Entity Framework](http://efactionprovider.codeplex.com/)  
  
## Implémentation d'une action  
 Pour implémenter une action de service, vous devez implémenter les interfaces <xref:System.IServiceProvider>, <xref:System.Data.Services.Providers.IDataServiceActionProvider> et <xref:System.Data.Services.Providers.IDataServiceInvokable>.  <xref:System.IServiceProvider> permet aux services de données WCF d'obtenir votre implémentation de <xref:System.Data.Services.Providers.IDataServiceActionProvider>.  <xref:System.Data.Services.Providers.IDataServiceActionProvider> permet aux services de données WCF de créer, rechercher, décrire et appeler des actions de service.  <xref:System.Data.Services.Providers.IDataServiceInvokable> vous permet d'appeler le code qui implémente le comportement des actions de service et d'obtenir le résultat, le cas échéant.  N'oubliez pas que les services de données WCF sont des services WCF par appel ; une nouvelle instance du service est créée à chaque fois que le service est appelé.  Assurez\-vous qu'aucun travail inutile n'est effectué lors de la création du service.  
  
### IServiceProvider  
 <xref:System.IServiceProvider> contient une méthode appelée <xref:System.IServiceProvider.GetService%2A>.  Cette méthode est appelée par les services de données WCF pour récupérer plusieurs fournisseurs de services, notamment des fournisseurs de services de métadonnées et des fournisseurs d'actions de service de données.  Lorsqu'un fournisseur d'actions de service de données est demandé, retourne votre implémentation <xref:System.Data.Services.Providers.IDataServiceActionProvider>.  
  
### IDataServiceActionProvider  
 <xref:System.Data.Services.Providers.IDataServiceActionProvider> contient des méthodes qui vous permettent de récupérer des informations concernant les actions disponibles.  Lorsque vous implémentez <xref:System.Data.Services.Providers.IDataServiceActionProvider>, vous augmentez les métadonnées de votre service qui est défini par votre implémentation de <xref:System.Data.Services.Providers.IDataServiceMetadataProvider> du service avec des actions et vous gérez la répartition de ces actions comme il convient.  
  
#### AdvertiseServiceAction  
 <xref:System.Data.Services.Providers.IDataServiceActionProvider.AdvertiseServiceAction%2A> est appelée pour déterminer quelles sont les actions disponibles pour la ressource spécifiée.  Cette méthode est appelée uniquement pour les actions qui ne sont pas toujours disponibles.  Elle s'utilise pour vérifier si l'action doit être incluse dans la réponse OData en fonction de l'état de la ressource demandée ou de l'état du service.  Le mode de vérification dépend entièrement de vous.  Si l'opération permettant de calculer la disponibilité et la ressource active dans un flux est coûteuse en ressources, la vérification peut être ignorée et l'action publiée.  Le paramètre `inFeed` a la valeur `true` si la ressource active retournée fait partie d'un flux.  
  
#### CreateInvokable  
 [M:System.Data.Services.Providers.IDataServiceActionProvider.CreateInvokable\(System.Data.Services.DataServiceOperationContext,System.Data.Services.Providers.ServiceAction,System.Object\<xref:System.Data.Services.Providers.IDataServiceActionProvider.CreateInvokable%2A> est appelée pour créer un <xref:System.Data.Services.Providers.IDataServiceInvokable> contenant un délégué qui encapsule le code qui implémente le comportement de l'action.  Cela crée l'instance <xref:System.Data.Services.Providers.IDataServiceInvokable>, mais n'appelle pas l'action.  Les actions de service de données WCF ont des effets secondaires et doivent fonctionner conjointement avec le fournisseur de mise à jour pour enregistrer ces modifications sur le disque.  La méthode <xref:System.Data.Services.Providers.IDataServiceInvokable.Invoke%2A> est appelée par la méthode SaveChanges\(\) du fournisseur de mise à jour.  
  
#### GetServiceActions  
 Cette méthode retourne une collection d'instances <xref:System.Data.Services.Providers.ServiceAction> qui représentent toutes les actions exposées par un service de données WCF.  <xref:System.Data.Services.Providers.ServiceAction> est la représentation des métadonnées d'une action, qui inclut des informations, telles que le nom d'une action, ses paramètres et son type de retour.  
  
#### GetServiceActionsByBindingParameterType  
 Cette méthode retourne la collection de toutes les instances <xref:System.Data.Services.Providers.ServiceAction> pouvant être liées au type de paramètre de liaison spécifié.  En d'autres termes, les <xref:System.Data.Services.Providers.ServiceAction> pouvant agir sur le type de ressource spécifié \(également appelé type de paramètre de liaison\). Elle s'utilise lorsque le service retourne une ressource afin d'inclure des informations sur les actions pouvant être appelées par rapport à cette ressource.  Cette méthode ne doit retourner que les actions qui lient au type exact de paramètre de liaison \(aucun type dérivé\).  Cette méthode est appelée une fois par demande par type rencontré et le résultat est mis en cache par les services de données WCF.  
  
#### TryResolveServiceAction  
 Cette méthode recherche un <xref:System.Data.Services.Providers.ServiceAction> spécifique et retourne `true` si <xref:System.Data.Services.Providers.ServiceAction> est trouvé.  Une fois trouvé, <xref:System.Data.Services.Providers.ServiceAction> est retourné dans le paramètre `serviceAction` `out`.  
  
### IDataServiceInvokable  
 Cette interface offre un moyen d'exécuter une action de service de données WCF.  Lorsque vous implémentez IDataServiceInvokable, vous êtes chargé de trois opérations :  
  
1.  capture et marshaling potentiel des paramètres ;  
  
2.  distribution des paramètres dans le code qui implémente réellement l'action lorsque la méthode Invoke\(\) est appelée ;  
  
3.  stockage des résultats de la méthode Invoke\(\) de façon à ce qu'ils puissent être récupérés en utilisant la méthode GetResult\(\).  
  
 Les paramètres peuvent être passés en tant que jetons.  En effet, il est possible d'écrire un fournisseur de services de données qui fonctionne avec des jetons qui représentent des ressources ; le cas échéant, vous devrez peut\-être convertir \(marshaler\) ces jetons en ressources réelles avant de les distribuer à l'action réelle.  Une fois le paramètre marshalé, il doit être dans un état modifiable de façon à ce que les modifications apportées à la ressource lorsque l'action est appelée soient enregistrées et écrites sur le disque.  
  
 Cette interface nécessite deux méthodes : Invoke et GetResult.  Invoke appelle le délégué qui implémente le comportement de l'action et GetResult retourne le résultat de l'action.  
  
## Appel d'une action de service de données WCF  
 Les actions sont appelées en utilisant une requête POST HTTP.  L'URL spécifie la ressource suivie par le nom de l'action.  Les paramètres sont passés dans le corps de la requête.  Par exemple, s'il existe un service appelé MovieService qui expose une action appelée Rate.  Vous pouvez utiliser l'URL suivante pour appeler l'action Rate sur un film spécifique :  
  
 http:\/\/MovieServer\/MovieService.svc\/Movies\(1\)\/Rate  
  
 Movies\(1\) indique le film que vous voulez évaluer et Rate indique l'action d'évaluation.  La valeur réelle de l'évaluation sera dans la corps de la requête HTTP tel que l'illustre cet exemple :  
  
```  
POST http://MovieServer/MoviesService.svc/Movies(1)/Rate HTTP/1.1   
Content-Type: application/json   
Content-Length: 20   
Host: localhost:15238  
{   
   "rating": 4   
}  
  
```  
  
> [!WARNING]
>  L'exemple de code ci\-dessus ne fonctionnera qu'avec les services de données WCF version 5.2 et version ultérieure qui prennent en charge le format JSON léger.  Si vous utilisez une version antérieure des services de données WCF, vous devez spécifier le type de contenu détaillé JSON comme suit : `application/json;odata=verbose`.  
  
 Vous pouvez aussi appeler une action en utilisant le client des services de données WCF comme indiqué dans l'extrait de code suivant :  
  
```  
MoviesModel context = new MoviesModel (new Uri("http://MyServer/MoviesService.svc/"));  
            //...  
            context.Execute(new Uri("http://MyServer/MoviesService.svc/Movies(1)/Rate"), "POST", new BodyOperationParameter("rating",4) );           
```  
  
 Dans l'extrait de code ci\-dessus, la classe `MoviesModel` a été générée en utilisant Visual Studio pour ajouter une référence de service à un service de données WCF.  
  
## Voir aussi  
 [WCF Data Services 4.5](../../../../docs/framework/data/wcf/index.md)   
 [Définition de WCF Data Services](../../../../docs/framework/data/wcf/defining-wcf-data-services.md)   
 [Développer et déployer WCF Data Services](../../../../docs/framework/data/wcf/developing-and-deploying-wcf-data-services.md)   
 [Fournisseurs de services de données personnalisés](../../../../docs/framework/data/wcf/custom-data-service-providers-wcf-data-services.md)