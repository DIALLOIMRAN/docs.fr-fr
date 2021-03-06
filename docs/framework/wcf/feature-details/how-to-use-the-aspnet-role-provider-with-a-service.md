---
title: "Comment&#160;: utiliser le fournisseur de r&#244;le ASP.NET avec un service | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 88d33a81-8ac7-48de-978c-5c5b1257951e
caps.latest.revision: 8
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 8
---
# Comment&#160;: utiliser le fournisseur de r&#244;le ASP.NET avec un service
Le fournisseur de rôle [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] \(conjointement au fournisseur d'appartenances [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] \) est une fonctionnalité qui permet aux développeurs [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)] de créer des sites Web permettant aux utilisateurs de créer un compte avec un site et d'être affectés d'un rôle à des fins d'autorisation.Grâce à cette fonctionnalité, les utilisateurs peuvent établir un compte avec le site et disposer d'un accès exclusif à celui\-ci et à ses services.Cette approche diffère de la sécurité Windows, qui requiert que les utilisateurs disposent de comptes dans un domaine Windows.Au lieu de cela, tout utilisateur qui fournit ses informations d'identification \(combinaison nom d'utilisateur\/mot de passe\) peut utiliser le site et ses services.  
  
 Pour obtenir un exemple d'application, consultez [Membership and Role Provider](../../../../docs/framework/wcf/samples/membership-and-role-provider.md).[!INCLUDE[crabout](../../../../includes/crabout-md.md)] la fonctionnalité de fournisseur d'appartenances [!INCLUDE[vstecasp](../../../../includes/vstecasp-md.md)], consultez [Comment : utiliser le fournisseur d'appartenances ASP.NET](../../../../docs/framework/wcf/feature-details/how-to-use-the-aspnet-membership-provider.md).  
  
 La fonctionnalité de fournisseur de rôle utilise une base de données SQL Server pour stocker des informations utilisateur.Les développeurs [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] peuvent tirer parti de ces fonctionnalités à des fins de sécurité.En cas d'intégration dans une application [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], les utilisateurs doivent fournir une combinaison nom d'utilisateur\/mot de passe à l'application cliente [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].Pour permettre à [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] d'utiliser la base de données, vous devez créer une instance de la classe <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>, affectez à sa propriété <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior.PrincipalPermissionMode%2A> la valeur <xref:System.ServiceModel.Description.PrincipalPermissionMode> et ajoutez l'instance à la collection de comportements au <xref:System.ServiceModel.ServiceHost> qui héberge le service.  
  
### Pour configurer le fournisseur de rôle  
  
1.  Dans le fichier Web.config, sous l'élément \<`system.web`\>, ajoutez un élément \<`roleManager`\> et affectez à son attribut `enabled` la valeur `true`.  
  
2.  Affectez à l'attribut `defaultProvider` la valeur `SqlRoleProvider`.  
  
3.  Ajoutez un élément \<`providers`\> en tant qu'enfant de l'élément \<`roleManager`\>.  
  
4.  Ajoutez un élément \<`add`\> en tant qu'enfant de l'élément \<`providers`\>, avec les attributs suivants définis aux valeurs appropriées : `name`, `type`, `connectionStringName`et `applicationName`, comme illustré dans l'exemple suivant.  
  
    ```  
    <!-- Configure the Sql Role Provider. -->  
    <roleManager enabled ="true"   
     defaultProvider ="SqlRoleProvider" >  
       <providers>  
         <add name ="SqlRoleProvider"   
           type="System.Web.Security.SqlRoleProvider"   
           connectionStringName="SqlConn"   
           applicationName="MembershipAndRoleProviderSample"/>  
       </providers>  
    </roleManager>  
    ```  
  
### Pour configurer le service pour utiliser le fournisseur de rôle  
  
1.  Dans le fichier Web.config, ajoutez un élément [\<system.serviceModel\>](../../../../docs/framework/configure-apps/file-schema/wcf/system-servicemodel.md).  
  
2.  Ajoutez un élément [\<comportements\>](../../../../docs/framework/configure-apps/file-schema/wcf/behaviors.md) à l'élément \<`system.ServiceModel`\>.  
  
3.  Ajoutez un [\<serviceBehaviors\>](../../../../docs/framework/configure-apps/file-schema/wcf/servicebehaviors.md) à l'élément \<`behaviors`\>.  
  
4.  Ajoutez un élément [\<comportement\>](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) et affectez une valeur appropriée à l'attribut `name`.  
  
5.  Ajoutez un [\<serviceAuthorization\>](../../../../docs/framework/configure-apps/file-schema/wcf/serviceauthorization-element.md) à l'élément \<`behavior`\>.  
  
6.  Affectez à l'attribut `principalPermissionMode` la valeur `UseAspNetRoles`.  
  
7.  Affectez à l'attribut `roleProviderName` la valeur `SqlRoleProvider`.L'exemple suivant présente un fragment de la configuration.  
  
    ```  
    <behaviors>  
     <serviceBehaviors>  
      <behavior name="CalculatorServiceBehavior">  
       <serviceAuthorization principalPermissionMode ="UseAspNetRoles"  
                             roleProviderName ="SqlRoleProvider" />  
      </behavior>  
     </serviceBehaviors>  
    </behaviors>  
    ```  
  
## Voir aussi  
 [Membership and Role Provider](../../../../docs/framework/wcf/samples/membership-and-role-provider.md)   
 [Comment : utiliser le fournisseur d'appartenances ASP.NET](../../../../docs/framework/wcf/feature-details/how-to-use-the-aspnet-membership-provider.md)