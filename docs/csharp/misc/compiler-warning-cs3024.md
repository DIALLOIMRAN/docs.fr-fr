---
title: "Avertissement du compilateur CS3024 | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.technology: 
  - "devlang-csharp"
ms.topic: "article"
f1_keywords: 
  - "CS3024"
dev_langs: 
  - "CSharp"
helpviewer_keywords: 
  - "CS3024"
ms.assetid: fef9db31-9a7f-42d5-ad37-3e7faf661f95
caps.latest.revision: 7
author: "BillWagner"
ms.author: "wiwagn"
caps.handback.revision: 7
---
# Avertissement du compilateur CS3024
Le type de contrainte 'type' n’est pas conforme CLS  
  
 Le compilateur émet cet avertissement, car l’utilisation d’un type non conforme CLS comme contrainte de type générique peut empêcher le code écrit dans certains langages de consommer votre classe générique.  
  
### Pour supprimer cet avertissement  
  
1.  Utilisez un type conforme CLS pour la contrainte de type.  
  
## Exemple  
 L’exemple suivant génère l’avertissement CS3024 à plusieurs emplacements :  
  
```  
// cs3024.cs // Compile with: /target:library [assembly: System.CLSCompliant(true)] [type: System.CLSCompliant(false)] public class TestClass // CS3024 { public ushort us; } [type: System.CLSCompliant(false)] public interface ITest // CS3024 {} public interface I<T> where T : TestClass {} public class TestClass_2<T> where T : ITest {} public class TestClass_3<T> : I<T> where T : TestClass {} public class TestClass_4<T> : TestClass_2<T> where T : ITest {} public class Test { public static int Main() { return 0; } }  
```  
  
## Voir aussi  
 [Contraintes sur les paramètres de type](../../csharp/programming-guide/generics/constraints-on-type-parameters.md)