---
title: "Navigate Among UI Automation Elements with TreeWalker | Microsoft Docs"
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
  - "classes, TreeWalker"
  - "TreeWalker class"
  - "elements, navigating among"
  - "UI Automation, navigating among elements"
ms.assetid: afcd21dc-2ffa-48c9-9332-51269f44b7e9
caps.latest.revision: 8
author: "Xansky"
ms.author: "mhopkins"
manager: "markl"
caps.handback.revision: 8
---
# Navigate Among UI Automation Elements with TreeWalker
> [!NOTE]
>  Cette documentation s'adresse aux développeurs .NET Framework qui veulent utiliser les classes [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] managées définies dans l'espace de noms <xref:System.Windows.Automation>.  Pour obtenir les informations les plus récentes sur [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)], consultez [API Windows Automation : UI Automation \(page éventuellement en anglais\)](http://go.microsoft.com/fwlink/?LinkID=156746).  
  
 Cette rubrique contient un exemple de code qui montre comment naviguer entre les éléments [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] à l'aide de la classe <xref:System.Windows.Automation.TreeWalker>.  
  
## Exemple  
 L'exemple suivant utilise <xref:System.Windows.Automation.TreeWalker.GetParent%2A> pour remonter l'arborescence [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] jusqu'à ce qu'il trouve l'élément racine ou le bureau.  L'élément juste en dessous est la fenêtre parente de l'élément spécifié.  
  
 [!code-csharp[UIAFocusTracker_snip#102](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAFocusTracker_snip/CSharp/FocusTracker.cs#102)]
 [!code-vb[UIAFocusTracker_snip#102](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAFocusTracker_snip/VisualBasic/FocusTracker.vb#102)]  
  
## Exemple  
 L'exemple suivant utilise <xref:System.Windows.Automation.TreeWalker.GetFirstChild%2A> et <xref:System.Windows.Automation.TreeWalker.GetNextSibling%2A> pour créer un <xref:System.Windows.Forms.TreeView> affichant un sous\-arbre entier d'éléments [!INCLUDE[TLA#tla_uiautomation](../../../includes/tlasharptla-uiautomation-md.md)] qui se trouvent dans l'affichage de contrôle et sont activés.  
  
 [!code-csharp[UIAClient_snip#174](../../../samples/snippets/csharp/VS_Snippets_Wpf/UIAClient_snip/CSharp/ClientForm.cs#174)]
 [!code-vb[UIAClient_snip#174](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/UIAClient_snip/VisualBasic/ClientForm.vb#174)]  
  
## Voir aussi  
 [Obtaining UI Automation Elements](../../../docs/framework/ui-automation/obtaining-ui-automation-elements.md)