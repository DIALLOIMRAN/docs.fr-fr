---
title: "TextMessageEncodingBindingElement | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 885e2d7a-3436-4093-bc5f-0a404c62acdc
caps.latest.revision: 8
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 8
---
# TextMessageEncodingBindingElement
TextMessageEncodingBindingElement  
  
## Syntaxe  
  
```  
class TextMessageEncodingBindingElement : MessageEncodingBindingElement  
{  
  string Encoding;  
  sint32 MaxReadPoolSize;  
  sint32 MaxWritePoolSize;  
  XmlDictionaryReaderQuotas ReaderQuotas;  
};  
```  
  
## Méthodes  
 La classe TextMessageEncodingBindingElement ne définit pas de méthodes.  
  
## Propriétés  
 La classe TextMessageEncodingBindingElement a les propriétés suivantes :  
  
### Encodage  
 Type de données : chaîne  
  
 Type d'accès : lecture seule  
  
 Encodage de jeu de caractères à utiliser pour l'émission de messages sur la liaison.  
  
### MaxReadPoolSize  
 Type de données : sint32  
  
 Type d'accès : lecture seule  
  
 Entier qui définit combien de messages peuvent être lus de manière simultanée sans allouer de nouveaux lecteurs.  
  
### MaxWritePoolSize  
 Type de données : sint32  
  
 Type d'accès : lecture seule  
  
 Entier qui définit combien de messages peuvent être envoyés simultanément sans allouer de nouveaux enregistreurs.  
  
### ReaderQuotas  
 Type de données : XmlDictionaryReaderQuotas  
  
 Type d'accès : lecture seule  
  
 Quotas des lecteurs.  
  
## Spécifications  
  
|MOF|Déclaré dans Servicemodel.mof.|  
|---------|------------------------------------|  
|Espace de noms|Défini dans root\\ServiceModel|  
  
## Voir aussi  
 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>