---
title: "Caract&#232;res d&#39;&#233;chappement dans les expressions r&#233;guli&#232;res | Microsoft Docs"
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
  - ".NET Framework (expressions régulières), caractères d'échappement"
  - "caractères, échappement"
  - "constructions, caractères d'échappement"
  - "caractères d'échappement"
  - "expressions régulières, caractères d'échappement"
  - "modèles de remplacement"
  - "caractères sans séquence d'échappement"
ms.assetid: f49cc9cc-db7d-4058-8b8a-422bc08b29b0
caps.latest.revision: 31
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 29
---
# Caract&#232;res d&#39;&#233;chappement dans les expressions r&#233;guli&#232;res
La barre oblique inverse \(\\\) dans une expression régulière indique une des possibilités suivantes :  
  
-   Le caractère qui la suit est un caractère spécial, comme indiqué dans le tableau de la section suivante.  Par exemple, `\b` est une ancre qui indique qu'une correspondance d'expression régulière doit commencer sur une limite de mot, `\t` représente une tabulation et `\x020` représente un espace.  
  
-   Un caractère qui doit être interprété littéralement, et qui sans cela serait interprété comme une construction du langage sans séquence d'échappement.  Par exemple, une accolade \(`{`\) commence la définition d'un quantificateur, mais une barre oblique inverse suivie d'une accolade \(`\{`\) indique que le moteur d'expressions régulières doit rechercher une correspondance avec l'accolade.  De la même façon, une seule barre oblique inverse marque le début d'une construction du langage avec échappement, mais deux barres obliques inverses \(`\\`\) indiquent que le moteur d'expressions régulières doit chercher une correspondance avec la barre oblique inverse.  
  
> [!NOTE]
>  Les séquences d'échappement des caractères sont reconnues dans les modèles d'expressions régulières, mais pas dans les modèles de remplacement.  
  
## Séquences d'échappement des caractères dans le .NET Framework  
 Le tableau suivant répertorie les séquences d'échappement des caractères prises en charge par les expressions régulières dans le .NET Framework.  
  
|Caractère ou séquence|Description|  
|---------------------------|-----------------|  
|Tous les caractères à l'exception des suivants :<br /><br /> .  $ ^ { \[ \( &#124; \) \* \+ ?  \\|Les caractères autres que ceux répertoriés dans la colonne **Caractère ou séquence** n'ont pas signification spéciale dans les expressions régulières ; ils ne correspondent qu'à eux\-mêmes.<br /><br /> Les caractères inclus dans la colonne **Caractère ou séquence** sont des éléments spéciaux du langage des expressions régulières.  Pour les faire correspondre dans une expression régulière, ils doivent être utilisés avec un caractère d'échappement ou inclus dans un [groupe de caractères positif](../../../docs/standard/base-types/character-classes-in-regular-expressions.md).  Par exemple, l'expression régulière `\$\d+` ou `[$]\d+` est en correspondance avec "$1200".|  
|`\a`|Correspond à un caractère représentant une cloche \(alarme\), `\u0007`.|  
|`\b`|Dans une classe de caractères `[`*groupe\_caractères*`]`, correspond à un retour arrière, `\u0008`.  \(Voir [Classes de caractères](../../../docs/standard/base-types/character-classes-in-regular-expressions.md).\) En dehors d'une classe de caractères, `\b` est une ancre qui correspond à une limite de mot.  \(Voir [Ancres](../../../docs/standard/base-types/anchors-in-regular-expressions.md).\)|  
|`\t`|Correspond à une tabulation, `\u0009`.|  
|`\r`|Correspond à un retour chariot, `\u000D`.  Notez que `\r` n'est pas équivalent au caractère de nouvelle ligne, `\n`.|  
|`\v`|Correspond à une tabulation verticale, `\u000B`.|  
|`\f`|Correspond à un saut de page, `\u000C`.|  
|`\n`|Correspond à une nouvelle ligne, `\u000A`.|  
|`\e`|Correspond à un caractère d'échappement, `\u001B`.|  
|`\` *nnn*|Correspond à un caractère ASCII, où *nnn* est constitué de deux ou trois chiffres qui représentent le code octal du caractère.  Par exemple, `\040` représente un espace.  Cette construction est interprétée comme une référence arrière si elle a un seul chiffre \(par exemple `\2`\) ou si elle correspond au nombre d'un groupe de capture.  \(Voir [Constructions de backreference](../../../docs/standard/base-types/backreference-constructs-in-regular-expressions.md).\)|  
|`\x` *nn*|Correspond à un caractère ASCII, où *nn* est un code hexadécimal à deux chiffres d'un caractère.|  
|`\c` *X*|Correspond à un caractère de contrôle ASCII, où X est la lettre du caractère de contrôle.  Par exemple, `\cC` est Ctrl\-C.|  
|`\u` *nnnn*|Correspond à une unité de code UTF\-16 dont la valeur est *nnnn* en hexadécimal. **Note:**  La séquence d'échappement des caractères de Perl 5 utilisée pour spécifier Unicode n'est pas prise en charge par le .NET Framework.  La séquence d'échappement des caractères de Perl 5 a la forme `\x{`*\#\#\#\#*`…}`, où *\#\#\#\#*`…` est une série de chiffres hexadécimaux.  Utilisez à la place `\u`*nnnn*.|  
|`\`|Quand ce caractère d'échappement est suivi d'un caractère non reconnu comme caractère d'échappement, correspond au caractère lui\-même.  Par exemple, `\*` correspond à un astérisque \(\*\) et est identique à `\x2A`.|  
  
## Exemple  
 L'exemple suivant montre l'utilisation de caractères d'échappement dans une expression régulière.  Il analyse une chaîne qui contient les noms des plus grandes villes du monde et leur population en 2009.  Chaque nom de ville est séparé de sa population par une tabulation \(`\t`\) ou par une barre verticale \(&#124; ou `\u007c`\).  Les villes individuelles et leur population sont séparées les unes des autres par un retour chariot et un saut de ligne.  
  
 [!code-csharp[RegularExpressions.Language.Escapes#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.escapes/cs/escape1.cs#1)]
 [!code-vb[RegularExpressions.Language.Escapes#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.escapes/vb/escape1.vb#1)]  
  
 L'expression régulière `\G(.+)[\t|\u007c](../../../amples/snippets/visualbasic/VS_Snippets_Wpf/DocumentStructure/visualbasic/spec_withstructure-xps/_rels/.rels)\r?\n` est interprétée comme indiqué dans le tableau suivant.  
  
|Modèle|Description|  
|------------|-----------------|  
|`\G`|Commencer la correspondance là où la dernière correspondance s'est terminée.|  
|`(.+)`|Faire correspondre à n'importe quel caractère une ou plusieurs fois.  Il s'agit du premier groupe de capture.|  
|`[\t\u007c]`|Faire correspondre à une tabulation \(`\t`\) ou à une barre verticale \(&#124;\).|  
|`(.+)`|Faire correspondre à n'importe quel caractère une ou plusieurs fois.  Il s'agit du deuxième groupe de capture.|  
|`\r?  \n`|Faire correspondre à zéro ou à une occurrence d'un retour chariot suivi d'une nouvelle ligne.|  
  
## Voir aussi  
 [Langage des expressions régulières \- Aide\-mémoire](../../../docs/standard/base-types/regular-expression-language-quick-reference.md)