---
title: "Comment&#160;: cr&#233;er un contr&#244;le Windows&#160;Forms qui affiche la progression | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "contrôles (Windows Forms), créer"
  - "contrôles (Windows Forms), progression (suivre)"
  - "FlashTrackBar (contrôle personnalisé)"
  - "progression, rapports (Windows Forms)"
ms.assetid: 24c5a2e3-058c-4b8d-a217-c06e6a130c2f
caps.latest.revision: 6
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 6
---
# Comment&#160;: cr&#233;er un contr&#244;le Windows&#160;Forms qui affiche la progression
L'exemple de code suivant illustre un contrôle personnalisé appelé `FlashTrackBar` qui permet d'afficher pour l'utilisateur le niveau ou la progression d'une application.  Il utilise un dégradé pour représenter visuellement la progression.  
  
 Le contrôle `FlashTrackBar` illustre les concepts suivants :  
  
-   Définition des propriétés personnalisées.  
  
-   Définition des événements personnalisés.  \(`FlashTrackBar` définit l'événement `ValueChanged`\)  
  
-   Substitution de la méthode <xref:System.Windows.Forms.Control.OnPaint%2A> pour fournir la logique permettant de dessiner le contrôle.  
  
-   Calcul de la zone disponible pour le dessin du contrôle à l'aide de sa propriété <xref:System.Windows.Forms.Control.ClientRectangle%2A>.  `FlashTrackBar` fait cela dans sa méthode `OptimizedInvalidate`.  
  
-   Implémentation de la sérialisation ou de la persistance d'une propriété lorsqu'elle est modifiée dans le concepteur Windows Forms.  `FlashTrackBar` définit les méthodes `ShouldSerializeStartColor` et `ShouldSerializeEndColor` pour la sérialisation de ses propriétés `StartColor` et `EndColor`.  
  
 Le tableau suivant affiche les propriétés personnalisées définies par `FlashTrackBar`.  
  
|Propriété|Description|  
|---------------|-----------------|  
|`AllowUserEdit`|Indique si l'utilisateur peut modifier la valeur de la barre de suivi de couleur en cliquant dessus et en la faisant glisser.|  
|`EndColor`|Spécifie la couleur de fin de la barre de suivi.|  
|`DarkenBy`|Spécifie l'assombrissement de l'arrière\-plan par rapport au dégradé de premier plan.|  
|`Max`|Spécifie la valeur maximale de la barre de suivi.|  
|`Min`|Spécifie la valeur minimale de la barre de suivi.|  
|`StartColor`|Spécifie la couleur de début du dégradé.|  
|`ShowPercentage`|Indique si un pourcentage doit être affiché sur le dégradé.|  
|`ShowValue`|Indique si la valeur actuelle doit être affichée sur le dégradé.|  
|`ShowGradient`|Indique si la barre de suivi doit afficher un dégradé de couleur montrant la valeur actuelle.|  
|-   `Value`|Spécifie la valeur actuelle de la barre de suivi.|  
  
 Le tableau suivant affiche des membres supplémentaires définis par l'événement de modification de propriété `FlashTrackBar:` et la méthode qui déclenche l'événement.  
  
|Membre|Description|  
|------------|-----------------|  
|`ValueChanged`|Événement déclenché lorsque la propriété `Value` de la barre de suivi change.|  
|`OnValueChanged`|Méthode qui déclenche l'événement `ValueChanged`.|  
  
> [!NOTE]
>  `FlashTrackBar` utilise la classe <xref:System.EventArgs> pour les données d'événement et <xref:System.EventHandler> pour le délégué d'événement.  
  
 Pour gérer les événements *NomÉvénement* correspondants, `FlashTrackBar` substitue les méthodes suivantes qu'il hérite de <xref:System.Windows.Forms.Control?displayProperty=fullName> :  
  
-   <xref:System.Windows.Forms.Control.OnPaint%2A>  
  
-   <xref:System.Windows.Forms.Control.OnMouseDown%2A>  
  
-   <xref:System.Windows.Forms.Control.OnMouseMove%2A>  
  
-   <xref:System.Windows.Forms.Control.OnMouseUp%2A>  
  
-   <xref:System.Windows.Forms.Control.OnResize%2A>  
  
 Pour gérer les événements de modification de propriété correspondants, `FlashTrackBar` substitue les méthodes suivantes qu'il hérite de <xref:System.Windows.Forms.Control?displayProperty=fullName> :  
  
-   <xref:System.Windows.Forms.Control.OnBackColorChanged%2A>  
  
-   <xref:System.Windows.Forms.Control.OnBackgroundImageChanged%2A>  
  
-   <xref:System.Windows.Forms.Control.OnTextChanged%2A>  
  
## Exemple  
 Le contrôle `FlashTrackBar` définit deux éditeurs de types munis d'une interface utilisateur, `FlashTrackBarValueEditor` et `FlashTrackBarDarkenByEditor`, qui sont affichés dans les listes de code ci\-dessous.  La classe `HostApp` utilise le contrôle `FlashTrackBar` sur un Windows Form.  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#1](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#1)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#1](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#1)]  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#10](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBarDarkenByEditor.cs#10)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#10](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBarDarkenByEditor.vb#10)]  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#20](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBarValueEditor.cs#20)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#20](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBarValueEditor.vb#20)]  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#30](../../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/HostApp.cs#30)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#30](../../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/HostApp.vb#30)]  
  
## Voir aussi  
 [Extending Design\-Time Support](../Topic/Extending%20Design-Time%20Support.md)   
 [Concepts de base du développement de contrôles Windows Forms](../../../../docs/framework/winforms/controls/windows-forms-control-development-basics.md)