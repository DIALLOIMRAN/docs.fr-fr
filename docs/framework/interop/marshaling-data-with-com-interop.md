---
title: "Marshaling Data with COM Interop | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "COM interop, data marshaling"
  - "marshaling data, COM interop"
ms.assetid: 0bcdd7bf-5026-43eb-b08b-f03f90db9df9
caps.latest.revision: 12
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 12
---
# Marshaling Data with COM Interop
COM Interop prend en charge l'utilisation des objets COM à partir de code managé, ainsi que l'exposition des objets managés à COM.  La prise en charge du marshaling des données vers et depuis COM est complète et fournit quasiment toujours le comportement de marshaling approprié.  
  
 Le [!INCLUDE[winsdklong](../../../includes/winsdklong-md.md)] comprend les outils COM Interop suivants :  
  
-   [Importateur de bibliothèques de types \(Tlbimp.exe\)](../../../docs/framework/tools/tlbimp-exe-type-library-importer.md), qui convertit une bibliothèque de types COM en assembly d'interopérabilité.  À partir de cet assembly, le service de marshaling d'interopérabilité génère des wrappers qui effectuent le marshaling des données entre la mémoire managée et non managée.  
  
-   [Exportateur de bibliothèques de types \(Tlbexp.exe\)](../../../docs/framework/tools/tlbexp-exe-type-library-exporter.md), qui produit une bibliothèque de types COM à partir d'un assembly et génère un wrapper qui effectue le marshaling lors des appels de méthode.  
  
 Cette section décrit les processus de personnalisation des wrappers d'interopérabilité quand vous pouvez \(ou devez\) fournir au marshaleur des informations supplémentaires concernant les types.  
  
## Dans cette section  
 [Types de données COM](http://msdn.microsoft.com/fr-fr/f93ae35d-a416-4218-8700-c8218cc90061)  
 Fournit les types de données managés et non managés correspondants.  
  
 [Personnalisation des wrappers CCW](http://msdn.microsoft.com/fr-fr/825177d3-4b2c-4723-82be-ce6ca2c34ace)  
 Décrit comment marshaler explicitement des types de données à l'aide de l'attribut **MarshalAsAttribute** au moment du design.  
  
 [Personnalisation des wrappers RCW](http://msdn.microsoft.com/fr-fr/4652beaf-77d0-4f37-9687-ca193288c0be)  
 Décrit comment ajuster le comportement de marshaling des types dans un assembly d'interopérabilité et comment définir des types COM manuellement.  
  
 [Comment : migrer DCOM de code managé vers WCF](../../../docs/framework/interop/how-to-migrate-managed-code-dcom-to-wcf.md)  
 Décrit comment migrer du code DCOM managé vers WCF pour la solution la plus sécurisée.  
  
## Rubriques connexes  
 [Advanced COM Interoperability](http://msdn.microsoft.com/fr-fr/3ada36e5-2390-4d70-b490-6ad8de92f2fb)  
 Fournit des liens vers des informations sur l'incorporation de composants COM dans une application .NET Framework.  
  
 [Assembly to Type Library Conversion Summary](http://msdn.microsoft.com/fr-fr/3a37eefb-a76c-4000-9080-7dbbf66a4896)  
 Décrit le processus d'exportation et de conversion d'un assembly en une bibliothèque de types.  
  
 [Type Library to Assembly Conversion Summary](http://msdn.microsoft.com/fr-fr/bf3f90c5-4770-4ab8-895c-3ba1055cc958)  
 Décrit le processus d'importation et de conversion d'une bibliothèque de types en un assembly.  
  
 [Interoperating Using Generic Types](http://msdn.microsoft.com/fr-fr/26b88e03-085b-4b53-94ba-a5a9c709ce58)  
 Décrit les actions prises en charge lors de l'utilisation de types génériques pour l'interopérabilité COM.