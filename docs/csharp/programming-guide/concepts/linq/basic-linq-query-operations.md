---
title: "Opérations de requête LINQ de base (C#)"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- orderby clause [LINQ in C#]
- ordering data [LINQ in C#]
- selecting data [LINQ in C#]
- queries [LINQ in C#], basic operations
- grouping data [LINQ in C#]
- data sources [LINQ in C#]
- sorting data [LINQ in C#]
- projections [LINQ in C#]
- filtering data [LINQ in C#]
- joining data [LINQ in C#]
- select clause [LINQ in C#]
- LINQ [C#], basic query operations
- join clause [LINQ in C#]
- group clause [LINQ in C#]
ms.assetid: a7ea3421-1cf4-4df7-832a-aa22fe6379e9
caps.latest.revision: 39
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
ms.translationtype: HT
ms.sourcegitcommit: 306c608dc7f97594ef6f72ae0f5aaba596c936e1
ms.openlocfilehash: e5dbebb7950678a0f40ec774d23b42dfe89cff49
ms.contentlocale: fr-fr
ms.lasthandoff: 07/28/2017

---
# <a name="basic-linq-query-operations-c"></a>Opérations de requête LINQ de base (C#)
Cette rubrique présente brièvement les expressions de requête [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] et quelques-uns des types d’opérations classiques que vous effectuez dans une requête. Vous trouverez des informations plus détaillées dans les rubriques suivantes :  
  
 [Expressions de requête LINQ](../../../../csharp/programming-guide/linq-query-expressions/index.md)  
  
 [Vue d’ensemble des opérateurs de requête standard (C#)](../../../../csharp/programming-guide/concepts/linq/standard-query-operators-overview.md)  
  
 [Procédure pas à pas : écriture de requêtes en C#](../../../../csharp/programming-guide/concepts/linq/walkthrough-writing-queries-linq.md)  
  
> [!NOTE]
>  Si vous êtes déjà familiarisé avec un langage de requête tel que SQL ou XQuery, vous pouvez ignorer la plupart des informations de cette rubrique. Consultez le paragraphe « Clause `from` » de la section suivante pour en savoir plus sur l’ordre des clauses dans les expressions de requête [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)].  
  
## <a name="obtaining-a-data-source"></a>Obtention d’une source de données  
 Dans une requête [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], la première étape consiste à spécifier la source de données. Dans C#, comme dans la plupart des langages de programmation, une variable doit être déclarée avant de pouvoir être utilisée. Dans une requête [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], la clause `from` apparaît en premier pour introduire la source de données (`customers`) et la *variable de portée* (`cust`).  
  
 [!code-cs[csLINQGettingStarted#23](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_1.cs)]  
  
 La variable de portée est similaire à la variable d’itération dans une boucle `foreach`, à la différence qu’aucune itération réelle ne se produit dans une expression de requête. Quand la requête est exécutée, la variable de portée sert de référence à chaque élément consécutif dans `customers`. Comme le compilateur déduit le type de `cust`, vous n’avez pas à le spécifier explicitement. Des variables de portée supplémentaires peuvent être introduites par une clause `let`. Pour plus d’informations, consultez [let, clause](../../../../csharp/language-reference/keywords/let-clause.md).  
  
> [!NOTE]
>  Pour les sources de données non génériques telles que <xref:System.Collections.ArrayList>, la variable de portée doit être explicitement typée. Pour plus d’informations, consultez [Guide pratique pour interroger un ArrayList avec LINQ (C#)](../../../../csharp/programming-guide/concepts/linq/how-to-query-an-arraylist-with-linq.md) et [from, clause](../../../../csharp/language-reference/keywords/from-clause.md).  
  
## <a name="filtering"></a>Filtrage  
 L'opération de requête la plus courante est probablement l'application d'un filtre sous forme d'expression booléenne. Du fait du filtre, la requête retourne uniquement les éléments pour lesquels l’expression a la valeur true. Le résultat est produit à l'aide de la clause `where`. En effet, le filtre spécifie les éléments à exclure de la séquence source. Dans l’exemple suivant, seuls les clients (`customers`) qui ont une adresse à Londres sont retournés.  
  
 [!code-cs[csLINQGettingStarted#24](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_2.cs)]  
  
 Vous pouvez utiliser les opérateurs logiques C# `AND` et `OR` habituels pour appliquer le nombre d’expressions de filtre nécessaire dans la clause `where`. Par exemple, pour retourner uniquement les clients situés à « Londres » ET (`AND`) dont le nom est « Devon », vous devez écrire le code suivant :  
  
 [!code-cs[csLINQGettingStarted#25](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_3.cs)]  
  
 Pour retourner les clients situés à Londres ou à Paris, vous devez écrire le code suivant :  
  
 [!code-cs[csLINQGettingStarted#26](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_4.cs)]  
  
 Pour plus d’informations, consultez [where, clause](../../../../csharp/language-reference/keywords/where-clause.md).  
  
## <a name="ordering"></a>Classement  
 Il est souvent utile de trier les données retournées. La clause `orderby` permet de trier les éléments de la séquence retournée en fonction du comparateur par défaut pour le type qui est trié. Par exemple, la requête suivante peut être étendue pour trier les résultats selon la propriété `Name`. Comme `Name` est une chaîne, le comparateur par défaut effectue un tri alphabétique de A à Z.  
  
 [!code-cs[csLINQGettingStarted#27](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_5.cs)]  
  
 Pour trier les résultats dans l’ordre inverse, de Z à A, utilisez la clause `orderby…descending`.  
  
 Pour plus d’informations, consultez [orderby, clause](../../../../csharp/language-reference/keywords/orderby-clause.md).  
  
## <a name="grouping"></a>Regroupement  
 La clause `group` vous permet de grouper vos résultats selon une clé que vous spécifiez. Par exemple, vous pouvez spécifier un regroupement des résultats par ville (`City`) pour que tous les clients de Londres ou Paris soient dans des groupes individuels. Dans ce cas, utilisez la clé `cust.City`.  
  
 [!code-cs[csLINQGettingStarted#28](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_6.cs)]  
  
 Quand vous terminez une requête avec une clause `group`, les résultats sont présentés sous la forme d’une liste de listes. Chaque élément de la liste est un objet qui a un membre `Key` et une liste d’éléments qui sont regroupés sous cette clé. Quand vous itérez une requête qui produit une séquence de groupes, vous devez utiliser une boucle `foreach` imbriquée. La boucle externe itère chaque groupe et la boucle interne itère les membres de chaque groupe.  
  
 Si vous devez faire référence aux résultats d’une opération de regroupement, utilisez le mot clé `into` pour créer un identificateur susceptible d’être interrogé ultérieurement. La requête suivante retourne uniquement les groupes qui contiennent plus de deux clients :  
  
 [!code-cs[csLINQGettingStarted#29](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_7.cs)]  
  
 Pour plus d’informations, consultez [group, clause](../../../../csharp/language-reference/keywords/group-clause.md).  
  
## <a name="joining"></a>Jointure  
 Les opérations de jointure créent des associations entre les séquences qui ne sont pas explicitement modélisées dans les sources de données. Par exemple, effectuez une jointure pour rechercher tous les clients et distributeurs qui se trouvent au même endroit. Dans [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], la clause `join` fonctionne toujours par rapport à des collections d’objets plutôt que directement par rapport à des tables de base de données.  
  
 [!code-cs[csLINQGettingStarted#36](../../../../csharp/programming-guide/concepts/linq/codesnippet/CSharp/basic-linq-query-operations_8.cs)]  
  
 Dans [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)], vous n’avez pas à utiliser `join` aussi souvent que vous le faites dans SQL, car les clés étrangères dans [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)] sont représentées dans le modèle objet comme des propriétés qui contiennent une collection d’éléments. Par exemple, un objet `Customer` contient une collection d’objets `Order`. Au lieu d’effectuer une jointure, accédez aux commandes en utilisant la notation par points :  
  
```  
from order in Customer.Orders...  
```  
  
 Pour plus d’informations, consultez [join, clause](../../../../csharp/language-reference/keywords/join-clause.md).  
  
## <a name="selecting-projections"></a>Sélection (projections)  
 La clause `select` produit les résultats de la requête et spécifie la « forme » ou le type de chaque élément retourné. Par exemple, spécifiez si les résultats doivent contenir des objets `Customer` complets, un seul membre, un sous-ensemble de membres ou un type de résultat complètement différent basé sur un calcul ou une création d’objet. Quand la clause `select` produit autre chose qu’une copie de l’élément source, l’opération est appelée *projection*. L’utilisation de projections pour transformer des données est une fonctionnalité puissante des expressions de requête [!INCLUDE[vbteclinq](~/includes/vbteclinq-md.md)]. Pour plus d’informations, consultez [Transformations de données avec LINQ (C#)](../../../../csharp/programming-guide/concepts/linq/data-transformations-with-linq.md) et [select, clause](../../../../csharp/language-reference/keywords/select-clause.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Bien démarrer avec LINQ en C#](../../../../csharp/programming-guide/concepts/linq/getting-started-with-linq.md)   
 [Expressions de requête LINQ](../../../../csharp/programming-guide/linq-query-expressions/index.md)   
 [Procédure pas à pas : écriture de requêtes en C#](../../../../csharp/programming-guide/concepts/linq/walkthrough-writing-queries-linq.md)   
 [Mots clés de requête (LINQ)](../../../../csharp/language-reference/keywords/query-keywords.md)   
 [Types anonymes](../../../../csharp/programming-guide/classes-and-structs/anonymous-types.md)

