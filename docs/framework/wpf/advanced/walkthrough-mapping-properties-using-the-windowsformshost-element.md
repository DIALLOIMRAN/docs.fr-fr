---
title: "Proc&#233;dure pas &#224; pas&#160;: mappage de propri&#233;t&#233;s &#224; l&#39;aide de l&#39;&#233;l&#233;ment WindowsFormsHost | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "mapper des propriétés"
  - "WindowsFormsHost (mappage des propriétés des éléments)"
ms.assetid: 74809167-bf8e-48b7-a2e7-b4ea08bc7d8c
caps.latest.revision: 21
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 21
---
# Proc&#233;dure pas &#224; pas&#160;: mappage de propri&#233;t&#233;s &#224; l&#39;aide de l&#39;&#233;l&#233;ment WindowsFormsHost
Cette procédure pas à pas vous montre comment utiliser le <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A> propriété à mapper [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] propriétés aux propriétés correspondantes dans un hébergé [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] contrôle.  
  
 Cette procédure pas à pas décrit notamment les tâches suivantes :  
  
-   Création du projet.  
  
-   Définition de la disposition de l’application.  
  
-   Définition d’un nouveau mappage de propriété.  
  
-   Suppression d’un mappage de propriété par défaut.  
  
-   Remplacement d’un mappage de propriété par défaut.  
  
-   Extension d’un mappage de propriété par défaut.  
  
 Pour l’intégralité du code des tâches illustrées dans cette procédure pas à pas, consultez [propriétés de mappage à l’aide de l’élément WindowsFormsHost, exemple](http://go.microsoft.com/fwlink/?LinkID=160019).  
  
 Lorsque vous avez terminé, vous ne pourrez pas mapper [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] propriétés aux propriétés correspondantes dans un hébergé [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] contrôle.  
  
## <a name="prerequisites"></a>Conditions préalables  
 Pour exécuter cette procédure pas à pas, vous devez disposer des composants suivants :  
  
-   [!INCLUDE[vs_orcas_long](../../../../includes/vs-orcas-long-md.md)].  
  
## <a name="creating-the-project"></a>Création du projet  
  
#### <a name="to-create-and-set-up-the-project"></a>Pour créer et configurer le projet  
  
1.  Créez un projet d’Application WPF nommé `PropertyMappingWithWfhSample`.  
  
2.  Dans l’Explorateur de solutions, ajoutez une référence à l’assembly WindowsFormsIntegration nommé WindowsFormsIntegration.dll.  
  
3.  Dans l’Explorateur de solutions, ajoutez des références aux assemblys System.Drawing et System.Windows.Forms.  
  
## <a name="defining-the-application-layout"></a>Définir la mise en Application  
 Le [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]-application utilise le <xref:System.Windows.Forms.Integration.WindowsFormsHost> élément pour héberger un [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] contrôle.  
  
#### <a name="to-define-the-application-layout"></a>Pour définir la disposition de l’application  
  
1.  Ouvrez Window1.xaml dans le [!INCLUDE[wpfdesigner_current_short](../../../../includes/wpfdesigner-current-short-md.md)].  
  
2.  Remplacez le code existant par le code suivant.  
  
     [!code-xml[PropertyMappingWithWfhSample#1](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml#1)]  
  
3.  Ouvrez Window1.xaml.cs dans l’éditeur de Code.  
  
4.  En haut du fichier, importez les espaces de noms suivant.  
  
     [!code-csharp[PropertyMappingWithWfhSample#20](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#20)]
     [!code-vb[PropertyMappingWithWfhSample#20](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#20)]  
  
## <a name="defining-a-new-property-mapping"></a>Définition d’un nouveau mappage de propriété  
 Le <xref:System.Windows.Forms.Integration.WindowsFormsHost> élément fournit des mappages de propriété par défaut. Vous ajoutez un nouveau mappage de propriété en appelant le <xref:System.Windows.Forms.Integration.PropertyMap.Add%2A> méthode sur le <xref:System.Windows.Forms.Integration.WindowsFormsHost> l’élément <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A>.  
  
#### <a name="to-define-a-new-property-mapping"></a>Pour définir un nouveau mappage de propriété  
  
-   Copiez le code suivant dans la définition de la `Window1` classe.  
  
     [!code-csharp[PropertyMappingWithWfhSample#14](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#14)]
     [!code-vb[PropertyMappingWithWfhSample#14](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#14)]  
  
     Le `AddClipMapping` méthode ajoute un nouveau mappage pour le <xref:System.Windows.UIElement.Clip%2A> propriété.  
  
     Le `OnClipChange` méthode traduit le <xref:System.Windows.UIElement.Clip%2A> propriété du [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] <xref:System.Windows.Forms.Control.Region%2A> propriété.  
  
     Le `Window1_SizeChanged` méthode traite de la fenêtre <xref:System.Windows.FrameworkElement.SizeChanged> événement et la taille de la zone de découpage pour s’ajuster à la fenêtre d’application.  
  
## <a name="removing-a-default-property-mapping"></a>Suppression d’un mappage de propriété par défaut  
 Supprimer un mappage de propriété par défaut en appelant le <xref:System.Windows.Forms.Integration.PropertyMap.Remove%2A> méthode sur le <xref:System.Windows.Forms.Integration.WindowsFormsHost> l’élément <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A>.  
  
#### <a name="to-remove-a-default-property-mapping"></a>Pour supprimer un mappage de propriété par défaut  
  
-   Copiez le code suivant dans la définition de la `Window1` classe.  
  
     [!code-csharp[PropertyMappingWithWfhSample#13](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#13)]
     [!code-vb[PropertyMappingWithWfhSample#13](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#13)]  
  
     Le `RemoveCursorMapping` méthode supprime le mappage par défaut pour le <xref:System.Windows.FrameworkElement.Cursor%2A> propriété.  
  
## <a name="replacing-a-default-property-mapping"></a>Remplacement d’un mappage de propriété par défaut  
 Remplacez un mappage de propriété par défaut en supprimant le mappage par défaut et en appelant le <xref:System.Windows.Forms.Integration.PropertyMap.Add%2A> méthode sur le <xref:System.Windows.Forms.Integration.WindowsFormsHost> l’élément <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A>.  
  
#### <a name="to-replace-a-default-property-mapping"></a>Pour remplacer un mappage de propriété par défaut  
  
-   Copiez le code suivant dans la définition de la `Window1` classe.  
  
     [!code-csharp[PropertyMappingWithWfhSample#12](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#12)]
     [!code-vb[PropertyMappingWithWfhSample#12](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#12)]  
  
     Le `ReplaceFlowDirectionMapping` méthode remplace le mappage par défaut pour le <xref:System.Windows.FrameworkElement.FlowDirection%2A> propriété.  
  
     Le `OnFlowDirectionChange` méthode traduit le <xref:System.Windows.FrameworkElement.FlowDirection%2A> propriété du [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] <xref:System.Windows.Forms.Control.RightToLeft%2A> propriété.  
  
     Le `cb_CheckedChanged` méthode gère les <xref:System.Windows.Forms.CheckBox.CheckedChanged> événement sur le <xref:System.Windows.Forms.CheckBox> contrôle. Il assigne le <xref:System.Windows.FrameworkElement.FlowDirection%2A> propriété basée sur la valeur de la <xref:System.Windows.Forms.CheckBox.CheckState%2A> propriété  
  
## <a name="extending-a-default-property-mapping"></a>Extension d’un mappage de propriété par défaut  
 Vous pouvez utiliser un mappage de propriété par défaut et l’étendre avec votre propre mappage.  
  
#### <a name="to-extend-a-default-property-mapping"></a>Pour étendre un mappage de propriété par défaut  
  
-   Copiez le code suivant dans la définition de la `Window1` classe.  
  
     [!code-csharp[PropertyMappingWithWfhSample#15](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#15)]
     [!code-vb[PropertyMappingWithWfhSample#15](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#15)]  
  
     Le `ExtendBackgroundMapping` méthode ajoute un traducteur de propriété personnalisé à l’objet de <xref:System.Windows.Controls.Control.Background%2A> mappage de propriété.  
  
     Le `OnBackgroundChange` méthode assigne une image spécifique pour le contrôle hébergé <xref:System.Windows.Forms.Control.BackgroundImage%2A> propriété. Le `OnBackgroundChange` méthode est appelée une fois le mappage de propriété par défaut est appliqué.  
  
## <a name="initializing-your-property-mappings"></a>Initialisation de vos mappages de propriété  
 Définissez vos mappages de propriété en appelant les méthodes décrites précédemment dans le <xref:System.Windows.FrameworkElement.Loaded> Gestionnaire d’événements.  
  
#### <a name="to-initialize-your-property-mappings"></a>Pour initialiser vos mappages de propriété  
  
1.  Copiez le code suivant dans la définition de la `Window1` classe.  
  
     [!code-csharp[PropertyMappingWithWfhSample#11](../../../../samples/snippets/csharp/VS_Snippets_Wpf/PropertyMappingWithWfhSample/CSharp/PropertyMappingWithWfh/Window1.xaml.cs#11)]
     [!code-vb[PropertyMappingWithWfhSample#11](../../../../samples/snippets/visualbasic/VS_Snippets_Wpf/PropertyMappingWithWfhSample/VisualBasic/PropertyMappingWithWfh/Window1.xaml.vb#11)]  
  
     Le `WindowLoaded` méthode gère les <xref:System.Windows.FrameworkElement.Loaded> événement et exécute l’initialisation suivante.  
  
    -   Crée un [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] <xref:System.Windows.Forms.CheckBox> contrôle.  
  
    -   Appelle les méthodes que vous avez défini précédemment dans la procédure pas à pas pour configurer les mappages de propriété.  
  
    -   Assigne les valeurs initiales aux propriétés mappées.  
  
2.  Appuyez sur F5 pour générer et exécuter l'application. Cliquez sur la case à cocher pour voir l’effet de la <xref:System.Windows.FrameworkElement.FlowDirection%2A> mappage. Lorsque vous cliquez sur la case à cocher, la mise en page inverse son orientation de gauche à droite.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:System.Windows.Forms.Integration.WindowsFormsHost.PropertyMap%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.Integration.ElementHost.PropertyMap%2A?displayProperty=fullName>   
 <xref:System.Windows.Forms.Integration.WindowsFormsHost>   
 [Windows Forms et WPF le mappage de propriété](../../../../docs/framework/wpf/advanced/windows-forms-and-wpf-property-mapping.md)   
 [Concepteur WPF](http://msdn.microsoft.com/fr-fr/c6c65214-8411-4e16-b254-163ed4099c26)   
 [Procédure pas à pas : Hébergement d’un contrôle Windows Forms dans WPF](../../../../docs/framework/wpf/advanced/walkthrough-hosting-a-windows-forms-control-in-wpf.md)