---
title: "Création et utilisation d'assemblys avec nom fort"
ms.date: 08/01/2017
ms.prod: .net-framework
ms.technology:
- dotnet-bcl
ms.topic: article
helpviewer_keywords:
- strong-name bypass feature
- strong-named assemblies, about strong-named assemblies
- strong-named assemblies
- signing assemblies
- assemblies [.NET Framework], signing
- strong-named assemblies, scenarios
- assemblies [.NET Framework], strong-named
- strong-named assemblies, loading into trusted application domains
- assembly binding, strong-named
ms.assetid: ffbf6d9e-4a88-4a8a-9645-4ce0ee1ee5f9
caps.latest.revision: 17
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 0ee5fed355e0d8418500f1ecee53019548d9f7f8
ms.openlocfilehash: 39fbd38549a791a761c633dca90dbdeeeefce10b
ms.contentlocale: fr-fr
ms.lasthandoff: 08/02/2017

---
# <a name="creating-and-using-strong-named-assemblies"></a>Création et utilisation d'assemblys avec nom fort
<a name="top"></a>Un nom fort est constitué de l’identité de l’assembly (son simple nom textuel, son numéro de version et des informations de culture, le cas échéant) ainsi que d’une clé publique et d’une signature numérique. Il est généré à partir d'un fichier d'assembly à l'aide de la clé privée correspondante. (Le fichier d'assembly contient le manifeste d'assembly, qui contient les noms et les hachages de tous les fichiers composant l'assembly.)  
  
 Un assembly avec nom fort peut uniquement utiliser les types d'autres assemblys avec nom fort. Si ce n'était pas le cas, l’intégrité de l’assembly avec nom fort serait compromise.  
  
 Cette vue d'ensemble contient les sections suivantes :  
  
-   [Scénario de nom fort](#strong_name_scenario)  
  
-   [Ignorer la vérification de signature des assemblys approuvés](#bypassing_signature_verification)  
  
-   [Rubriques connexes](#related_topics)  
  
<a name="strong_name_scenario"></a>   
## <a name="strong-name-scenario"></a>Scénario de nom fort  
 Le scénario suivant met en avant le processus de signature d'un assembly avec nom fort, puis son référencement par ce nom.  
  
1.  L'assembly A est créé avec un nom fort à l'aide de l'une des méthodes suivantes :  
  
    -   En utilisant un environnement de développement qui prend en charge la création de noms forts, tel que [!INCLUDE[vsprvslong](../../../includes/vsprvslong-md.md)].  
  
    -   En créant une paire de clés de chiffrement avec l’[outil Strong Name (Sn.exe)](../../../docs/framework/tools/sn-exe-strong-name-tool.md) et en l’assignant à l’assembly à l’aide d’un compilateur de ligne de commande ou de [Assembly Linker (Al.exe)](../../../docs/framework/tools/al-exe-assembly-linker.md). Le Kit de développement logiciel (SDK) Windows fournit Sn.exe et Al.exe.  
  
2.  L'environnement de développement ou l'outil signe le hachage du fichier contenant le manifeste de l'assembly avec la clé privée du développeur. Cette signature numérique est stockée dans le fichier exécutable portable (PE) qui contient le manifeste de l'assembly A.  
  
3.  L’assembly B est un consommateur de l’Assembly A. La section de référence du manifeste de l’assembly B inclut un jeton qui représente la clé publique de l’assembly A. Un jeton est une partie de la clé publique complète. Il est utilisé à la place de la clé pour économiser de l'espace.  
  
4.  Le Common Language Runtime vérifie la signature de nom fort quand l'assembly est placé dans le Global Assembly Cache. Lors de la liaison par nom fort au moment de l’exécution, le common language runtime compare la clé stockée dans le manifeste de l’Assembly B avec la clé utilisée pour générer le nom fort de l’Assembly A. Si les vérifications de la sécurité .NET Framework et la liaison réussissent, l’Assembly B a une garantie que les bits de l’Assembly A n’ont pas été falsifiés et qu’ils proviennent réellement des développeurs de l’Assembly A.  
  
> [!NOTE]
>  Ce scénario ne traite pas des problèmes de confiance. Outre un nom fort, les assemblys peuvent posséder des signatures Microsoft Authenticode complètes. Les signatures Authenticode incluent un certificat établissant la confiance. Il est important de noter que les noms forts ne nécessitent pas que le code soit signé de cette manière. Les noms forts fournissent seulement une identité unique.  
  
 [Retour au début](#top)  
  
<a name="bypassing_signature_verification"></a>   
## <a name="bypassing-signature-verification-of-trusted-assemblies"></a>Ignorer la vérification de signature des assemblys approuvés  
 À partir du [!INCLUDE[net_v35SP1_long](../../../includes/net-v35sp1-long-md.md)], les signatures de nom fort ne sont pas validées quand un assembly est chargé dans un domaine d'application de confiance totale tel que le domaine de l'application par défaut pour la zone `MyComputer`. Cette fonctionnalité permet d'ignorer les noms forts. Dans un environnement de confiance totale, les demandes de <xref:System.Security.Permissions.StrongNameIdentityPermission> aboutissent toujours pour les assemblys de confiance totale signés, quelle que soit leur signature. La fonctionnalité consistant à ignorer les noms forts évite les traitements inutiles liés à la vérification de signature de nom fort des assemblys de confiance totale dans cette situation, ce qui permet un chargement plus rapide des assemblys.  
  
 Cette fonctionnalité s'applique à tout assembly signé avec un nom fort qui présente les caractéristiques suivantes :  
  
-   Confiance totale sans preuve <xref:System.Security.Policy.StrongName> (par exemple, dispose de la preuve de zone `MyComputer`).  
  
-   Chargé dans un <xref:System.AppDomain> de confiance totale.  
  
-   Chargé à partir d'un emplacement sous la propriété <xref:System.AppDomainSetup.ApplicationBase%2A> de cet <xref:System.AppDomain>.  
  
-   Sans signature différée.  
  
 Cette fonctionnalité peut être désactivée pour des applications individuelles ou pour un ordinateur. Consultez [Guide pratique pour désactiver la fonctionnalité consistant à ignorer les noms forts](../../../docs/framework/app-domains/how-to-disable-the-strong-name-bypass-feature.md).  
  
 [Retour au début](#top)  
  
<a name="related_topics"></a>   
## <a name="related-topics"></a>Rubriques connexes  
  
|Titre|Description|  
|-----------|-----------------|  
|[Guide pratique pour créer une paire de clés publique/privée](../../../docs/framework/app-domains/how-to-create-a-public-private-key-pair.md)|Décrit comment créer une paire de clés de chiffrement pour signer un assembly.|  
|[Comment : signer un assembly avec un nom fort](../../../docs/framework/app-domains/how-to-sign-an-assembly-with-a-strong-name.md)|Décrit comment créer un assembly avec nom fort.|  
|[Utilisation de noms forts améliorés](../../../docs/framework/app-domains/enhanced-strong-naming.md)|Décrit les améliorations apportées aux noms forts dans le [!INCLUDE[net_v45](../../../includes/net-v45-md.md)].|  
|[Guide pratique pour référencer un assembly avec un nom fort](../../../docs/framework/app-domains/how-to-reference-a-strong-named-assembly.md)|Décrit comment référencer des types ou des ressources dans un assembly avec nom fort au moment de la compilation ou de l'exécution.|  
|[Guide pratique pour désactiver la fonctionnalité consistant à ignorer les noms forts](../../../docs/framework/app-domains/how-to-disable-the-strong-name-bypass-feature.md)|Décrit comment désactiver la fonctionnalité qui ignore la validation des signatures avec nom fort. Cette fonctionnalité peut être désactivée pour toutes les applications ou pour des applications spécifiques.|  
|[Création d’assemblys](../../../docs/framework/app-domains/create-assemblies.md)|Fournit une vue d'ensemble des assemblys multifichiers et à fichier unique.|  
|[Comment : temporiser la signature d’un assembly (Visual Studio)](/visualstudio/ide/managing-assembly-and-manifest-signing#how-to-sign-an-assembly-in-visual-studio)|Explique comment signer un assembly avec un nom fort après la création de l'assembly.|  
|[Sn.exe (outil Strong Name)](../../../docs/framework/tools/sn-exe-strong-name-tool.md)|Décrit l'outil inclus dans le .NET Framework qui facilite la création d'assemblys avec des noms forts. Cet outil fournit des options de gestion des clés, de génération des signatures et de vérification des signatures.|  
|[Al.exe (Assembly Linker)](../../../docs/framework/tools/al-exe-assembly-linker.md)|Décrit l'outil inclus dans le .NET Framework qui génère un fichier possédant un manifeste d'assembly à partir de modules ou de fichiers de ressources.|

