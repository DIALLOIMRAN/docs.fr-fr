---
title: "R&#232;gles pour l&#39;inf&#233;rence de types et de structure de nœud de sch&#233;ma | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
ms.assetid: d74ce896-717d-4871-8fd9-b070e2f53cb0
caps.latest.revision: 2
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 2
---
# R&#232;gles pour l&#39;inf&#233;rence de types et de structure de nœud de sch&#233;ma
Cette rubrique décrit comment le processus d'inférence de schéma convertit les types de nœuds d'un document XML en une structure en langage XSD \(XML Schema Definition\).  
  
## Règles d'inférence d'élément  
 Cette section décrit les règles d'inférence pour les déclarations d'élément.  Huit structures de déclaration d'élément seront déduites :  
  
1.  élément de type simple ;  
  
2.  élément vide ;  
  
3.  élément vide avec attributs ;  
  
4.  élément avec attributs et contenu simple ;  
  
5.  élément avec une séquence d'éléments enfants ;  
  
6.  élément avec une séquence d'éléments enfants et d'attributs ;  
  
7.  élément avec une séquence de choix d'éléments enfants ;  
  
8.  élément avec une séquence de choix d'éléments enfants et d'attributs.  
  
> [!NOTE]
>  Toutes les déclarations `complexType` sont déduites comme des types anonymes.  Le seul élément global déduit est l'élément racine ; tous les autres éléments sont locaux.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
### Élément de type simple  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  L'élément en gras montre le schéma déduit pour l'élément de type simple.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root>text</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="root" type="xs:string" />`<br /><br /> `\</xs:schema>`|  
  
### Élément vide  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  L'élément en gras montre le schéma déduit pour l'élément vide.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<empty/>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="empty" />`<br /><br /> `\</xs:schema>`|  
  
### Élément vide avec attributs  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  Les éléments en gras montrent le schéma déduit pour l'élément vide avec des attributs.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<empty attribute1="text"/>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="empty">`<br /><br /> `\<xs:complexType>`<br /><br /> `\<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `\</xs:complexType>`<br /><br /> `\</xs:element>`<br /><br /> `\</xs:schema>`|  
  
### Élément avec attributs et contenu simple  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  Les éléments en gras montrent le schéma déduit pour un élément avec des attributs et un contenu simple.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root attribute1="text">value</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="root">`<br /><br /> `\<xs:complexType>`<br /><br /> `\<xs:simpleContent>`<br /><br /> `\<xs:extension base="xs:string">`<br /><br /> `\<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `\</xs:extension>`<br /><br /> `\</xs:simpleContent>`<br /><br /> `\</xs:complexType>`<br /><br /> `\</xs:element>`<br /><br /> `\</xs:schema>`|  
  
### Élément avec une séquence d'éléments enfants  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  Les éléments en gras montrent le schéma déduit pour un élément avec une séquence d'éléments enfants.  
  
> [!NOTE]
>  Même si un élément n'a qu'un seul élément enfant, il est traité comme une séquence.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root>`<br /><br /> `<subElement/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="root">`<br /><br /> `\<xs:complexType>`<br /><br /> `\<xs:sequence>`<br /><br /> `\<xs:element name="subElement" />`<br /><br /> `\</xs:sequence>`<br /><br /> `\</xs:complexType>`<br /><br /> `\</xs:element>`<br /><br /> `\</xs:schema>`|  
  
### Élément avec une séquence d'éléments enfants et d'attributs  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  Les éléments en gras montrent le schéma déduit pour un élément avec une séquence d'éléments enfants et d'attributs.  
  
> [!NOTE]
>  Même si un élément n'a qu'un seul élément enfant, il est traité comme une séquence.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root attribute1="text">`<br /><br /> `<subElement1/>`<br /><br /> `<subElement2/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="root">`<br /><br /> `\<xs:complexType>`<br /><br /> `\<xs:sequence>`<br /><br /> `\<xs:element name="subElement1" />`<br /><br /> `\<xs:element name="subElement2" />`<br /><br /> `\</xs:sequence>`<br /><br /> `\<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `\</xs:complexType>`<br /><br /> `\</xs:element>`<br /><br /> `\</xs:schema>`|  
  
### Élément avec une séquence de choix et d'éléments enfants  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  Les éléments en gras montrent le schéma déduit pour un élément avec une séquence et un choix d'éléments enfants.  
  
> [!NOTE]
>  L'attribut `maxOccurs` de l'élément `xs:choice` a la valeur `"unbounded"` dans le schéma déduit.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root>`<br /><br /> `<subElement1/>`<br /><br /> `<subElement2/>`<br /><br /> `<subElement1/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="root">`<br /><br /> `\<xs:complexType>`<br /><br /> `\<xs:sequence>`<br /><br /> `\<xs:choice maxOccurs="unbounded">`<br /><br /> `\<xs:element name="subElement1" />`<br /><br /> `\<xs:element name="subElement2" />`<br /><br /> `\</xs:choice>`<br /><br /> `\</xs:sequence>`<br /><br /> `\</xs:complexType>`<br /><br /> `\</xs:element>`<br /><br /> `\</xs:schema>`|  
  
### Élément avec une séquence et un choix d'éléments enfants et d'attributs  
 Le tableau suivant présente l'entrée XML dans la méthode <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> et le schéma XML généré.  Les éléments en gras montrent le schéma déduit pour un élément avec une séquence et un choix d'éléments enfants et d'attributs.  
  
> [!NOTE]
>  L'attribut `maxOccurs` de l'élément `xs:choice` a la valeur `"unbounded"` dans le schéma déduit.  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
|XML|Schéma|  
|---------|------------|  
|`<?xml version="1.0"?>`<br /><br /> `<root attribute1="text">`<br /><br /> `<subElement1/>`<br /><br /> `<subElement2/>`<br /><br /> `<subElement1/>`<br /><br /> `</root>`|`<?xml version="1.0" encoding="utf-8"?>`<br /><br /> `<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xml`<br /><br /> `ns:xs="http://www.w3.org/2001/XMLSchema">`<br /><br /> ``   `\<xs:element name="root">`<br /><br /> `\<xs:complexType>`<br /><br /> `\<xs:sequence>`<br /><br /> `\<xs:choice maxOccurs="unbounded">`<br /><br /> `\<xs:element name="subElement1" />`<br /><br /> `\<xs:element name="subElement2" />`<br /><br /> `\</xs:choice>`<br /><br /> `\</xs:sequence>`<br /><br /> `\<xs:attribute name="attribute1" type="xs:string" use="required" />`<br /><br /> `\</xs:complexType>`<br /><br /> `\</xs:element>`<br /><br /> `\</xs:schema>`|  
  
### Traitement d'attribut  
 Chaque fois qu'un nouvel attribut est rencontré dans un nœud, il est ajouté à la définition déduite du nœud avec `use="required"`.  La prochaine fois que le même nœud est trouvé dans l'instance, le processus d'inférence comparera les attributs de l'instance actuelle avec ceux déjà déduits.  Si certains des attributs déjà déduits manquent dans l'instance, `use="optional"` est ajouté à la définition d'attribut.  De nouveaux attributs sont ajoutés aux déclarations existantes avec `use="optional"`.  
  
### Contraintes d'occurrence  
 Durant le processus d'inférence de schéma, les attributs `minOccurs` et `maxOccurs` sont générés, pour des composants déduits d'un schéma, avec les valeurs `"0"` ou `"1"` et `"1"` ou `"unbounded"`.  Les valeurs `"1"` et `"unbounded"` sont utilisées uniquement lorsque les valeurs `"0"` et `"1"` ne peuvent pas valider le document XML \(par exemple, si `MinOccurs="0"` ne décrit pas précisément un élément, `minOccurs="1"` est utilisé\).  
  
### Contenu mixte  
 Si un élément contient un contenu mixte \(par exemple, du texte intercalé avec des éléments\), l'attribut `mixed="true"` est généré pour la définition de type complexe déduite.  
  
## Autres règles d'inférence de type de nœud  
 Le tableau suivant décrit les règles d'inférence pour le traitement d'instruction, de commentaire, de référence d'entité, de CDATA, de type de document et de nœuds d'espace de noms.  
  
|Type de nœud|Traduction|  
|------------------|----------------|  
|Instruction de traitement|Ignoré.|  
|Commentaire|Ignoré.|  
|Référence d'entité|La classe <xref:System.Xml.Schema.XmlSchemaInference> ne gère pas les références d'entité.  Si un document XML contient des références d'entité, vous devez utiliser un lecteur qui étend les entités.  Par exemple, vous pouvez passer un objet <xref:System.Xml.XmlTextReader> avec la propriété <xref:System.Xml.XmlTextReader.EntityHandling%2A> définie à <xref:System.Xml.EntityHandling> comme paramètre.  Si des références d'entité sont rencontrées et que le lecteur n'étend pas les entités, une exception est levée.|  
|CDATA|Toute section `<![CDATA[ … ]]` dans un document XML sera déduite en tant que `xs:string`.|  
|Type de document|Ignoré.|  
|Espaces de noms|Ignoré.|  
  
 Pour plus d'informations sur le processus d'inférence de schéma, consultez [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md).  
  
## Voir aussi  
 <xref:System.Xml.Schema.XmlSchemaInference>   
 [Modèle Objet du schéma \(SOM\) XML](../../../../docs/standard/data/xml/xml-schema-object-model-som.md)   
 [Inférence d'un schéma XML](../../../../docs/standard/data/xml/inferring-an-xml-schema.md)   
 [Inférence de schémas à partir de documents XML](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md)   
 [Règles relatives à l'inférence de types simples](../../../../docs/standard/data/xml/rules-for-inferring-simple-types.md)