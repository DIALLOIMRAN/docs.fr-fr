---
title: "Comment&#160;: cr&#233;er un v&#233;rificateur d&#39;identit&#233; du client personnalis&#233; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: f2d34e43-fa8b-46d2-91cf-d2960e13e16b
caps.latest.revision: 15
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 15
---
# Comment&#160;: cr&#233;er un v&#233;rificateur d&#39;identit&#233; du client personnalis&#233;
La fonctionnalité d'*identité* de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] active un client pour spécifier à l'avance l'identité attendue du service.  Lorsqu'un serveur s'authentifie auprès du client, l'identité est vérifiée par rapport à l'identité attendue.  \(Pour obtenir une explication de l'identité et de son fonctionnement, consultez [Identité du service et authentification](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md).\)  
  
 Si nécessaire, la vérification peut être personnalisée à l'aide d'un vérificateur d'identité personnalisé.  Par exemple, vous pouvez effectuer des contrôles supplémentaires de vérification de l'identité du service.  Dans cet exemple, le vérificateur d'identité personnalisé vérifie des revendications supplémentaires dans le certificat X.509 retourné par le serveur.  Pour obtenir un exemple d'application, consultez [Service Identity, exemple](../../../../docs/framework/wcf/samples/service-identity-sample.md).  
  
### Pour étendre la classe EndpointIdentity  
  
1.  Définissez une nouvelle classe dérivée de la classe <xref:System.ServiceModel.EndpointIdentity>.  Cet exemple nomme l'extension `OrgEndpointIdentity`.  
  
2.  Ajoutez les membres privés avec les propriétés qui seront utilisées par la classe <xref:System.ServiceModel.Security.IdentityVerifier> étendue pour exécuter le contrôle d'identité par rapport aux revendications dans le jeton de sécurité retourné par le service.  Cet exemple définit une propriété : la propriété `OrganizationClaim`.  
  
     [!code-csharp[c_HowToSetCustomClientIdentity#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#6)]
     [!code-vb[c_HowToSetCustomClientIdentity#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#6)]  
  
### Pour étendre la classe IdentityVerifier  
  
1.  Définissez une nouvelle classe dérivée de <xref:System.ServiceModel.Security.IdentityVerifier>.  Cet exemple nomme l'extension `CustomIdentityVerifier`.  
  
     [!code-csharp[c_HowToSetCustomClientIdentity#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#7)]
     [!code-vb[c_HowToSetCustomClientIdentity#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#7)]  
  
2.  Remplacez la méthode <xref:System.ServiceModel.Security.IdentityVerifier.CheckAccess%2A>.  La méthode détermine si le contrôle d'identité a réussi ou a échoué.  
  
3.  La méthode `CheckAccess` possède deux paramètres.  Le premier est une instance de la classe <xref:System.ServiceModel.EndpointIdentity>.  Le second est une instance de la classe <xref:System.IdentityModel.Policy.AuthorizationContext>.  
  
     Dans l'implémentation de méthode, examinez la collection de revendications retournée par la propriété <xref:System.IdentityModel.Policy.AuthorizationContext.ClaimSets%2A> de la classe <xref:System.IdentityModel.Policy.AuthorizationContext> et exécutez des contrôles d'authentification selon les besoins.  Cet exemple commence par rechercher les revendications de type « Distinguished Name » puis compare le nom à l'extension du <xref:System.ServiceModel.EndpointIdentity> \(`OrgEndpointIdentity`\).  
  
     [!code-csharp[c_HowToSetCustomClientIdentity#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#1)]
     [!code-vb[c_HowToSetCustomClientIdentity#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#1)]  
  
### Pour implémenter la méthode TryGetIdentity  
  
1.  Implémentez la méthode <xref:System.ServiceModel.Security.IdentityVerifier.TryGetIdentity%2A> qui détermine si une instance de la classe <xref:System.ServiceModel.EndpointIdentity> peut être retournée par le client.  L'infrastructure [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] appelle d'abord l'implémentation de la méthode `TryGetIdentity` pour récupérer l'identité du service à partir du message.  Ensuite, l'infrastructure appelle l'implémentation `CheckAccess` avec les `EndpointIdentity` et <xref:System.IdentityModel.Policy.AuthorizationContext> retournés.  
  
2.  Dans la méthode `TryGetIdentity`, insérez le code suivant :  
  
     [!code-csharp[c_HowToSetCustomClientIdentity#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#2)]
     [!code-vb[c_HowToSetCustomClientIdentity#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#2)]  
  
### Pour implémenter une liaison personnalisée et définir l'IdentityVerifier personnalisé  
  
1.  Créez une méthode qui retourne un objet <xref:System.ServiceModel.Channels.Binding>.  Cet exemple commence par créer une instance de la classe <xref:System.ServiceModel.WSHttpBinding> et affecte à son mode de sécurité la valeur <xref:System.ServiceModel.SecurityMode>, et à son <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A> la valeur <xref:System.ServiceModel.MessageCredentialType>.  
  
2.  Créez un <xref:System.ServiceModel.Channels.BindingElementCollection> à l'aide de la méthode <xref:System.ServiceModel.WSHttpBinding.CreateBindingElements%2A>.  
  
3.  Retournez le <xref:System.ServiceModel.Channels.SecurityBindingElement> de la collection et effectuez un cast en une variable <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>.  
  
4.  Affectez à la propriété <xref:System.ServiceModel.Channels.LocalClientSecuritySettings.IdentityVerifier%2A> de la classe <xref:System.ServiceModel.Channels.LocalClientSecuritySettings> une nouvelle instance de la classe `CustomIdentityVerifier` créée précédemment.  
  
     [!code-csharp[c_HowToSetCustomClientIdentity#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#3)]
     [!code-vb[c_HowToSetCustomClientIdentity#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#3)]  
  
5.  La liaison personnalisée retournée est utilisée pour créer une instance du client et de la classe.  Le client peut effectuer ensuite une vérification personnalisée de l'identité du service comme indiqué dans le code suivant.  
  
     [!code-csharp[c_HowToSetCustomClientIdentity#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#4)]
     [!code-vb[c_HowToSetCustomClientIdentity#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#4)]  
  
## Exemple  
 L'exemple suivant illustre une implémentation complète de la classe <xref:System.ServiceModel.Security.IdentityVerifier>.  
  
 [!code-csharp[c_HowToSetCustomClientIdentity#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#5)]
 [!code-vb[c_HowToSetCustomClientIdentity#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#5)]  
  
## Exemple  
 L'exemple suivant illustre une implémentation complète de la classe <xref:System.ServiceModel.EndpointIdentity>.  
  
 [!code-csharp[c_HowToSetCustomClientIdentity#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_howtosetcustomclientidentity/cs/source.cs#6)]
 [!code-vb[c_HowToSetCustomClientIdentity#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howtosetcustomclientidentity/vb/source.vb#6)]  
  
## Voir aussi  
 <xref:System.ServiceModel.ServiceAuthorizationManager>   
 <xref:System.ServiceModel.EndpointIdentity>   
 <xref:System.ServiceModel.Security.IdentityVerifier>   
 [Service Identity, exemple](../../../../docs/framework/wcf/samples/service-identity-sample.md)   
 [Authorization Policy](../../../../docs/framework/wcf/samples/authorization-policy.md)   
 [Authorization Policy](../../../../docs/framework/wcf/samples/authorization-policy.md)