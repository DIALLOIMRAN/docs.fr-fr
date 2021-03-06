---
title: "Winres.exe (Éditeur de ressources Windows Forms)"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
- CSharp
- C++
- jsharp
helpviewer_keywords:
- Winres.exe
- Windows Forms Resource Editor
- localization
- Windows Forms, localization
- forms, localizing
- resx files
- .resx files
ms.assetid: cb8bc835-9221-4888-af53-1a4f5fad6c48
caps.latest.revision: 23
author: mairaw
ms.author: mairaw
manager: wpickett
ms.translationtype: HT
ms.sourcegitcommit: 934373d61407c8cc19b7d6424898a582880f9c21
ms.openlocfilehash: e2a2121d97cd1f7de211a72ed95e2fdf24bfac60
ms.contentlocale: fr-fr
ms.lasthandoff: 08/10/2017

---
# <a name="winresexe-windows-forms-resource-editor"></a>Winres.exe (Éditeur de ressources Windows Forms)
L'Éditeur de ressources Windows Forms, Winres.exe, est un outil de présentation visuelle qui permet aux experts en localisation de localiser les ressources de l'interface utilisateur Windows Forms utilisées par des formulaires. Vous pouvez créer les fichiers .resx ou .resources servant d'entrée à Winres.exe à l'aide d'un environnement de design visuel tel que Microsoft Visual Studio. Pour plus d’informations sur le déploiement des ressources dans les applications .NET Framework, consultez [Ressources dans des applications de bureau](../../../docs/framework/resources/index.md).  
  
 Cet outil est installé automatiquement avec Visual Studio. Pour exécuter l'outil, utilisez l'invite de commandes développeur (ou l'invite de commandes Visual Studio dans Windows 7). Pour plus d’informations, consultez [Invites de commandes](../../../docs/framework/tools/developer-command-prompt-for-vs.md).  
  
 À l'invite de commandes, tapez le texte suivant :  
  
## <a name="syntax"></a>Syntaxe  
  
```  
winres resourceFile   
winres /?   
```  
  
## <a name="remarks"></a>Remarques  
  
|Argument|Description|  
|--------------|-----------------|  
|`resourceFile`|Fichier de ressources à localiser. Ce fichier doit être un fichier de ressources .resx ou .resources de formulaire Windows Forms généré par le concepteur Visual Studio . Winres.exe ne peut pas ouvrir les fichiers génériques .resx ou .resources.|  
  
|Option|Description|  
|------------|-----------------|  
|**/?**|Affiche la syntaxe et les options de commande de l'outil.|  
  
 L'état des éléments d'interface d'un formulaire dans un projet Windows Forms est généralement stocké dans des fichiers de ressources, c'est-à-dire des fichiers XML portant l'extension .resx ou leurs versions binaires et compilées correspondantes, portant l'extension .resources. Winres.exe est un outil qui permet d'éditer de façon limitée les deux types de fichiers en dehors de l'environnement de design Visual Studio. Vous pouvez y effectuer les types de modifications suivants :  
  
-   Un fichier de ressources de culture spécifique ou neutre peut être modifié pour changer les propriétés d'interface utilisateur du formulaire ou de ses contrôles, notamment le texte, la taille ou la position.  
  
-   Les fichiers de ressources de culture neutre ou spécifique peuvent être générés à partir du fichier de ressources par défaut.  
  
-   Un fichier de ressources avec une culture spécifique peut être enregistré en tant que fichier de ressources d'une autre culture. Par exemple, un fichier de ressources anglais (États-Unis) peut être enregistré en tant que fichier de ressources polonais. En général, le nouveau fichier est modifié par la suite pour être compatible avec la nouvelle culture.  
  
 Consultez également [Organisation hiérarchique des ressources pour la localisation](http://msdn.microsoft.com/library/756hydy4\(v=vs.110\)) ou [Organisation hiérarchique des ressources pour la localisation](http://msdn.microsoft.com/library/756hydy4\(v=vs.120\)).  
  
 Winres.exe ne peut pas convertir un fichier .resx en fichier .resources correspondant ; utilisez à la place l'outil Resgen.exe. Pour plus d’informations sur Resgen.exe, consultez [Resgen.exe (Générateur de fichier de ressources)](../../../docs/framework/tools/resgen-exe-resource-file-generator.md).  
  
 Winres.exe est une application graphique qui recrée une version d’un formulaire Windows Forms au moment du design simplement à partir du fichier de ressources, sans devoir accéder au code source. Winres.exe héberge le Concepteur de formulaires Windows Forms et la fenêtre Propriétés de Visual Studio. Ces fonctionnalités permettent une modification visuelle d'un fichier .resources ou .resx contenant un formulaire Windows Forms. En règle générale, les localisateurs utilisent Winres.exe pour modifier les étiquettes des contrôle et régler l'emplacement et la taille de ces derniers pour adapter les étiquettes à la culture cible.  
  
 Si Winres.exe ne parvient pas à résoudre le type d'un contrôle, il crée un contrôle réservé dans le fichier .resx ou .resources localisé. Ce contrôle apparaît sur le formulaire Windows Forms sous la forme d'une fenêtre hachurée. dont la taille et la position correspondent à celles du contrôle réel. Toutes les propriétés localisables disponibles pour le contrôle réservé apparaissent dans la fenêtre Propriétés. Toute modification apportée au contrôle réservé est enregistrée dans le contrôle réel.  
  
## <a name="winresexe-versus-visual-studio"></a>Winres.exe et Visual Studio  
 En général, avant de commencer à localiser les formulaires Windows Forms d'une application, vous devez décider si vous souhaitez utiliser Visual Studio ou Winres.exe comme outil de localisation. Certains problèmes de compatibilité de versions, décrits ultérieurement, peuvent vous empêcher de basculer d'un outil à l'autre.  
  
 L'avantage de Visual Studio réside dans le fait qu'il permet à la fois de développer et de localiser une application. Pour localiser un formulaire à l’issue du développement, affectez au <xref:System.ComponentModel.LocalizableAttribute> (propriété **Localizable** dans l’éditeur des propriétés) du formulaire la valeur `true` et modifiez sa propriété **Language** en fonction de la culture cible souhaitée. Ensuite, modifiez les chaînes et réglez l'emplacement et la taille des contrôles en fonction des chaînes de la culture cible. Lors de l'enregistrement du fichier .resx localisé, Visual Studio n'écrit que les propriétés localisables (les propriétés modifiées dans la culture cible) dans le fichier. Visual Studio crée automatiquement un assembly satellite pour le fichier .resx localisé à l'emplacement de répertoire correct.  Consultez aussi [Procédure pas à pas : localisation de Windows Forms](http://msdn.microsoft.com/library/y99d1cd3\(v=vs.110\)).  
  
 Visual Studio fournit un environnement de développement et de localisation intégré, mais Winres.exe est préférable si la localisation est effectuée par des tiers. Dans la mesure où Winres.exe est exclusivement un outil de localisation, il permet de mieux séparer le code d'une application des formulaires à localiser et s'avère plus pratique pour gérer des projets volumineux.  
  
## <a name="using-winresexe"></a>Utilisation de Winres.exe  
 Si vous optez pour une localisation avec Winres.exe, vous devez d'abord développer une application à l'aide d'un concepteur visuel tel que le Concepteur de formulaires dans Visual Studio. Une fois le développement terminé, affectez au <xref:System.ComponentModel.LocalizableAttribute> du formulaire (propriété **Localizable** dans l’éditeur des propriétés) la valeur `true`, puis confiez le fichier .resx de la culture neutre à un localisateur tiers. Ce fichier .resx contient des informations supplémentaires dont se sert Winres.exe pour recréer une version du formulaire d’origine au moment du design.  
  
> [!CAUTION]
>  Winres.exe ne peut pas être utilisé pour modifier le fichier de ressources par défaut. Winres.exe interprète toutes les propriétés modifiées comme des propriétés localisées et les enregistre dans le fichier de ressources de la culture cible.  
  
 Les versions finales des fichiers de ressources de la culture peuvent enfin être utilisées pour créer des versions localisées de l'application. Pour plus d’informations, consultez [Ressources dans les applications de bureau](../../../docs/framework/resources/index.md).  
  
 La version 2.0 de Winres.exe possède les fonctions et fonctionnalités suivantes :  
  
-   Winres peut fonctionner en mode fichier unique (SFM) ou en mode fichier Visual Studio (VSFM). Le mode fichier unique est le mode hérité (legacy) où l'intégralité des informations relatives au formulaire et à son contenu sont stockées dans le fichier de ressources. Le mode fichier Visual Studio stocke uniquement les modifications relatives à la culture dans le fichier de ressources.  
  
-   Une fenêtre de rapport d'erreurs, ancrée dans le coin inférieur gauche de la fenêtre principale, a été ajoutée à l'interface.  
  
-   Vous pouvez vérifier s’il existe des doublons de touches d’accès rapide : dans le menu Format, cliquez sur la commande **Vérifiez les touches d’accès rapide**.  
  
## <a name="version-compatibility"></a>Compatibilité des versions  
 Dans la mesure où le format des fichiers de ressources entre Visual Studio .NET 2002 et Visual Studio 2005 a changé, Winres.exe a été également modifié pour être compatible. Par conséquent, en règle générale, vous devez utiliser la version de Winres.exe publiée avec le .NET Framework que vous utilisez pour créer l'application. Le tableau suivant répertorie les versions compatibles.  
  
|Visual Studio|.NET Framework|Winres.exe|  
|-------------------|--------------------|----------------|  
|Visual Studio .NET 2002|1.0|1.0|  
|Visual Studio .NET 2003|1.1|1.1|  
|Visual Studio 2005|2.0|2.0|  
|Visual Studio 2008|3.0 et 3.5|3.0 et 3.5|  
|Visual Studio 2010|4.0|4.0|  
  
 Si vous essayez d'ouvrir un fichier de ressources plus ancien avec la version 2.0 de Winres.exe, vous serez invité à mettre à niveau le format du fichier pour qu'il soit compatible avec la version 2.0 du .NET Framework.  
  
 Dans les versions du .NET Framework antérieures à la version 2.0, Winres.exe et le Concepteur de formulaires de Visual Studio créaient des fichiers de ressources de culture spécifique ou neutre incompatibles. Dès lors, vous n'aviez d'autre choix, une fois le processus de localisation entamé, que de continuer à utiliser le même outil. En revanche, le mode fichier Visual Studio (VSFM) a été ajouté à la version 2.0 de Winres.exe. Comme son nom l'indique, un fichier de ressources enregistré dans ce mode de compatibilité peut être modifié avec les deux outils.  
  
> [!NOTE]
>  Bien que le mode fichier Visual Studio présente l'avantage d'être compatible avec Visual Studio, dans la mesure où il stocke uniquement les valeurs modifiées dans le fichier de ressources, Winres.exe exige que les parents du fichier de ressources actif figurent dans le même répertoire. Par exemple, `TestApp.de-DE.resources`, un fichier de ressources en allemand, nécessite la présence du fichier de ressources par défaut, `TestApp.resx`, et éventuellement celle du fichier de ressources de culture neutre, `TestApp.de.resources`.  
  
## <a name="examples"></a>Exemples  
  
#### <a name="to-localize-a-resx-or-resources-file-associated-with-a-form"></a>Pour localiser un fichier .resx ou .resources associé à un formulaire  
  
1.  Tapez `winres` à l'invite de commandes développeur pour exécuter Winres.exe.  
  
2.  Pour ouvrir les ressources par défaut d’un formulaire à localiser, cliquez sur la commande **Ouvrir** dans le menu **Fichier** et accédez au fichier pour l’ouvrir.  
  
     ou  
  
     Spécifiez le fichier à ouvrir dans la ligne de commande lorsque vous démarrez Winres.exe.  
  
     La commande suivante démarre Winres.exe et charge le formulaire associé à `TestApp.resx` dans le Concepteur de formulaires.  
  
    ```  
    winres TestApp.resx  
    ```  
  
     La commande suivante démarre Winres.exe et charge le formulaire associé à `TestApp.resources` dans le Concepteur de formulaires.  
  
    ```  
    winres TestApp.resources  
    ```  
  
    > [!NOTE]
    >  Si le formulaire dont vous modifiez les ressources est un formulaire hérité, à la fois l'assembly qui contient le formulaire hérité et l'assembly qui contient le formulaire héritant (dérivé) doivent être enregistrés dans le Global Assembly Cache (GAC) ou résider dans le même répertoire que WinRes.exe. Pour plus d’informations sur l’installation des composants du .NET Framework dans le GAC, consultez [Global Assembly Cache](../../../docs/framework/app-domains/gac.md).  
  
3.  Sélectionnez les contrôles sur le formulaire et modifiez leurs propriétés <xref:System.Windows.Forms.Control.Text%2A> et autres propriétés pour refléter la culture localisée et sa langue. Si nécessaire, déplacez ou redimensionnez les contrôles pour tenir compte du texte localisé.  
  
4.  Pour enregistrer la version localisée du fichier .resx ou .resources, cliquez sur l’icône **Enregistrer** ou sur la même commande dans le menu **Fichier**. L’outil affiche la fenêtre **Choisir la culture**.  
  
5.  Sélectionnez la culture appropriée et le mode de fichier, puis cliquez sur **OK**. L'outil enregistre le fichier à l'aide de la convention de nommage que l'exécution attend pour les fichiers de ressources localisés. Par exemple, si vous localisez `TestApp.resources` en allemand, l'outil enregistre le fichier sous `TestApp.de-DE.resources`. Si vous localisez `TestApp.resx` en allemand, l'outil enregistre le fichier sous `TestApp.de-DE.resx`. Pour plus d’informations sur les conventions de nommage des ressources, consultez [Empaquetage et déploiement de ressources](../../../docs/framework/resources/packaging-and-deploying-resources-in-desktop-apps.md). Pour obtenir la liste des noms de cultures prédéfinis utilisés au moment de l’exécution, consultez la classe <xref:System.Globalization.CultureInfo>.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.ComponentModel.LocalizableAttribute>   
 <xref:System.Globalization.CultureInfo>   
 <xref:System.Resources.ResourceManager>   
 <xref:System.Resources.ResourceReader>   
 <xref:System.Resources.ResourceWriter>   
 [Outils](../../../docs/framework/tools/index.md)   
 [Ressources dans des applications de bureau](../../../docs/framework/resources/index.md)   
 [Globalisation et localisation](../../../docs/standard/globalization-localization/index.md)

