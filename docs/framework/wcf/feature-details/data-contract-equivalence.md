---
title: "&#201;quivalence de contrats de donn&#233;es | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
helpviewer_keywords: 
  - "contrats de données (WCF), équivalence"
ms.assetid: f06f3c7e-c235-4ec1-b200-68142edf1ed1
caps.latest.revision: 16
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 16
---
# &#201;quivalence de contrats de donn&#233;es
Pour qu'un client puisse envoyer des données d'un certain type à un service ou pour qu'un service puisse envoyer des données à un client, le type des données envoyées n'a pas nécessairement besoin d'exister à l'extrémité de réception.  La seule exigence est que les contrats de données des deux types soient équivalents.  \(Parfois, une équivalence stricte n'est pas requise, comme expliqué dans [Contrôle de version des contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-versioning.md).\)  
  
 Pour être équivalents, des contrats de données doivent avoir le même espace de noms et le même nom.  En outre, chaque membre de données d'un côté doit avoir un membre de données équivalent de l'autre côté.  
  
 Pour être équivalents, les membres de données doivent avoir le même nom.  En outre, ils doivent représenter le même type de données ; autrement dit, leurs contrats de données doivent être équivalents.  
  
> [!NOTE]
>  Notez que le nom et l'espace de noms des contrats de données, de même que le nom des membres de données, respectent la casse.  
  
 [!INCLUDE[crabout](../../../../includes/crabout-md.md)] les noms et espaces de noms des contrats de données, ainsi que sur les noms des membres de données, consultez [Noms de contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-names.md).  
  
 Si deux types existent d'un même côté \(expéditeur ou récepteur\) et si leurs contrats de données ne sont pas équivalents \(par exemple, ils possèdent des membre de données différents\), vous ne devez pas leur donner le même nom ni le même espace de noms.  Vous risqueriez alors de lever des exceptions.  
  
 Les contrats de données répondant aux types suivants sont équivalents :  
  
 [!code-csharp[C_DataContractNames#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#5)]
 [!code-vb[C_DataContractNames#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#5)]  
  
## Ordre des membres de données et équivalence des contrats de données  
 La propriété <xref:System.Runtime.Serialization.DataMemberAttribute.Order%2A> de la classe <xref:System.Runtime.Serialization.DataMemberAttribute> peut affecter l'équivalence des contrats de données.  Les membres des contrats de données doivent apparaître dans le même ordre pour que les contrats de données soient équivalents.  L'ordre par défaut est l'ordre alphabétique.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Classement des membres de données](../../../../docs/framework/wcf/feature-details/data-member-order.md).  
  
 Par exemple, le code suivant donne des contrats de données équivalents :  
  
 [!code-csharp[C_DataContractNames#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#6)]
 [!code-vb[C_DataContractNames#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#6)]  
  
 Par contre, le code suivant ne donne pas des contrats de données équivalents :  
  
 [!code-csharp[C_DataContractNames#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#7)]
 [!code-vb[C_DataContractNames#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#7)]  
  
## Héritage, interfaces et équivalence des contrats de données  
 Lors de la détermination de l'équivalence, un contrat de données qui hérite d'un autre contrat de données est traité comme un seul contrat de données incluant tous les membres de données du type de base.  N'oubliez pas que l'ordre des membres de données doit correspondre et que les membres du type de base précèdent les membres du type dérivé, dans l'ordre choisi.  En outre, si, comme dans l'exemple de code suivant, deux membres de données ont la même valeur d'ordre, l'ordre de ces membres de données est alphabétique.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Classement des membres de données](../../../../docs/framework/wcf/feature-details/data-member-order.md).  
  
 Dans l'exemple suivant, le contrat de données de type `Employee` est équivalent au contrat de données de type `Worker`.  
  
 [!code-csharp[C_DataContractNames#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_datacontractnames/cs/source.cs#8)]
 [!code-vb[C_DataContractNames#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_datacontractnames/vb/source.vb#8)]  
  
 Lors du passage des paramètres et des valeurs de retour entre un client et un service, un contrat de données d'une classe de base ne peut pas être envoyé lorsque le point de terminaison de réception attend un contrat de données d'une classe dérivée,  conformément aux doctrines de la programmation orientée objet.  Dans l'exemple précédent, un objet de type `Person` ne peut pas être envoyé lorsqu'un `Employee`  est attendu.  
  
 Un contrat de données d'une classe dérivée peut être envoyé lorsqu'un contrat de données d'une classe de base est attendu, mais uniquement si le point de terminaison de réception prend connaissance du type dérivé à l'aide de <xref:System.Runtime.Serialization.KnownTypeAttribute>.  [!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Types connus de contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md).  Dans l'exemple précédent, un objet de type `Employee` peut être envoyé lorsqu'un `Person` est attendu, mais uniquement si le code de récepteur utilise <xref:System.Runtime.Serialization.KnownTypeAttribute> pour l'inclure dans la liste des types connus.  
  
 Lors du passage des paramètres et des valeurs de retour entre des applications, si le type attendu est une interface, il est équivalent au type attendu de type <xref:System.Object>.  Dans la mesure où chaque type dérive en dernier lieu de <xref:System.Object>, chaque contrat de données dérive en dernier lieu du contrat de données pour <xref:System.Object>.  De ce fait, tout type de contrat de données peut être passé lorsqu'une interface est attendue.  Des étapes supplémentaires sont requises pour utiliser convenablement les interfaces. Pour plus d'informations, consultez [Types connus de contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md).  
  
## Voir aussi  
 <xref:System.Runtime.Serialization.DataContractAttribute>   
 <xref:System.Runtime.Serialization.DataMemberAttribute>   
 [Classement des membres de données](../../../../docs/framework/wcf/feature-details/data-member-order.md)   
 [Types connus de contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md)   
 [Noms de contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-names.md)