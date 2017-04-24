---
title: "Comment&#160;: ex&#233;cuter des op&#233;rations de glisser-d&#233;placer entre des applications | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "glisser-déplacer, entre les applications"
ms.assetid: fa347436-2b12-4dd6-8507-59d7241f6a06
caps.latest.revision: 11
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 11
---
# Comment&#160;: ex&#233;cuter des op&#233;rations de glisser-d&#233;placer entre des applications
L'exécution d'opérations de glisser\-déplacer entre applications revient à activer cette action dans une application, du moment que les deux applications concernées respectent le "contrat" établi entre les propriétés <xref:System.Windows.Forms.DragEventArgs.AllowedEffect%2A> et <xref:System.Windows.Forms.DragEventArgs.Effect%2A>.  
  
 Dans la procédure suivante, vous allez utiliser une application Windows que vous aurez créée, ainsi que le traitement de texte WordPad fourni avec le système d'exploitation Windows pour effectuer des opérations de glisser\-déplacer entre applications.  WordPad possède un ensemble d'effets autorisés pour le texte faisant l'objet du glisser\-déplacer. L'application Windows pour laquelle vous allez écrire du code utilisera ces effets pour que les opérations de glisser\-déplacer se déroulent correctement.  
  
### Pour effectuer une opération de glisser\-déplacer entre applications  
  
1.  Créez une application Windows Forms.  
  
2.  Ajoutez un contrôle <xref:System.Windows.Forms.TextBox> à votre formulaire.  
  
3.  Configurez le contrôle <xref:System.Windows.Forms.TextBox> pour recevoir les données déplacées.  
  
     Pour plus d'informations, voir [Procédure pas à pas : exécution d'opérations de glisser\-déplacer dans Windows Forms](../../../../docs/framework/winforms/advanced/walkthrough-performing-a-drag-and-drop-operation-in-windows-forms.md).  
  
4.  Exécutez votre application Windows et pendant que celle\-ci s'exécute, exécutez WordPad.  
  
     WordPad est un éditeur de texte installé par Windows qui permet les opérations de glisser\-déplacer.  Pour y accéder, appuyez sur le bouton **Démarrer**, sélectionnez **Exécuter**. Tapez `WordPad` dans la zone de texte de la boîte de dialogue **Exécuter**, puis cliquez sur **OK**.  
  
5.  Une fois que WordPad est ouvert, tapez une chaîne de texte.  
  
6.  À l'aide de la souris, sélectionnez le texte, puis faites glisser le texte sélectionné vers le contrôle <xref:System.Windows.Forms.TextBox> dans votre application Windows.  
  
     Notez que quand vous placez la souris sur le contrôle <xref:System.Windows.Forms.TextBox> \(et déclenchez donc l'événement <xref:System.Windows.Forms.Control.DragEnter>\), le curseur se transforme et vous pouvez placer le texte sélectionné dans le contrôle <xref:System.Windows.Forms.TextBox>.  
  
     En outre, vous pouvez configurer le contrôle <xref:System.Windows.Forms.TextBox> pour permettre le glisser\-déplacer des chaînes de texte dans WordPad.  Pour plus d'informations, voir [Procédure pas à pas : exécution d'opérations de glisser\-déplacer dans Windows Forms](../../../../docs/framework/winforms/advanced/walkthrough-performing-a-drag-and-drop-operation-in-windows-forms.md).  
  
## Voir aussi  
 [Comment : ajouter des données au Presse\-papiers](../../../../docs/framework/winforms/advanced/how-to-add-data-to-the-clipboard.md)   
 [Comment : récupérer des données du Presse\-papiers](../../../../docs/framework/winforms/advanced/how-to-retrieve-data-from-the-clipboard.md)   
 [Opérations glisser\-déplacer et prise en charge du Presse\-papiers](../../../../docs/framework/winforms/advanced/drag-and-drop-operations-and-clipboard-support.md)