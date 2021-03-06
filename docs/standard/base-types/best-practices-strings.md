---
title: "Meilleures pratiques pour l&#39;utilisation de cha&#238;nes dans le .NET Framework | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-standard"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "meilleures pratiques, comparaison et tri de chaînes"
  - "comparer des chaînes"
  - "trier des chaînes"
  - "comparaisons de chaînes (.NET Framework), meilleures pratiques"
  - "tri de chaînes"
  - "chaînes (.NET Framework), opérations de chaîne de base"
  - "chaînes (.NET Framework), meilleures pratiques"
  - "chaînes (.NET Framework), comparer"
  - "chaînes (.NET Framework), rechercher"
  - "chaînes (.NET Framework), trier"
ms.assetid: b9f0bf53-e2de-4116-8ce9-d4f91a1df4f7
caps.latest.revision: 35
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 35
---
# Meilleures pratiques pour l&#39;utilisation de cha&#238;nes dans le .NET Framework
<a name="top"></a> Le .NET Framework offre une prise en charge complète du développement d'applications localisées et globalisées, et facilite l'application des conventions de la culture actuelle ou d'une culture spécifique lors de l'exécution d'opérations courantes telles que le tri et l'affichage de chaînes. Toutefois, le tri ou la comparaison de chaînes n'est pas toujours une opération dépendante de la culture. Par exemple, les chaînes utilisées en interne par une application doivent généralement être gérées de la même manière dans toutes les cultures. Quand des données de type chaîne culturellement indépendantes, telles que des balises XML, des balises HTML, des noms d'utilisateurs, des chemins d'accès aux fichiers et des noms d'objets système, sont interprétées comme si elles étaient dépendantes de la culture, le code d'application peut faire l'objet de bogues subtils, de performances médiocres et, dans certains cas, de problèmes de sécurité.  
  
 Cette rubrique examine les méthodes de tri, de comparaison et d'application de la casse dans .NET Framework, présente les recommandations pour sélectionner une méthode appropriée de gestion des chaînes et fournit des informations supplémentaires concernant les méthodes de gestion des chaînes. Elle décrit également comment les données mises en forme, telles que les données numériques et les données de date et d'heure, sont traitées pour l'affichage et pour le stockage.  
  
 Cette rubrique contient les sections suivantes :  
  
-   [Recommandations relatives à l'utilisation de chaînes](#recommendations_for_string_usage)  
  
-   [Spécification explicite de comparaisons de chaînes](#specifying_string_comparisons_explicitly)  
  
-   [Détails de la comparaison de chaînes](#the_details_of_string_comparison)  
  
-   [Choix d'un membre StringComparison pour votre appel de méthode](#choosing_a_stringcomparison_member_for_your_method_call)  
  
-   [Méthodes courantes de comparaison de chaînes dans le .NET Framework](#common_string_comparison_methods_in_the_net_framework)  
  
-   [Méthodes qui effectuent indirectement la comparaison de chaînes](#methods_that_perform_string_comparison_indirectly)  
  
-   [Affichage et persistance des données mises en forme](#Formatted)  
  
<a name="recommendations_for_string_usage"></a>   
## Recommandations relatives à l'utilisation de chaînes  
 Dans le cadre du développement à l'aide du .NET Framework, suivez les recommandations simples ci\-après quand vous utilisez des chaînes :  
  
-   Utilisez des surcharges qui spécifient explicitement les règles de comparaison de chaînes pour les opérations de chaînes. Cela implique généralement l'appel d'une surcharge de méthode avec un paramètre de type <xref:System.StringComparison>.  
  
-   Utilisez <xref:System.StringComparison?displayProperty=fullName> ou <xref:System.StringComparison?displayProperty=fullName> pour les comparaisons comme valeur par défaut sécurisée pour la correspondance de chaînes de culture non spécifiée.  
  
-   Pour obtenir de meilleures performances, utilisez des comparaisons avec <xref:System.StringComparison?displayProperty=fullName> ou <xref:System.StringComparison?displayProperty=fullName>.  
  
-   Utilisez des opérations de chaînes basées sur <xref:System.StringComparison?displayProperty=fullName> quand vous affichez la sortie à l'utilisateur.  
  
-   Utilisez les valeurs <xref:System.StringComparison?displayProperty=fullName> ou <xref:System.StringComparison?displayProperty=fullName> non linguistiques plutôt que des opérations de chaînes basées sur <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=fullName> quand la comparaison est linguistiquement non pertinente \(symbolique, par exemple\).  
  
-   Utilisez la méthode <xref:System.String.ToUpperInvariant%2A?displayProperty=fullName> à la place de la méthode <xref:System.String.ToLowerInvariant%2A?displayProperty=fullName> quand vous normalisez des chaînes pour la comparaison.  
  
-   Utilisez une surcharge de la méthode <xref:System.String.Equals%2A?displayProperty=fullName> pour tester si deux chaînes sont égales.  
  
-   Utilisez les méthodes <xref:System.String.Compare%2A?displayProperty=fullName> et <xref:System.String.CompareTo%2A?displayProperty=fullName> pour trier les chaînes, et non pour en vérifier l'égalité.  
  
-   Utilisez la mise en forme en fonction de la culture pour afficher des données non\-chaînées, telles que les nombres et les dates, dans une interface utilisateur. Utilisez la mise en forme en fonction de la culture dite indifférente pour rendre les données non\-chaînées sous forme de chaîne.  
  
 Quand vous utilisez des chaînes, évitez les pratiques suivantes :  
  
-   N'utilisez pas de surcharges qui ne spécifient pas explicitement ou implicitement les règles de comparaison de chaînes pour les opérations de chaînes.  
  
-   N'utilisez pas d'opérations de chaînes basées sur <xref:System.StringComparison?displayProperty=fullName> dans la plupart des cas. L'une des rares exceptions est quand vous rendez persistantes des données linguistiquement explicites, mais dont la culture n'est pas spécifiée.  
  
-   N'utilisez pas une surcharge de la méthode <xref:System.String.Compare%2A?displayProperty=fullName> ou <xref:System.String.CompareTo%2A>, et testez une valeur de retour de zéro pour déterminer si deux chaînes sont égales.  
  
-   N'utilisez pas la mise en forme qui tient compte de la culture pour rendre des données numériques ou les données de date et d'heure sous forme de chaîne.  
  
 [Retour au début](#top)  
  
<a name="specifying_string_comparisons_explicitly"></a>   
## Spécification explicite de comparaisons de chaînes  
 La plupart des méthodes de manipulation de chaînes dans le .NET Framework sont surchargées. Une ou plusieurs surcharges acceptent généralement des paramètres par défaut, contrairement à d'autres qui définissent avec précision la façon dont les chaînes doivent être comparées ou manipulées. La plupart des méthodes qui ne s'appuient pas sur des valeurs par défaut incluent un paramètre de type <xref:System.StringComparison>, qui est une énumération spécifiant explicitement des règles pour la comparaison de chaînes selon la culture et la casse. Le tableau suivant décrit les membres de l'énumération <xref:System.StringComparison>.  
  
|Membre StringComparison|Description|  
|-----------------------------|-----------------|  
|<xref:System.StringComparison>|Effectue une comparaison respectant la casse à l'aide de la culture actuelle.|  
|<xref:System.StringComparison>|Effectue une comparaison ne respectant pas la casse à l'aide de la culture actuelle.|  
|<xref:System.StringComparison>|Effectue une comparaison respectant la casse à l'aide de la culture dite indifférente.|  
|<xref:System.StringComparison>|Effectue une comparaison ne respectant pas la casse à l'aide de la culture dite indifférente.|  
|<xref:System.StringComparison>|Effectue une comparaison ordinale.|  
|<xref:System.StringComparison>|Effectue une comparaison ordinale ne respectant pas la casse.|  
  
 Par exemple, la méthode <xref:System.String.IndexOf%2A>, qui retourne l'index d'une sous\-chaîne contenue dans un objet <xref:System.String> correspondant à un caractère ou à une chaîne, a neuf surcharges :  
  
-   <xref:System.String.IndexOf%28System.Char%29>, <xref:System.String.IndexOf%28System.Char%2CSystem.Int32%29> et <xref:System.String.IndexOf%28System.Char%2CSystem.Int32%2CSystem.Int32%29>, qui effectuent par défaut une recherche ordinale \(respectant la casse et indépendante de la culture\) d'un caractère dans la chaîne.  
  
-   <xref:System.String.IndexOf%28System.String%29>, <xref:System.String.IndexOf%28System.String%2CSystem.Int32%29> et <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.Int32%29>, qui effectuent par défaut une recherche respectant la casse et dépendante de la culture d'une sous\-chaîne dans la chaîne.  
  
-   <xref:System.String.IndexOf%28System.String%2CSystem.StringComparison%29>, <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.StringComparison%29> et <xref:System.String.IndexOf%28System.String%2CSystem.Int32%2CSystem.Int32%2CSystem.StringComparison%29>, qui incluent un paramètre de type <xref:System.StringComparison> permettant la spécification de la forme de la comparaison.  
  
 Nous vous recommandons de sélectionner une surcharge qui n'utilise pas de valeurs par défaut, pour les raisons suivantes :  
  
-   Certaines surcharges ayant des paramètres par défaut \(celles qui recherchent un <xref:System.Char> dans l'instance de chaîne\) effectuent une comparaison ordinale, tandis que d'autres \(celles qui recherchent une chaîne dans l'instance de chaîne\) sont dépendantes de la culture. Il est difficile de mémoriser quelle méthode utilise quelle valeur par défaut, et les surcharges peuvent être facilement confondues.  
  
-   L'objectif du code qui s'appuie sur des valeurs par défaut pour les appels de méthode n'est pas clair. Dans l’exemple suivant, qui est basé sur des valeurs par défaut, il est difficile de savoir si le développeur voulait en fait une comparaison ordinale ou linguistique de deux chaînes, ou si une différence de casse entre `protocol` et "http" peut entraîner le retour de `false` par le test d’égalité.  
  
     [!code-csharp[Conceptual.Strings.BestPractices#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/explicitargs1.cs#1)]
     [!code-vb[Conceptual.Strings.BestPractices#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/explicitargs1.vb#1)]  
  
 En général, nous vous recommandons d'appeler une méthode qui ne s'appuie pas sur des valeurs par défaut, car elle rend l'objectif du code non équivoque. Cela rend ensuite le code plus lisible et plus facile à déboguer et à gérer. L'exemple suivant aborde les questions soulevées à propos de l'exemple précédent. Il explique que la comparaison ordinale est utilisée et que les différences de casse sont ignorées.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/explicitargs1.cs#2)]
 [!code-vb[Conceptual.Strings.BestPractices#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/explicitargs1.vb#2)]  
  
 [Retour au début](#top)  
  
<a name="the_details_of_string_comparison"></a>   
## Détails de la comparaison de chaînes  
 La comparaison de chaînes est le cœur de nombreuses opérations liées aux chaînes, en particulier le tri et le test d'égalité. Les chaînes sont triées dans un ordre déterminé : si "my" s'affiche avant "string" dans une liste triée de chaînes, "my" doit être considéré comme inférieur ou égal à "string". En outre, la comparaison définit implicitement l'égalité. L'opération de comparaison retourne zéro pour les chaînes qu'il estime égales. Considérer qu'aucune chaîne n'est inférieure à l'autre constitue une bonne interprétation. La plupart des opérations significatives impliquant des chaînes incluent l'une des procédures suivantes, ou les deux : comparaison avec une autre chaîne et exécution d'une opération de tri bien définie.  
  
 Toutefois, l'évaluation de l'égalité ou de l'ordre de tri de deux chaînes ne produit pas un résultat correct unique ; le résultat dépend des critères utilisés pour comparer les chaînes. En particulier, les comparaisons de chaînes qui sont ordinales ou basées sur les conventions de casse et de tri de la culture actuelle ou la culture dite indifférente \(culture aux paramètres régionaux non spécifiés basée sur la langue anglaise\) peuvent produire des résultats différents.  
  
<a name="current_culture"></a>   
### Comparaisons de chaînes qui utilisent la culture actuelle  
 L'un des critères à prendre en compte est l'utilisation des conventions de la culture actuelle lors de la comparaison de chaînes. Les comparaisons basées sur la culture actuelle utilisent la culture ou les paramètres régionaux actuels du thread. Si la culture n'est pas définie par l'utilisateur, sa valeur par défaut est le paramètre défini dans la fenêtre **Options régionales** du Panneau de configuration. Vous devez toujours utiliser des comparaisons basées sur la culture actuelle quand les données sont linguistiquement pertinentes et quand elles reflètent l'intervention de l'utilisateur dépendante de la culture.  
  
 Toutefois, le comportement de la comparaison et de la casse dans le .NET Framework change en fonction de la culture. Cela se produit quand une application s'exécute sur un ordinateur dont la culture est différente de celle de l'ordinateur sur lequel l'application a été développée, ou quand le thread en cours d'exécution change sa culture. Ce comportement est intentionnel, mais il reste peu évident à de nombreux développeurs. L'exemple suivant illustre les différences au niveau de l'ordre de tri entre les cultures Anglais \(États\-Unis\) \("en\-US"\) et Suédois \("sv\-SE"\). Notez que les mots "ångström", "Windows" et "Visual Studio" s'affichent à des positions différentes dans les tableaux de chaînes triées.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison1.cs#3)]
 [!code-vb[Conceptual.Strings.BestPractices#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison1.vb#3)]  
  
 Les comparaisons ne respectant pas la casse qui utilisent la culture actuelle sont identiques aux comparaisons dépendantes de la culture, mais elles ignorent la casse, comme défini par la culture actuelle du thread. Ce comportement peut également se manifester dans les ordres de tri.  
  
 Les comparaisons qui utilisent la sémantique de la culture actuelle sont la valeur par défaut pour les méthodes suivantes :  
  
-   les surcharges <xref:System.String.Compare%2A?displayProperty=fullName> qui n'incluent pas de paramètre <xref:System.StringComparison> ;  
  
-   les surcharges <xref:System.String.CompareTo%2A?displayProperty=fullName> ;  
  
-   la méthode <xref:System.String.StartsWith%28System.String%29?displayProperty=fullName> par défaut et la méthode <xref:System.String.StartsWith%28System.String%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=fullName> avec un paramètre <xref:System.Globalization.CultureInfo>`null` ;  
  
-   la méthode <xref:System.String.EndsWith%28System.String%29?displayProperty=fullName> par défaut et la méthode <xref:System.String.EndsWith%28System.String%2CSystem.Boolean%2CSystem.Globalization.CultureInfo%29?displayProperty=fullName> avec un paramètre <xref:System.Globalization.CultureInfo>`null` ;  
  
-   les surcharges <xref:System.String.IndexOf%2A?displayProperty=fullName> qui acceptent un <xref:System.String> comme paramètre de recherche et qui n'ont pas de paramètre <xref:System.StringComparison> ;  
  
-   les surcharges <xref:System.String.LastIndexOf%2A?displayProperty=fullName> qui acceptent un <xref:System.String> comme paramètre de recherche et qui n'ont pas de paramètre <xref:System.StringComparison> ;  
  
 Dans tous les cas, nous vous recommandons d'appeler une surcharge qui a un paramètre <xref:System.StringComparison>, afin que l'objectif de l'appel de la méthode soit clair.  
  
 Des bogues, plus ou moins subtils, peuvent émerger quand des données de type chaîne non linguistiques sont interprétées linguistiquement, ou quand des données de type chaîne d'une culture particulière sont interprétées à l'aide des conventions d'une autre culture. L'exemple canonique est le problème du caractère I en turc.  
  
 Dans presque tous les alphabets latins, y compris l'anglais \(États\-Unis\), le caractère "i" \(\\u0069\) est la version en minuscule du caractère "I" \(\\u0049\). Cette règle de casse devient rapidement la valeur par défaut pour quelqu'un qui effectue de la programmation dans une telle culture. Toutefois, l'alphabet turc \("tr\-TR"\) inclut un caractère "I avec un point", "İ" \(\\u0130\), qui est la version en majuscule de "i". Le turc comprend également un caractère "i sans point" minuscule, "ı" \(\\u0131\), dont la majuscule est "I". Ce comportement se produit également dans la culture azérie \("az"\).  
  
 Par conséquent, les suppositions faites sur la mise en majuscule du "i" ou de la mise en minuscule du "I" ne sont pas valides dans toutes les cultures. Si vous utilisez les surcharges par défaut pour les routines de comparaison de chaînes, elles varieront suivant les cultures. Si les données à comparer sont non linguistiques, l'utilisation de surcharges par défaut peut produire des résultats indésirables, comme le montre la tentative suivante d'exécution d'une comparaison ne respectant pas la casse des chaînes "file" et "FILE".  
  
 [!code-csharp[Conceptual.Strings.BestPractices#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#11)]
 [!code-vb[Conceptual.Strings.BestPractices#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#11)]  
  
 Cette comparaison peut provoquer de sérieux problèmes si la culture est utilisée par inadvertance dans des paramètres de sécurité sensibles, comme dans l'exemple suivant. Un appel de méthode comme `IsFileURI("file:")` retourne `true` si la culture actuelle est Anglais \(États\-Unis\), mais `false` si la culture actuelle est Turc. Par conséquent, sur les systèmes turcs, quelqu'un pourrait contourner les mesures de sécurité qui bloquent l'accès aux URI ne respectant pas la casse qui commencent par "FILE:".  
  
 [!code-csharp[Conceptual.Strings.BestPractices#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#12)]
 [!code-vb[Conceptual.Strings.BestPractices#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#12)]  
  
 Dans ce cas, étant donné que "file:" doit être interprété comme un identificateur non linguistique indépendant de la culture, le code doit plutôt être écrit comme indiqué dans l'exemple suivant.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/turkish1.cs#13)]
 [!code-vb[Conceptual.Strings.BestPractices#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/turkish1.vb#13)]  
  
### Opérations de chaînes ordinales  
 La spécification de la valeur <xref:System.StringComparison?displayProperty=fullName> ou <xref:System.StringComparison?displayProperty=fullName> dans un appel de méthode correspond à une comparaison non linguistique dans laquelle les fonctionnalités de langages naturels sont ignorées. Les méthodes qui sont appelées avec ces valeurs <xref:System.StringComparison> font reposer les décisions d'opération de chaîne sur de simples comparaisons d'octets plutôt que sur la casse ou des tables d'équivalences paramétrables par la culture. Dans la plupart des cas, cette approche correspond le mieux à l'interprétation de chaînes prévue tout en rendant le code plus rapide et plus fiable.  
  
 Les comparaisons ordinales sont des comparaisons de chaînes dans lesquelles chaque octet de chaque chaîne est comparé sans interprétation linguistique ; par exemple, "windows" ne correspond pas à "Windows". Il s’agit fondamentalement d’un appel à la fonction `strcmp` du runtime C. Utilisez cette comparaison quand le contexte indique que les chaînes doivent correspondre exactement ou demande une stratégie de correspondance classique. En outre, la comparaison ordinale est l'opération de comparaison la plus rapide, car elle n'applique aucune règle linguistique lors de la détermination d'un résultat.  
  
 Dans le .NET Framework, les chaînes peuvent contenir des caractères Null incorporés. L'une des différences les plus évidentes entre la comparaison ordinale et la comparaison dépendante de la culture \(y compris les comparaisons qui utilisent la culture dite indifférente\) concerne la gestion des caractères Null incorporés dans une chaîne. Ces caractères sont ignorés quand vous utilisez les méthodes <xref:System.String.Compare%2A?displayProperty=fullName> et <xref:System.String.Equals%2A?displayProperty=fullName> pour effectuer des comparaisons dépendantes de la culture \(notamment des comparaisons qui utilisent la culture dite indifférente\). Par conséquent, dans les comparaisons dépendantes de la culture, les chaînes qui contiennent des caractères Null incorporés peuvent être considérées comme égales à des chaînes qui n'en contiennent pas.  
  
> [!IMPORTANT]
>  Les méthodes de comparaison de chaînes ignorent les caractères Null incorporés, contrairement aux méthodes de recherche de chaînes telles que <xref:System.String.Contains%2A?displayProperty=fullName>, <xref:System.String.EndsWith%2A?displayProperty=fullName>, <xref:System.String.IndexOf%2A?displayProperty=fullName>, <xref:System.String.LastIndexOf%2A?displayProperty=fullName> et <xref:System.String.StartsWith%2A?displayProperty=fullName>.  
  
 L'exemple suivant effectue une comparaison dépendante de la culture de la chaîne "Aa" avec une chaîne semblable qui contient plusieurs caractères Null incorporés entre "A" et "a", et montre comment les deux chaînes sont considérées comme égales.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/embeddednulls1.cs#19)]
 [!code-vb[Conceptual.Strings.BestPractices#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/embeddednulls1.vb#19)]  
  
 Toutefois, les chaînes ne sont pas considérées comme égales quand vous utilisez la comparaison ordinale, comme le montre l'exemple suivant.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/embeddednulls2.cs#20)]
 [!code-vb[Conceptual.Strings.BestPractices#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/embeddednulls2.vb#20)]  
  
 Les comparaisons ordinales ne respectant pas la casse sont l'approche la plus conservatrice suivante. Ces comparaisons ignorent la plus grande partie de la casse ; par exemple, "windows" correspond à "Windows". Lors du traitement de caractères ASCII, cette stratégie est équivalente à <xref:System.StringComparison?displayProperty=fullName>, excepté qu'elle ignore la casse ASCII habituelle. Par conséquent, tout caractère de la plage \[A, Z\] \(\\u0041\-\\u005A\) correspond au caractère correspondant de la plage \[a,z\] \(\\u0061\-\\007A\). La casse hors de la plage ASCII utilise les tables de la culture dite indifférente. Par conséquent, la comparaison suivante :  
  
 [!code-csharp[Conceptual.Strings.BestPractices#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison2.cs#4)]
 [!code-vb[Conceptual.Strings.BestPractices#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison2.vb#4)]  
  
 est équivalente à la comparaison suivante \(mais plus rapide\) :  
  
 [!code-csharp[Conceptual.Strings.BestPractices#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison2.cs#5)]
 [!code-vb[Conceptual.Strings.BestPractices#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison2.vb#5)]  
  
 Ces comparaisons restent très rapides.  
  
> [!NOTE]
>  <xref:System.StringComparison?displayProperty=fullName> constitue la meilleure représentation du comportement de chaîne du système de fichiers, des clés de Registre et des valeurs, ainsi que des variables d'environnement.  
  
 <xref:System.StringComparison?displayProperty=fullName> et <xref:System.StringComparison?displayProperty=fullName> utilisent tous les deux directement les valeurs binaires et sont les plus adaptés à la mise en correspondance. Quand vous n'êtes pas sûr de vos paramètres de comparaison, utilisez l'une de ces deux valeurs. Toutefois, étant donné qu'elles effectuent une comparaison octet par octet, elles n'effectuent pas le tri selon un ordre de tri linguistique \(comme un dictionnaire français\), mais selon un ordre de tri binaire. Les résultats peuvent sembler étranges dans la plupart des contextes s'ils sont affichés aux utilisateurs.  
  
 La sémantique ordinale est la valeur par défaut pour les surcharges de <xref:System.String.Equals%2A?displayProperty=fullName> qui n'incluent pas d'argument <xref:System.StringComparison> \(notamment l'opérateur d'égalité\). Dans tous les cas, nous vous recommandons d'appeler une surcharge ayant un paramètre <xref:System.StringComparison>.  
  
### Opérations de chaîne qui utilisent la culture dite indifférente  
 Les comparaisons avec la culture dite indifférente utilisent la propriété <xref:System.Globalization.CultureInfo.CompareInfo%2A> retournée par la propriété <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=fullName> statique. Ce comportement est le même sur tous les systèmes ; il traduit tous les caractères en dehors de sa plage en ce qu'il suppose être des caractères invariants équivalents. Cette stratégie peut être utile pour la gestion d'un jeu de comportements de chaîne dans toutes les cultures, mais elle donne souvent des résultats inattendus.  
  
 Les comparaisons ne respectant pas la casse avec la culture dite indifférente utilisent également la propriété <xref:System.Globalization.CultureInfo.CompareInfo%2A> statique retournée également par la propriété <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=fullName> statique pour les informations de comparaison. Toutes les différences de casse entre ces caractères traduits sont ignorées.  
  
 Les comparaisons qui utilisent <xref:System.StringComparison?displayProperty=fullName> et <xref:System.StringComparison?displayProperty=fullName> fonctionnent de la même manière sur les chaînes ASCII. Toutefois, <xref:System.StringComparison?displayProperty=fullName> prend des décisions linguistiques qui peuvent ne pas être appropriées pour les chaînes qui doivent être interprétées comme un jeu d'octets. L’objet `CultureInfo.InvariantCulture.CompareInfo` entraîne l’interprétation par la méthode <xref:System.String.Compare%2A> de certains jeux de caractères comme étant équivalents. Par exemple, l'équivalence suivante est valide dans la culture dite indifférente :  
  
 InvariantCulture: a \+ ̊ \= å  
  
 Le caractère LETTRE MINUSCULE LATINE A "a" \(\\u0061\), quand il se trouve à côté du caractère DIACRITIQUE ROND EN CHEF "\+ " ̊" \(\\u030a\), est interprété comme le caractère LETTRE MINUSCULE LATINE A AVEC DIACRITIQUE ROND EN CHEF "å" \(\\u00e5\). Comme le montre l'exemple suivant, ce comportement diffère de la comparaison ordinale.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/comparison3.cs#15)]
 [!code-vb[Conceptual.Strings.BestPractices#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/comparison3.vb#15)]  
  
 Lors de l'interprétation de noms de fichiers, de cookies ou de tout autre élément dans lequel peut s'afficher une combinaison telle que "å", les comparaisons ordinales offrent toujours le comportement le plus transparent et le plus approprié.  
  
 En définitive, la culture dite indifférente a très peu de propriétés qui la rendent utile pour la comparaison. Elle effectue la comparaison d'une manière linguistiquement pertinente, qui l'empêche de garantir une équivalence symbolique complète, mais elle ne constitue pas le choix approprié pour l'affichage dans n'importe quelle culture. L'une des rares raisons justifiant l'utilisation de <xref:System.StringComparison?displayProperty=fullName> pour la comparaison est de rendre persistantes des données classées pour un affichage identique dans toutes les cultures. Par exemple, si un fichier de données volumineux qui contient une liste d'identificateurs triés à des fins d'affichage accompagne une application, un ajout à cette liste nécessiterait une insertion avec un tri de style invariant.  
  
 [Retour au début](#top)  
  
<a name="choosing_a_stringcomparison_member_for_your_method_call"></a>   
## Choix d'un membre StringComparison pour votre appel de méthode  
 Le tableau suivant décrit le mappage de contexte de chaîne sémantique à un membre d'énumération <xref:System.StringComparison>.  
  
|Données|Comportement|Valeur System.StringComparison<br /><br /> valeur|  
|-------------|------------------|-----------------------------------------------|  
|Identificateurs internes respectant la casse.<br /><br /> Identificateurs respectant la casse dans des normes telles que XML et HTTP.<br /><br /> Paramètres liés à la sécurité respectant la casse.|Identificateur non linguistique, où les octets correspondent exactement.|<xref:System.StringComparison>|  
|Identificateurs internes ne respectant pas la casse.<br /><br /> Identificateurs ne respectant pas la casse dans des normes telles que XML et HTTP.<br /><br /> Chemins d'accès aux fichiers.<br /><br /> Clés et valeurs de Registre.<br /><br /> Variables d'environnement.<br /><br /> Identificateurs de ressource \(par exemple, noms de handles\).<br /><br /> Paramètres liés à la sécurité ne respectant pas la casse.|Identificateur non linguistique, où la casse n'est pas pertinente ; en particulier, les données stockées dans la plupart des services système Windows.|<xref:System.StringComparison>|  
|Certaines données rendues persistantes et linguistiquement pertinentes.<br /><br /> Affichage de données linguistiques qui nécessitent un ordre de tri fixe.|Données dont la culture n'est pas spécifiée qui sont toutefois linguistiquement pertinentes.|<xref:System.StringComparison><br /><br /> ou<br /><br /> <xref:System.StringComparison>|  
|Données affichées à l'utilisateur.<br /><br /> La plupart des entrées d'utilisateur.|Données qui nécessitent des usages linguistiques locaux.|<xref:System.StringComparison><br /><br /> ou<br /><br /> <xref:System.StringComparison>|  
  
 [Retour au début](#top)  
  
<a name="common_string_comparison_methods_in_the_net_framework"></a>   
## Méthodes courantes de comparaison de chaînes dans le .NET Framework  
 Les sections suivantes décrivent les méthodes le plus souvent utilisées pour la comparaison de chaînes.  
  
### String.Compare  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 En tant qu'opération la plus centrale de l'interprétation de chaînes, toutes les instances de ces appels de méthode doivent être examinées pour déterminer si les chaînes doivent être interprétées d'après la culture actuelle ou être dissociées de la culture \(symboliquement\). L'opération appropriée est, en général, la dernière, et une comparaison <xref:System.StringComparison?displayProperty=fullName> doit être utilisée à la place.  
  
 La classe <xref:System.Globalization.CompareInfo?displayProperty=fullName>, retournée par la propriété <xref:System.Globalization.CultureInfo.CompareInfo%2A?displayProperty=fullName>, inclut également une méthode <xref:System.Globalization.CompareInfo.Compare%2A> qui fournit un grand nombre d'options de correspondance \(ordinale, ignorance des espaces blancs, ignorance du type Kana, etc.\) au moyen de l'énumération d'indicateur <xref:System.Globalization.CompareOptions>.  
  
### String.CompareTo  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 Cette méthode n'offre actuellement pas de surcharge spécifiant un type <xref:System.StringComparison>. Il est généralement possible de convertir cette méthode dans la forme <xref:System.String.Compare%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=fullName> recommandée.  
  
 Les types qui implémentent les interfaces <xref:System.IComparable> et <xref:System.IComparable%601> implémentent cette méthode. Étant donné qu'elle n'offre pas la possibilité d'un paramètre <xref:System.StringComparison>, les types d'implémentation permettent souvent à l'utilisateur de spécifier un <xref:System.StringComparer> dans leur constructeur. L'exemple suivant définit une classe `FileName` dont le constructeur de classe inclut un paramètre <xref:System.StringComparer>. Cet objet <xref:System.StringComparer> est ensuite utilisé dans la méthode `FileName.CompareTo`.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/api1.cs#6)]
 [!code-vb[Conceptual.Strings.BestPractices#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/api1.vb#6)]  
  
### String.Equals  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 La classe <xref:System.String> vous permet de tester l'égalité en appelant les surcharges de méthode <xref:System.String.Equals%2A> statique ou d'instance, ou en utilisant l'opérateur d'égalité statique. Par défaut, les surcharges et l'opérateur utilisent la comparaison ordinale. Toutefois, nous vous recommandons quand même d'appeler une surcharge qui spécifie explicitement le type <xref:System.StringComparison>, même si vous voulez effectuer une comparaison ordinale ; cela le simplifie la recherche d'une interprétation de chaîne particulière dans du code.  
  
### String.ToUpper et String.ToLower  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 Vous devez faire preuve de prudence quand vous utilisez ces méthodes, car imposer une majuscule ou une minuscule dans une chaîne est souvent utilisé comme une petite normalisation pour la comparaison de chaînes indépendamment de la casse. Si tel est le cas, vous devez envisager d'utiliser une comparaison ne respectant pas la casse.  
  
 Les méthodes <xref:System.String.ToUpperInvariant%2A?displayProperty=fullName> et <xref:System.String.ToLowerInvariant%2A?displayProperty=fullName> sont également disponibles.<xref:System.String.ToUpperInvariant%2A> est le moyen standard de normaliser la casse. Les comparaisons faites à l'aide de <xref:System.StringComparison?displayProperty=fullName> sont, sur le plan comportemental, la composition de deux appels : appel à <xref:System.String.ToUpperInvariant%2A> sur les deux arguments de chaîne, et exécution d'une comparaison à l'aide de <xref:System.StringComparison?displayProperty=fullName>.  
  
 Des surcharges sont également disponibles pour la conversion en majuscules et en minuscules dans une culture spécifique, en passant à la méthode un objet <xref:System.Globalization.CultureInfo> qui représente cette culture.  
  
### Char.ToUpper et Char.ToLower  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 Ces méthodes fonctionnent de la même façon que les méthodes <xref:System.String.ToUpper%2A?displayProperty=fullName> et <xref:System.String.ToLower%2A?displayProperty=fullName> décrites dans la section précédente.  
  
### String.StartsWith et String.EndsWith  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 Par défaut, ces deux méthodes effectuent une comparaison dépendante de la culture.  
  
### String.IndexOf et String.LastIndexOf  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 La façon dont les surcharges par défaut de ces méthodes effectuent les comparaisons n'est pas cohérente. Toutes les méthodes <xref:System.String.IndexOf%2A?displayProperty=fullName> et <xref:System.String.LastIndexOf%2A?displayProperty=fullName> qui incluent un paramètre <xref:System.Char> effectuent une comparaison ordinale, mais les méthodes <xref:System.String.IndexOf%2A?displayProperty=fullName> et <xref:System.String.LastIndexOf%2A?displayProperty=fullName> par défaut qui incluent un paramètre <xref:System.String> effectuent une comparaison dépendante de la culture.  
  
 Si vous appelez la méthode <xref:System.String.IndexOf%28System.String%29?displayProperty=fullName> ou <xref:System.String.LastIndexOf%28System.String%29?displayProperty=fullName> et que vous lui passez une chaîne à localiser dans l'instance actuelle, nous vous recommandons d'appeler une surcharge qui spécifie explicitement le type <xref:System.StringComparison>. Les surcharges qui incluent un argument <xref:System.Char> ne vous permettent pas de spécifier un type <xref:System.StringComparison>.  
  
 [Retour au début](#top)  
  
<a name="methods_that_perform_string_comparison_indirectly"></a>   
## Méthodes qui effectuent indirectement la comparaison de chaînes  
 Certaines méthodes autres que les méthodes de chaîne dont l'opération centrale est la comparaison de chaînes utilisent le type <xref:System.StringComparer>. La classe <xref:System.StringComparer> inclut six propriétés statiques qui retournent des instances de <xref:System.StringComparer> dont les méthodes <xref:System.StringComparer.Compare%2A?displayProperty=fullName> effectuent les types de comparaisons de chaînes suivants :  
  
-   Comparaisons de chaînes dépendantes de la culture à l'aide de la culture actuelle. Cet objet <xref:System.StringComparer> est retourné par la propriété <xref:System.StringComparer.CurrentCulture%2A?displayProperty=fullName>.  
  
-   Comparaisons ne respectant pas la casse à l'aide de la culture actuelle. Cet objet <xref:System.StringComparer> est retourné par la propriété <xref:System.StringComparer.CurrentCultureIgnoreCase%2A?displayProperty=fullName>.  
  
-   Comparaisons indépendantes de la culture à l'aide des règles de comparaison de mots de la culture dite indifférente. Cet objet <xref:System.StringComparer> est retourné par la propriété <xref:System.StringComparer.InvariantCulture%2A?displayProperty=fullName>.  
  
-   Comparaisons ne respectant pas la casse et indépendantes de la culture à l'aide des règles de comparaison des mots de la culture dite indifférente. Cet objet <xref:System.StringComparer> est retourné par la propriété <xref:System.StringComparer.InvariantCultureIgnoreCase%2A?displayProperty=fullName>.  
  
-   Comparaison ordinale. Cet objet <xref:System.StringComparer> est retourné par la propriété <xref:System.StringComparer.Ordinal%2A?displayProperty=fullName>.  
  
-   Comparaison ordinale ne respectant pas la casse. Cet objet <xref:System.StringComparer> est retourné par la propriété <xref:System.StringComparer.OrdinalIgnoreCase%2A?displayProperty=fullName>.  
  
### Array.Sort et Array.BinarySearch  
 Interprétation par défaut : <xref:System.StringComparison?displayProperty=fullName>.  
  
 Quand vous stockez des données dans une collection ou quand vous lisez des données persistantes à partir d'un fichier ou d'une base de données dans une collection, le changement de culture actuelle peut invalider les invariants de la collection. La méthode <xref:System.Array.BinarySearch%2A?displayProperty=fullName> suppose que les éléments du tableau dans lequel effectuer la recherche sont déjà triés. Pour trier tout élément de chaîne dans le tableau, la méthode <xref:System.Array.Sort%2A?displayProperty=fullName> appelle la méthode <xref:System.String.Compare%2A?displayProperty=fullName> pour classer les éléments individuels. L'utilisation d'un comparateur dépendant de la culture peut s'avérer dangereux si la culture change entre le moment où le tableau est trié et le moment où son contenu fait l'objet d'une recherche. Par exemple, dans le code suivant, le stockage et la récupération fonctionnent sur le comparateur fourni implicitement par la propriété `Thread.CurrentThread.CurrentCulture`. Si la culture peut changer entre les appels à `StoreNames` et `DoesNameExist`, et surtout si le contenu du tableau est rendu persistant à un moment donné entre les deux appels de méthode, la recherche binaire peut échouer.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#7)]
 [!code-vb[Conceptual.Strings.BestPractices#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#7)]  
  
 L'exemple suivant, qui utilise la même méthode de comparaison ordinale \(indépendante de la culture\) pour trier le tableau et pour effectuer une recherche, présente une variation recommandée. Le code de modification est reflété dans les lignes libellées `Line A` et `Line B` dans les deux exemples.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#8)]
 [!code-vb[Conceptual.Strings.BestPractices#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#8)]  
  
 Si ces données sont rendues persistantes et déplacées dans toutes les cultures, et que le tri est utilisé pour présenter ces données à l'utilisateur, vous pouvez envisager d'utiliser <xref:System.StringComparison?displayProperty=fullName>, qui fonctionne linguistiquement pour une meilleure sortie d'utilisateur mais n'est pas affecté par les modifications apportées à la culture. L'exemple suivant modifie les deux exemples précédents pour utiliser la culture dite indifférente pour le tri du tableau et la recherche dans celui\-ci.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect1.cs#9)]
 [!code-vb[Conceptual.Strings.BestPractices#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect1.vb#9)]  
  
### Exemple de collections : constructeur Hashtable  
 Le hachage de chaînes constitue un deuxième exemple d'opération qui est affectée par la façon dont des chaînes sont comparées.  
  
 L'exemple suivant instancie un objet <xref:System.Collections.Hashtable> en lui passant l'objet <xref:System.StringComparer> qui est retourné par la propriété <xref:System.StringComparer.OrdinalIgnoreCase%2A?displayProperty=fullName>. Étant donné qu'une classe <xref:System.StringComparer> dérivée de <xref:System.StringComparer> implémente l'interface <xref:System.Collections.IEqualityComparer>, sa méthode <xref:System.Collections.IEqualityComparer.GetHashCode%2A> est utilisée pour calculer le code de hachage de chaînes dans la table de hachage.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/indirect2.cs#10)]
 [!code-vb[Conceptual.Strings.BestPractices#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/indirect2.vb#10)]  
  
 [Retour au début](#top)  
  
<a name="Formatted"></a>   
## Affichage et persistance des données mises en forme  
 Lorsque vous affichez des données non\-chaînées telles que les nombres et les dates et heures aux utilisateurs, mettez\-les en forme en utilisant les paramètres de la culture de l'utilisateur. Par défaut, la méthode <xref:System.String.Format%2A?displayProperty=fullName> et les méthodes `ToString` des types numériques et des types de date et d’heure, utilisent la culture du thread actuelle pour les opérations de mise en forme. Pour spécifier explicitement que la méthode de mise en forme doit utiliser la culture actuelle, vous pouvez appeler une surcharge d'une méthode de mise en forme ayant un paramètre `provider`, comme [String.Format\(IFormatProvider, String, Object\<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=fullName> ou <xref:System.DateTime.ToString%28System.IFormatProvider%29?displayProperty=fullName>, et lui passer la propriété <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=fullName>.  
  
 Vous pouvez rendre persistantes des données non\-chaînées soit comme données binaires, soit comme données mises en forme. Si vous choisissez de l'enregistrer en tant que données mises en forme, vous devez appeler une surcharge de méthode de mise en forme qui inclut un paramètre `provider` et le passer à la propriété <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=fullName>. La culture dite indifférente fournit un format cohérent pour les données mises en forme qui est indépendant de la culture et de l'ordinateur. En revanche, assurer la persistance de données mises en forme à l'aide de cultures autres que la culture dite indifférente a plusieurs limites :  
  
-   Les données seront vraisemblablement inutilisables si elles sont récupérées sur un système ayant une autre culture, ou si l'utilisateur du système actuel change la culture actuelle et essaie de récupérer les données.  
  
-   Les propriétés d'une culture sur un ordinateur spécifique peuvent différer des valeurs standard. À tout moment, un utilisateur peut personnaliser les paramètres d'affichage selon la culture. De ce fait, les données mises en forme sont stockées sur un système et peuvent ne pas être lisibles lorsque l'utilisateur personnalise les paramètres de culture. La portabilité des données mises en forme sur différents ordinateurs peut être encore plus limitée.  
  
-   Des normes internationales, régionales ou nationales qui régissent la mise en forme des nombres ou des dates et heures évoluent au fil du temps, et ces modifications sont intégrées dans les mises à jour du système d'exploitation Windows. Quand les conventions de mise en forme changent, les données qui ont été mises en forme en utilisant les conventions antérieures peuvent devenir illisibles.  
  
 L'exemple suivant illustre la portabilité limitée qui résulte de l'utilisation de la mise en forme qui tient compte de la culture pour assurer la persistance des données. L'exemple enregistre un tableau de valeurs de date et d'heure dans un fichier. Celles\-ci sont mises en forme en utilisant les conventions culturelles de l'anglais \(États\-Unis\). Une fois que l'application change la culture du thread actuel pour appliquer le français \(Suisse\), elle essaie de lire les valeurs enregistrées en utilisant les conventions de mise en forme de la culture actuelle. La tentative de lecture des éléments de données par deux fois renvoie une exception <xref:System.FormatException>, et le tableau de dates contient maintenant deux éléments incorrects qui sont identiques à <xref:System.DateTime.MinValue>.  
  
 [!code-csharp[Conceptual.Strings.BestPractices#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.bestpractices/cs/persistence.cs#21)]
 [!code-vb[Conceptual.Strings.BestPractices#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.bestpractices/vb/persistence.vb#21)]  
  
 Toutefois, si vous remplacez la propriété <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=fullName> par <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=fullName> dans les appels à <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=fullName> et <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%29?displayProperty=fullName>, les données de date et d'heure persistantes sont correctement restaurées, comme le montre la sortie suivante.  
  
```  
  
06.05.1758 21:26  
05.05.1818 07:19  
22.04.1870 23:54  
08.09.1890 06:47  
18.02.1905 15:12  
  
```  
  
## Voir aussi  
 [Manipulation des chaînes](../../../docs/standard/base-types/manipulating-strings.md)