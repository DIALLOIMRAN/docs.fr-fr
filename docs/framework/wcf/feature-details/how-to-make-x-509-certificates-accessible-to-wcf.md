---
title: "Comment&#160;: rendre des certificats X.509 accessibles &#224; WCF | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "certificats (WCF), rendre des certificats X.509 accessibles à WCF"
  - "certificats X.509 (WCF)"
  - "certificats X.509 (WCF), rendre accessible à WCF"
ms.assetid: a54e407c-c2b5-4319-a648-60e43413664b
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# Comment&#160;: rendre des certificats X.509 accessibles &#224; WCF
Pour rendre un certificat X.509 accessible à [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)], le code d'application doit spécifier le nom et l'emplacement du magasin de certificats.Dans certains cas, l'identité du processus doit avoir accès au fichier contenant la clé privée associée au certificat X.509.Pour obtenir la clé privée associée à un certificat X.509 dans un magasin de certificats, [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] doit disposer de l'autorisation requise.Par défaut, seuls le propriétaire et le compte système peuvent accéder à la clé privée d'un certificat.  
  
### Pour rendre des certificats X.509 accessibles à WCF  
  
1.  Accordez au compte sous lequel [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] s'exécute l'accès en lecture au fichier contenant la clé privée associée au certificat X.509.  
  
    1.  Déterminez si [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] requiert l'accès en lecture à la clé privée du certificat X.509.  
  
         Le tableau suivant précise si une clé privée doit être disponible lors de l'utilisation d'un certificat X.509.  
  
        |Utilisation d'un certificat X.509|Clé privée|  
        |---------------------------------------|----------------|  
        |Signature numérique d'un message SOAP sortant.|Oui|  
        |Vérification de la signature d'un message SOAP entrant.|Non|  
        |Chiffrement d'un message SOAP sortant.|Non|  
        |Déchiffrement d'un message SOAP entrant.|Oui|  
  
    2.  Déterminez l'emplacement et le nom du magasin de certificats dans lequel le certificat est stocké.  
  
         Le magasin de certificats dans lequel le certificat est stocké est spécifié dans le code d'application ou dans la configuration.Ainsi, l'exemple suivant spécifie que le certificat se trouve dans le magasin de certificats `CurrentUser` nommé `My`.  
  
         [!code-csharp[x509Accessible#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/x509accessible/cs/source.cs#1)]
         [!code-vb[x509Accessible#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/x509accessible/vb/source.vb#1)]  
  
    3.  Déterminez l'emplacement de la clé privée du certificat sur l'ordinateur à l'aide de l'outil [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md).  
  
         L'outil [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md) requiert le nom et l'emplacement du magasin de certificats, et un élément qui identifie le certificat de façon unique.Il accepte le nom de sujet du certificat ou son empreinte numérique comme identificateur unique.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] la manière de déterminer l'empreinte numérique d'un certificat, consultez [Comment : récupérer l'empreinte numérique d'un certificat](../../../../docs/framework/wcf/feature-details/how-to-retrieve-the-thumbprint-of-a-certificate.md).  
  
         L'exemple de code suivant utilise l'outil [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md) pour déterminer l'emplacement de la clé privée d'un certificat dans le magasin `My` dans `CurrentUser` avec une empreinte numérique de `46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d`.  
  
        ```  
        findprivatekey.exe My CurrentUser -t "46 dd 0e 7a ed 0b 7a 31 9b 02 a3 a0 43 7a d8 3f 60 40 92 9d" -a  
        ```  
  
    4.  Déterminez le compte sous lequel [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] s'exécute.  
  
         Le tableau suivant précise le compte sous lequel [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] s'exécute pour un scénario donné.  
  
        |Scénario|Identité du processus|  
        |--------------|---------------------------|  
        |Client \(console ou application WinForms\).|Utilisateur actuellement connecté.|  
        |Service auto\-hébergé.|Utilisateur actuellement connecté.|  
        |Service hébergé dans IIS 6.0 \([!INCLUDE[ws2003](../../../../includes/ws2003-md.md)]\) ou IIS 7.0 \([!INCLUDE[wv](../../../../includes/wv-md.md)]\).|SERVICE RÉSEAU|  
        |Service hébergé dans IIS 5.X \([!INCLUDE[wxp](../../../../includes/wxp-md.md)]\).|Contrôlé par l'élément `<processModel>` dans le fichier Machine.config.Le compte par défaut est ASPNET.|  
  
    5.  Accordez l'accès en lecture au fichier contenant la clé privée du compte sous lequel [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] s'exécute, à l'aide d'un outil tel que cacls.exe.  
  
         L'exemple de code suivant modifie \(\/E\) la liste de contrôle d'accès \(ACL\) du fichier spécifié pour accorder \(\/G\) au compte SERVICE RÉSEAU l'accès en lecture \(: R\) au fichier.  
  
        ```  
        cacls.exe "C:\Documents and Settings\All Users\Application Data\Microsoft\Crypto\RSA\MachineKeys\8aeda5eb81555f14f8f9960745b5a40d_38f7de48-5ee9-452d-8a5a-92789d7110b1" /E /G "NETWORK SERVICE":R  
        ```  
  
## Voir aussi  
 [FindPrivateKey](../../../../docs/framework/wcf/samples/findprivatekey.md)   
 [Comment : récupérer l'empreinte numérique d'un certificat](../../../../docs/framework/wcf/feature-details/how-to-retrieve-the-thumbprint-of-a-certificate.md)   
 [Utilisation des certificats](../../../../docs/framework/wcf/feature-details/working-with-certificates.md)