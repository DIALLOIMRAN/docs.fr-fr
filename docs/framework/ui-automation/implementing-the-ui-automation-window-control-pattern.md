---
title: "Implementing the UI Automation Window Control Pattern | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-bcl"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "control patterns, Window"
  - "UI Automation, Window control pattern"
  - "Window control pattern"
ms.assetid: a28cb286-296e-4a62-b4cb-55ad636ebccc
caps.latest.revision: 21
author: "Xansky"
ms.author: "mhopkins"
manager: "markl"
caps.handback.revision: 20
---
# Implementing the UI Automation Window Control Pattern
> [!NOTE]
>  Cette documentation s'adresse aux développeurs .NET Framework qui souhaitent utiliser les classes [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] managées définies dans l'espace de noms <xref:System.Windows.Automation>. Pour obtenir les dernières informations sur [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consultez [API Windows Automation : UI Automation](http://go.microsoft.com/fwlink/?LinkID=156746).  
  
 Cette rubrique présente des conventions et des directives pour implémenter <xref:System.Windows.Automation.Provider.IWindowProvider>, notamment des informations sur les propriétés, méthodes et événements <xref:System.Windows.Automation.WindowPattern>. Des liens vers des références supplémentaires sont répertoriés à la fin de la rubrique.  
  
 Le modèle de contrôle <xref:System.Windows.Automation.WindowPattern> est utilisé pour prendre en charge les contrôles qui fournissent une fonctionnalité de fenêtre fondamentale dans une [!INCLUDE[TLA#tla_gui](../../../includes/tlasharptla-gui-md.md)] traditionnelle. Les contrôles qui doivent implémenter ce modèle de contrôle sont notamment les fenêtres d’application de niveau supérieur, les fenêtres enfants de l’[!INCLUDE[TLA#tla_mdi](../../../includes/tlasharptla-mdi-md.md)], les contrôles de volet de fractionnement redimensionnables, les boîtes de dialogue modales et les fenêtres d’info\-bulle.  
  
<a name="Implementation_Guidelines_and_Conventions"></a>   
## Conventions et directives d'implémentation  
 Quand vous implémentez le modèle de contrôle Window, notez les conventions et recommandations suivantes :  
  
-   Pour prendre en charge la possibilité de modifier la taille de la fenêtre et la position de l’écran à l’aide d’UI Automation, un contrôle doit implémenter <xref:System.Windows.Automation.Provider.ITransformProvider> en plus de <xref:System.Windows.Automation.Provider.IWindowProvider>.  
  
-   Les contrôles qui contiennent des barres de titre et des éléments de barre de titre qui permettent de déplacer, redimensionner, agrandir, réduire ou fermer le contrôle sont généralement obligatoires pour implémenter <xref:System.Windows.Automation.Provider.IWindowProvider>.  
  
-   En général, les contrôles tels que les info\-bulles contextuelles, les zones de liste modifiable ou les menus déroulants n’implémentent pas <xref:System.Windows.Automation.Provider.IWindowProvider>.  
  
-   Les fenêtres d’info\-bulle se distinguent des info\-bulles contextuelles de base par un bouton Fermer identique à celui des fenêtres.  
  
-   Le mode plein écran n’est pas pris en charge par IWindowProvider, car il est propre à la fonctionnalité d’une application et n’est pas un comportement standard de fenêtre.  
  
<a name="Required_Members_for_IWindowProvider"></a>   
## Membres obligatoires pour IWindowProvider  
 Les propriétés, méthodes et événements suivants sont obligatoires pour l’interface IWindowProvider.  
  
|Membre obligatoire|Type de membre|Notes|  
|------------------------|--------------------|-----------|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.InteractionState%2A>|Propriété|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.IsModal%2A>|Propriété|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.IsTopmost%2A>|Propriété|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.Maximizable%2A>|Propriété|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.Minimizable%2A>|Propriété|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.VisualState%2A>|Propriété|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.Close%2A>|Méthode|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.SetVisualState%2A>|Méthode|Aucune|  
|<xref:System.Windows.Automation.Provider.IWindowProvider.WaitForInputIdle%2A>|Méthode|Aucune|  
|<xref:System.Windows.Automation.WindowPattern.WindowClosedEvent>|Événement|Aucune|  
|<xref:System.Windows.Automation.WindowPattern.WindowOpenedEvent>|Événement|Aucune|  
|<xref:System.Windows.Automation.WindowInteractionState>|Événement|N’est pas nécessairement défini sur <xref:System.Windows.Automation.WindowInteractionState>|  
  
<a name="Exceptions"></a>   
## Exceptions  
 Les fournisseurs doivent lever les exceptions suivantes.  
  
|Type d'exception|Condition|  
|----------------------|---------------|  
|<xref:System.InvalidOperationException>|<xref:System.Windows.Automation.Provider.IWindowProvider.SetVisualState%2A><br /><br /> -   Quand un contrôle ne prend pas en charge un comportement demandé.|  
|<xref:System.ArgumentOutOfRangeException>|<xref:System.Windows.Automation.Provider.IWindowProvider.WaitForInputIdle%2A><br /><br /> -   Quand le paramètre n’est pas un nombre valide.|  
  
## Voir aussi  
 [UI Automation Control Patterns Overview](../../../docs/framework/ui-automation/ui-automation-control-patterns-overview.md)   
 [Support Control Patterns in a UI Automation Provider](../../../docs/framework/ui-automation/support-control-patterns-in-a-ui-automation-provider.md)   
 [UI Automation Control Patterns for Clients](../../../docs/framework/ui-automation/ui-automation-control-patterns-for-clients.md)   
 [UI Automation Tree Overview](../../../docs/framework/ui-automation/ui-automation-tree-overview.md)   
 [Use Caching in UI Automation](../../../docs/framework/ui-automation/use-caching-in-ui-automation.md)