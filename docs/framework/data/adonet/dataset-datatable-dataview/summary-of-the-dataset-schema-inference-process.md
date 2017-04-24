---
title: "R&#233;sum&#233; du processus d&#39;inf&#233;rence du sch&#233;ma d&#39;un DataSet | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: fd0891c8-d068-4e30-a76f-7c375f078bf7
caps.latest.revision: 4
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 4
---
# R&#233;sum&#233; du processus d&#39;inf&#233;rence du sch&#233;ma d&#39;un DataSet
Le processus d'inférence identifie d'abord, à partir du document XML, les éléments qui seront déduits en tant que tables.  À partir du XML restant, le processus d'inférence détermine les colonnes qui feront partie de ces tables.  Pour les tables imbriquées, le processus d'inférence génère des objets <xref:System.Data.DataRelation> et <xref:System.Data.ForeignKeyConstraint> imbriqués.  
  
 Les règles d'inférence sont brièvement résumées ci\-après :  
  
-   Les éléments assortis d'attributs sont déduits en tant que tables.  
  
-   Les éléments possédant des éléments enfants sont déduits en tant que tables.  
  
-   Les éléments qui se répètent sont déduits une seule fois en tant que table.  
  
-   Si l'élément de document, ou racine, ne comporte pas d'attributs ni d'éléments enfants qui seraient déduits en tant que colonnes, il est déduit en tant qu'objet <xref:System.Data.DataSet>.  Sinon, l'élément de document est déduit en tant que table.  
  
-   Les attributs sont déduits en tant que colonnes.  
  
-   Les éléments dépourvus d'attributs ou d'éléments enfants et qui ne se répètent pas sont déduits en tant que colonnes.  
  
-   Pour les éléments qui sont déduits en tant que tables imbriquées dans d'autres éléments également déduits en tant que tables, un **DataRelation** imbriqué est créé entre les deux tables.  Une nouvelle colonne de clé primaire, nommée **TableName\_Id**, est ajoutée aux deux tables et utilisée par le **DataRelation**.  Un **ForeignKeyConstraint** est créé entre les deux tables utilisant la colonne **TableName\_Id**.  
  
-   Pour les éléments déduits en tant que tables et qui contiennent du texte mais n'ont pas d'éléments enfants, une nouvelle colonne nommée **TableName\_Text** est créée pour le texte de chacun de ces éléments.  Si un élément déduit en tant que table comprend du texte, mais également des éléments enfants, le texte est ignoré.  
  
## Voir aussi  
 [Inférence de la structure relationnelle d'un DataSet à partir de XML](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/inferring-dataset-relational-structure-from-xml.md)   
 [Chargement d'un DataSet à partir de XML](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/loading-a-dataset-from-xml.md)   
 [Chargement des informations de schéma d'un DataSet à partir de XML](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/loading-dataset-schema-information-from-xml.md)   
 [Utilisation de XML dans un DataSet](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/using-xml-in-a-dataset.md)   
 [Objets DataSet, DataTable et DataView](../../../../../docs/framework/data/adonet/dataset-datatable-dataview/index.md)   
 [Fournisseurs managés ADO.NET et Centre de développement de DataSet](http://go.microsoft.com/fwlink/?LinkId=217917)