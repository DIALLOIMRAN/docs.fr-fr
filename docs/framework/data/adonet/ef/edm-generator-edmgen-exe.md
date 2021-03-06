---
title: "EDM Generator (EdmGen.exe) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
ms.assetid: fe8297a1-1fc3-48ce-8eeb-f70f63f857aa
caps.latest.revision: 6
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 6
---
# EDM Generator (EdmGen.exe)
EdmGen.exe est un outil en ligne de commande utilisé avec le modèle [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)] et les fichiers de mappage.  Vous pouvez utiliser l'outil EdmGen.exe pour effectuer les opérations suivantes :  
  
-   Vous connecter à une source de données en utilisant un fournisseur de données .NET Framework spécifique à la source de données, et générer les fichiers de modèle conceptuel \(.csdl\), de modèle de stockage \(.ssdl\) et de mappage \(.msl\) utilisés par [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)].  Pour plus d'informations, consultez [Procédure : utiliser EdmGen.exe pour générer les fichiers de modèle et de mappage \(Entity Framework\)](../../../../../docs/framework/data/adonet/ef/how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md).  
  
-   Valider un modèle existant.  Pour plus d'informations, consultez [Procédure : utiliser EdmGen.exe pour valider les fichiers de modèle et de mappage \(Entity Framework\)](../../../../../docs/framework/data/adonet/ef/how-to-use-edmgen-exe-to-validate-model-and-mapping-files.md).  
  
-   Générer un fichier de code C\# ou Visual Basic qui contient les classes d'objets générées à partir d'un fichier de modèle conceptuel \(.csdl\).  Pour plus d'informations, consultez [Procédure : utiliser EdmGen.exe pour générer le code de couche objet](../../../../../docs/framework/data/adonet/ef/how-to-use-edmgen-exe-to-generate-object-layer-code.md).  
  
-   Générez un fichier de code C\# ou Visual Basic qui contient les vues prégénérées d'un modèle existant.  Pour plus d'informations, consultez [How to: Pre\-Generate Views to Improve Query Performance](http://msdn.microsoft.com/fr-fr/b18a9d16-e10b-4043-ba91-b632f85a2579).  
  
 L'outil EdmGen.exe est installé dans le répertoire du [!INCLUDE[dnprdnshort](../../../../../includes/dnprdnshort-md.md)].  Dans de nombreux cas, celui\-ci se trouve dans C:\\windows\\Microsoft.NET\\Framework\\v4.0.  Pour les systèmes 64 bits, il se trouve dans C:\\windows\\Microsoft.NET\\Framework64\\v4.0.  Vous pouvez également accéder à l'outil EdmGen.exe à partir de l'invite de commandes de Microsoft Visual Studio \(cliquez sur **Démarrer**, pointez sur **Tous les programmes**, sur **Microsoft Visual Studio 2010** et sur **Outils Visual Studio**, puis cliquez sur **Invite de commandes de Visual Studio 2010**\).  
  
## Syntaxe  
  
```  
EdmGen /mode:choice [options]  
```  
  
## Mode  
 Lorsque vous utilisez l'outil EdmGen.exe, vous devez spécifier l'un des modes suivants.  
  
|Mode|Description|  
|----------|-----------------|  
|`/mode:ValidateArtifacts`|Valide les fichiers .csdl, .ssdl et .msl, et affiche les erreurs ou les avertissements éventuels.<br /><br /> Cette option requiert au moins l'un des arguments `/inssdl` ou `/incsdl`.  Si `/inmsl` est spécifié, les arguments `/inssdl` et `/incsdl` sont également requis.|  
|`/mode:FullGeneration`|Utilise les informations de connexion à la base de données spécifiées dans l'option `/connectionstring` et génère des fichiers .csdl, .ssdl, .msl, de couche objet et de vue.<br /><br /> Cette option requiert un argument `/connectionstring`, et soit un argument `/project` soit les arguments `/outssdl`, `/outcsdl`, `/outmsdl`, `/outobjectlayer`, `/outviews`, `/namespace` et `/entitycontainer`.|  
|`/mode:FromSSDLGeneration`|Génère des fichiers .csdl et .msl, du code source et des vues à partir du fichier .ssdl spécifié.<br /><br /> Cette option requiert l'argument `/inssdl`, et soit un argument `/project` soit les arguments `/outcsdl`, `/outmsl`, `/outobjectlayer`, `/outviews`, `/namespace,` et `/entitycontainer`.|  
|`/mode:EntityClassGeneration`|Crée un fichier de code source qui contient les classes générées à partir du fichier .csdl.<br /><br /> Cette option requiert l'argument `/incsdl`, et soit l'argument `/project` soit l'argument `/outobjectlayer`.  L'argument `/language` est obligatoire.|  
|`/mode:ViewGeneration`|Crée un fichier de code source qui contient les vues générées à partir des fichiers .csdl, .ssdl, et .msl.<br /><br /> Cette option requiert les arguments `/inssdl`, `/incsdl`, `/inmsl,` et soit l'argument `/project` soit l'argument `/outviews`.  L'argument `/language` est obligatoire.|  
  
## Options  
  
|Option|Description|  
|------------|-----------------|  
|`/p[roject]:`\<chaîne\>|Spécifie le nom de projet à utiliser.  Le nom de projet est utilisé comme valeur par défaut pour le paramètre d'espace de noms, le nom du modèle et des fichiers de mappage, le nom du fichier source de l'objet et le nom de fichier source de génération de vues.  Le nom du conteneur d'entités a la valeur \<project\>Context.|  
|`/prov[ider]:`\<chaîne\>|Nom du fournisseur de données [!INCLUDE[dnprdnshort](../../../../../includes/dnprdnshort-md.md)] à utiliser pour générer le fichier de modèle de stockage \(.ssdl\).  Le fournisseur par défaut est le fournisseur de données [!INCLUDE[dnprdnshort](../../../../../includes/dnprdnshort-md.md)] pour SQL Server \(<xref:System.Data.SqlClient?displayProperty=fullName>\).|  
|`/c[onnectionstring]:`\<chaîne de connexion\>|Spécifie la chaîne utilisée pour se connecter à la source de données.|  
|`/incsdl:`\<fichier\>|Spécifie le fichier .csdl ou un répertoire où se trouvent les fichiers .csdl.  Cet argument peut être spécifié plusieurs fois afin de pouvoir spécifier plusieurs répertoires ou fichiers .csdl.  La spécification de plusieurs répertoires peut être utile pour générer des classes \(`/mode:EntityClassGeneration`\) ou des vues \(`/mode:ViewGeneration`\) lorsque le modèle conceptuel est divisé en plusieurs fichiers.  Cela peut également être utile lorsque vous voulez valider plusieurs modèles \(`/mode:ValidateArtifacts`\).|  
|`/refcsdl:`\<fichier\>|Spécifie le ou les fichiers .csdl supplémentaires utilisés pour résoudre toute référence dans le fichier .csdl source.  \(Le fichier .csdl source est le fichier spécifié par l'option `/incsdl`\).  Le fichier `/refcsdl` contient des types dont dépend le fichier .csdl source.  Cet argument peut être spécifié plusieurs fois.|  
|`/inmsl:`\<fichier\>|Spécifie le fichier .msl ou un répertoire où se trouvent les fichiers .msl.  Cet argument peut être spécifié plusieurs fois afin de pouvoir spécifier plusieurs répertoires ou fichiers .msl.  La spécification de plusieurs répertoires peut être utile pour générer des vues \(`/mode:ViewGeneration`\) lorsque le modèle conceptuel est divisé en plusieurs fichiers.  Cela peut également être utile lorsque vous voulez valider plusieurs modèles \(`/mode:ValidateArtifacts`\).|  
|`/inssdl:`\<fichier\>|Spécifie le fichier .ssdl ou un répertoire où se trouvent les fichiers .ssdl.  Cet argument peut être spécifié plusieurs fois afin de pouvoir spécifier plusieurs répertoires ou fichiers .ssdl.  Cela peut être utile lorsque vous voulez valider plusieurs modèles `(/mode:ValidateArtifacts)`.|  
|`/outcsdl:`\<fichier\>|Spécifie le nom du fichier .csdl qui sera créé.|  
|`/outmsl:`\<fichier\>|Spécifie le nom du fichier .msl qui sera créé.|  
|`/outssdl:`\<fichier\>|Spécifie le nom du fichier .ssdl qui sera créé.|  
|`/outobjectlayer:`\<fichier\>|Spécifie le nom du fichier de code source qui contient les objets générés à partir du fichier .csdl.|  
|`/outviews:`\<fichier\>|Spécifie le nom du fichier de code source qui contient les vues qui ont été générés.|  
|`/language:`\[VB&#124;CSharp\]|Spécifie le langage des fichiers de code source générés.  Le langage par défaut est C\#.|  
|`/namespace:`\<chaîne\>|Spécifie l'espace de noms du modèle à utiliser.  L'espace de noms est défini dans le fichier .csdl lors de l'exécution de `/mode:FullGeneration` ou de `/mode:FromSSDLGeneration`.  L'espace de noms n'est pas utilisé lors de l'exécution de `/mode:EntityClassGeneration`.|  
|`/entitycontainer:`\<chaîne\>|Spécifie le nom à appliquer à l'élément `<EntityContainer>` dans le modèle et les fichiers de mappage générés.|  
|`/pl[uralize]`|Applique aux noms `Entity`, `EntitySet` et `NavigationProperty` dans le modèle conceptuel les règles de la langue anglaise pour les singuliers et les pluriels.  Cette option effectuera les actions suivantes :<br /><br /> -   Mettre tous les noms `EntityType` au singulier.<br />-   Mettre tous les noms `EntitySet` au pluriel.<br />-   Pour chaque `NavigationProperty` qui retourne une entité au plus, mettre le nom au singulier.<br />-   Pour chaque `NavigationProperty` qui retourne plusieurs entités, mettre le nom au pluriel.|  
|`/SupressForeignKeyProperties or /nofk`|Empêche que des colonnes de clé étrangère soient exposées comme propriétés scalaires sur les types d'entité dans le modèle conceptuel.|  
|`/help` ou `?`|Affiche la syntaxe et les options de commande de l'outil.|  
|`/nologo`|Supprime l'affichage du message de copyright.|  
|`/targetversion:` \<chaîne\>|Version du .NET Framework qui sera utilisée pour compiler le code généré.  Les versions prises en charge sont 4 et 4.5.  La valeur par défaut est 4.|  
  
## Dans cette section  
 [Procédure : utiliser EdmGen.exe pour générer les fichiers de modèle et de mappage \(Entity Framework\)](../../../../../docs/framework/data/adonet/ef/how-to-use-edmgen-exe-to-generate-the-model-and-mapping-files.md)  
  
 [Procédure : utiliser EdmGen.exe pour générer le code de couche objet](../../../../../docs/framework/data/adonet/ef/how-to-use-edmgen-exe-to-generate-object-layer-code.md)  
  
 [Procédure : utiliser EdmGen.exe pour valider les fichiers de modèle et de mappage \(Entity Framework\)](../../../../../docs/framework/data/adonet/ef/how-to-use-edmgen-exe-to-validate-model-and-mapping-files.md)  
  
## Voir aussi  
 [ADO.NET Entity Data Model  Tools](http://msdn.microsoft.com/fr-fr/91076853-0881-421b-837a-f582f36be527)   
 [Entity Data Model](../../../../../docs/framework/data/adonet/entity-data-model.md)   
 [Spécifications CSDL, SSDL et MSL](../../../../../docs/framework/data/adonet/ef/language-reference/csdl-ssdl-and-msl-specifications.md)