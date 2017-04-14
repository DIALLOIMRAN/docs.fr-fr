---
title: "Comment&#160;: d&#233;clarer des erreurs dans des contrats de service | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: e8da98e7-d22f-4f60-ac82-3fb0928a353f
caps.latest.revision: 5
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 5
---
# Comment&#160;: d&#233;clarer des erreurs dans des contrats de service
Dans le code managé, des exceptions sont levées en cas de conditions d'erreur.  Cependant, dans les applications [!INCLUDE[indigo1](../../../includes/indigo1-md.md)], les contrats de service spécifient les informations d'erreur à retourner aux clients en déclarant des erreurs SOAP.  Pour obtenir une vue d'ensemble des relations entre les exceptions et les erreurs, consultez [Spécification et gestion des erreurs dans les contrats et les services](../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md).  
  
### Créer un contrat de service qui spécifie une erreur SOAP  
  
1.  Créez un contrat de service qui contient au moins une opération.  Pour obtenir un exemple, consultez [Comment : définir un contrat de service](../../../docs/framework/wcf/how-to-define-a-wcf-service-contract.md).  
  
2.  Sélectionnez une opération qui peut spécifier une condition d'erreur dont les clients peuvent s'attendre à être notifiés.  Pour déterminer les conditions d'erreur qui justifient de retourner des erreurs SOAP aux clients, consultez [Spécification et gestion des erreurs dans les contrats et les services](../../../docs/framework/wcf/specifying-and-handling-faults-in-contracts-and-services.md).  
  
3.  Appliquez <xref:System.ServiceModel.FaultContractAttribute?displayProperty=fullName> à l'opération sélectionnée et passez un type d'erreur sérialisable au constructeur.  Pour plus d'informations sur la création et l'utilisation de types sérialisables, consultez [Spécification du transfert de données dans des contrats de service](../../../docs/framework/wcf/feature-details/specifying-data-transfer-in-service-contracts.md).  L'exemple suivant montre comment spécifier que l'opération `SampleMethod` peut provoquer `GreetingFault`.  
  
     [!code-csharp[FaultContractAttribute#4](../../../samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#4)]
     [!code-vb[FaultContractAttribute#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#4)]  
  
4.  Répétez les étapes 2 et 3 pour toutes les opérations dans le contrat qui communiquent des conditions d'erreur aux clients.  
  
## Implémentation d'une opération pour retourner une erreur SOAP spécifiée  
 Une fois qu'une opération a spécifié qu'une erreur SOAP spécifique peut être retournée \(tel que dans la procédure précédente\) pour communiquer une condition d'erreur à une application appelante, l'étape suivante consiste à implémenter cette spécification.  
  
#### Générer l'erreur SOAP spécifiée dans l'opération  
  
1.  Lorsqu'une condition d'erreur spécifique à <xref:System.ServiceModel.FaultContractAttribute> se produit dans une opération, levez une nouvelle <xref:System.ServiceModel.FaultException%601?displayProperty=fullName> où l'erreur SOAP spécifiée est le paramètre de type.  L'exemple suivant montre comment générer `GreetingFault` dans le `SampleMethod` présenté dans la procédure précédente et dans la section de code suivante.  
  
     [!code-csharp[FaultContractAttribute#5](../../../samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#5)]
     [!code-vb[FaultContractAttribute#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#5)]  
  
## Exemple  
 L'exemple de code suivant montre une implémentation d'une opération unique qui spécifie `GreetingFault` pour l'opération `SampleMethod`.  
  
 [!code-csharp[FaultContractAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CFX/faultcontractattribute/cs/services.cs#1)]
 [!code-vb[FaultContractAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/faultcontractattribute/vb/services.vb#1)]  
  
## Voir aussi  
 <xref:System.ServiceModel.FaultContractAttribute?displayProperty=fullName>   
 <xref:System.ServiceModel.FaultException%601?displayProperty=fullName>