---
title: "Marquage des &#233;v&#233;nements rout&#233;s comme g&#233;r&#233;s et gestion de classe | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-wpf"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "événements de propagation"
  - "écouteurs de classe"
  - "contrôles composites"
  - "contrôles, composition"
  - "événements, propagation"
  - "événements, supprimer"
  - "événements, tunneling"
  - "écouteurs d'instance"
  - "écouteurs"
  - "aperçu des événements routés"
  - "événements routés, marquer comme géré"
  - "événements routés, Aperçu"
  - "supprimer des événements"
  - "événements de tunneling"
ms.assetid: 5e745508-4861-4b48-b5f6-5fc7ce5289d2
caps.latest.revision: 19
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 18
---
# Marquage des &#233;v&#233;nements rout&#233;s comme g&#233;r&#233;s et gestion de classe
Les gestionnaires d'un événement routé peuvent marquer l'événement géré dans les données d'événement.  La gestion de l'événement raccourcira l'itinéraire efficacement.  La gestion de classe est un concept de programmation pris en charge par les événements routés.  Un gestionnaire de classe a la possibilité de gérer un événement routé particulier à un niveau de la classe avec un gestionnaire appelé avant tout gestionnaire d'instance sur toute instance de la classe.  
  
   
  
<a name="prerequisites"></a>   
## Composants requis  
 Cette rubrique décrit plus en détail les concepts introduits dans [Vue d'ensemble des événements routés](../../../../docs/framework/wpf/advanced/routed-events-overview.md).  
  
<a name="When_to_Mark_Events_as_Handled"></a>   
## Quand marquer des événements comme gérés  
 L'affectation de la valeur `true` à la propriété <xref:System.Windows.RoutedEventArgs.Handled%2A> dans les données d'un événement routé est appelée "marquage de l'événement géré".  Aucune règle absolue ne détermine quand marquer des événements routés comme gérés, que vous soyez un auteur d'application ou un auteur de contrôle répondant aux événements routés existants ou implémentant de nouveaux événements routés.  Globalement, le concept "géré", tel qu'il est retenu dans les données de l'événement routé, doit être utilisé comme un protocole limité pour les réponses de votre propre application aux nombreux événements routés présentés dans les [!INCLUDE[TLA2#tla_api#plural](../../../../includes/tla2sharptla-apisharpplural-md.md)] [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] ainsi qu'à tout événement routé personnalisé.  Une autre façon de réfléchir au problème « géré » est que vous devez en général marquer un événement routé géré si votre code répondait à l'événement routé de manière significative et relativement complète. En général, il ne doit pas y avoir plusieurs réponses significatives nécessitant des implémentations séparées de gestionnaire pour toute occurrence d'événement routé unique.  Si plusieurs réponses sont nécessaires, le code correspondant doit être implémenté à travers la logique de l'application chaînée dans un gestionnaire unique plutôt qu'en utilisant le système d'événement routé pour le transfert.  Le concept de ce qui est "significatif" est également subjectif et dépend de votre application ou de votre code.  En règle générale, certains exemples de "réponses significatives" comprennent la définition du focus, la modification de l'état public, la définition des propriétés qui affectent la représentation visuelle et le déclenchement d'autres événements nouveaux.  Les exemples de réponses non significatives comprennent la modification de l'état privé \(sans impact visuel ni représentation de programmation\), l'enregistrement d'événements ou la consultation des arguments d'un événement et la décision de ne pas y répondre.  
  
 Le comportement du système de l'événement routé renforce ce modèle de « réponse significative » pour l'utilisation de l'état géré d'un événement routé car les gestionnaires ajoutés en [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] ou la signature commune de <xref:System.Windows.UIElement.AddHandler%2A> ne sont pas appelés en réponse à un événement routé où les données d'événement sont déjà marquées comme étant gérées.  Vous devez déployer un effort supplémentaire pour ajouter un gestionnaire avec la version de paramètre `handledEventsToo` \(<xref:System.Windows.UIElement.AddHandler%28System.Windows.RoutedEvent%2CSystem.Delegate%2CSystem.Boolean%29>\) afin de gérer les événements routés marqués comme étant gérés par premiers participants dans l'itinéraire de l'événement.  
  
 Dans certains cas, les contrôles mêmes marquent certains événements routés comme étant gérés.  Un événement routé géré représente une décision des auteurs de contrôle [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] que les actions du contrôle en réponse à l'événement routé soient significatives ou complètes dans le cadre de l'implémentation du contrôle, et l'événement ne nécessite plus aucune autre gestion.  Cela se fait généralement en ajoutant un gestionnaire de classe pour un événement ou en remplaçant l'un des virtuels de gestionnaire de classe qui existent sur une classe de base.  Vous pouvez toujours résoudre les problèmes liés à cette gestion d'événements, si nécessaire ; consultez [Résolution des problèmes liés à la suppression d'événements par des contrôles](#WorkingAroundEventSuppressionByControls) plus loin dans cette rubrique.  
  
<a name="Preview_Events_vs__Bubbling_Events_and_Handling"></a>   
## Événements d'aperçu \(tunneling\) ouévénements de propagation et gestion des événements  
 Les événements routés d'aperçu sont des événements qui suivent un itinéraire [de tunneling](GTMT) dans l'arborescence des éléments.  "L'aperçu" exprimé dans la convention d'affectation de noms indique le principe général des événements d'entrée qui veut que les événements routés d'aperçu \(tunneling\) soient déclenchés avant l'événement routé de propagation équivalent.  En outre, les événements routés d'entrée associés à une paire de tunneling et de propagation ont une logique de gestion distincte.  Si l'événement routé de tunneling\/d'aperçu est marqué comme étant géré par un écouteur d'événements, l'événement routé de propagation sera alors également marqué comme étant géré, avant même que le moindre écouteur de l'événement routé de propagation ne le reçoive.  Les événements routés de tunneling et de propagation sont des événements techniquement distincts, mais ils partagent délibérément la même instance de données d'événement pour permettre ce comportement.  
  
 La connexion entre les événements routés de tunneling et de propagation est établie par l'implémentation interne du mode de déclenchement par toute classe [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] donnée de ses propres événements routés déclarés. Cela s'applique à la paire d'événements routés d'entrée.  Toutefois, à moins que cette implémentation au niveau de la classe n'existe, aucune connexion n'est établie entre un événement routé de tunneling et un événement routé de propagation qui partagent le schéma d'affectation de noms : sans cette implémentation, ils deviendraient deux événements routés totalement distincts et ils ne seraient pas déclenchés dans l'ordre, ni ne partageraient les données d'événement.  
  
 Pour plus d'informations sur l'implémentation des paires d'événements routés d'entrée de tunneling\/propagation dans une classe personnalisée, consultez [Créer un événement routé personnalisé](../../../../docs/framework/wpf/advanced/how-to-create-a-custom-routed-event.md).  
  
<a name="Class_Handlers_and_Instance_Handlers"></a>   
## Gestionnaires de classe et gestionnaires d'instance  
 Les événements routés considèrent deux types d'écouteurs différents sur l'événement : des écouteurs de classe et des écouteurs d'instance.  Les écouteurs de classe existent parce que des types ont appelé une [!INCLUDE[TLA2#tla_api](../../../../includes/tla2sharptla-api-md.md)] <xref:System.Windows.EventManager> particulière, <xref:System.Windows.EventManager.RegisterClassHandler%2A>, dans leur constructeur statique, ou ont remplacé la méthode virtuelle d'un gestionnaire de classe d'une classe de base de l'élément.  Les écouteurs d'instance sont des instances de classe\/éléments particuliers où un ou plusieurs gestionnaires ont été liés pour cet événement routé par un appel de <xref:System.Windows.UIElement.AddHandler%2A>. Les événements routés [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] existants effectuent des appels de <xref:System.Windows.UIElement.AddHandler%2A> dans le cadre des implémentations ajouter {} et {} supprimer du wrapper d'événement [!INCLUDE[TLA#tla_clr](../../../../includes/tlasharptla-clr-md.md)] de l'événement qui est également la manière dont le mécanisme [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] simple de joindre des gestionnaires d'événements via une syntaxe d'attribut est activé.  Ainsi, même l'utilisation simple de [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] équivaut en fin de compte à un appel de <xref:System.Windows.UIElement.AddHandler%2A>.  
  
 Les éléments de l'arborescence d'éléments visuels sont vérifiés pour les implémentations de gestionnaire enregistrées.  Les gestionnaires sont potentiellement appelés tout au long de l'itinéraire, dans l'ordre inhérent au type de la stratégie de routage pour cet événement routé.  Par exemple, les événements routés de propagation appelleront en premier lieu les gestionnaires joints au même élément qui a déclenché l'événement routé.  Les événements routés sont ensuite "propagés" à l'élément parent suivant, et ainsi de suite, jusqu'à atteindre l'élément racine de l'application.  
  
 Du point de vue de l'élément racine dans un itinéraire de propagation, si la gestion de classe ou tout élément plus proche de la source de l'événement routé appelle des gestionnaires qui marquent les arguments d'événement comme étant gérés, les gestionnaires sur les éléments racine ne sont alors pas appelés, et l'itinéraire de l'événement est raccourci efficacement avant d'atteindre l'élément racine en question.  Toutefois, l'itinéraire n'est pas complètement interrompu car des gestionnaires peuvent être ajoutés à l'aide d'un conditionnel spécial exigeant qu'ils soient tout de même appelés, même si un gestionnaire de classe ou d'instance a marqué l'événement routé comme étant géré.  Ce thème est expliqué plus loin dans cette rubrique, dans [Ajout de gestionnaires d'instance déclenchés y compris si les événements sont marqués comme étant gérés](#AddingInstanceHandlersthatAreRaisedEvenWhenEventsareMarkedHandled).  
  
 À un niveau plus profond que l'itinéraire d'événement se trouvent également des gestionnaires de classe potentiellement multiples qui agissent sur toute instance donnée d'une classe.  Cela est dû au fait que le modèle de gestion de classe des événements routés active toutes les classes possibles dans une hiérarchie de classes afin d'enregistrer son propre gestionnaire de classe pour chaque événement routé.  Chaque gestionnaire de classe est ajouté dans un magasin interne et, lorsque l'itinéraire d'événement d'une application est construit, les gestionnaires de classe sont tous ajoutés à l'itinéraire d'événement.  Les gestionnaires de classe sont ajoutés à l'itinéraire de sorte que le gestionnaire de classe le plus dérivé soit appelé en premier et que les gestionnaires de chaque classe de base consécutive soient appelés ensuite.  En général, les gestionnaires de classe ne sont pas enregistrés ; ainsi répondent\-ils également aux événements routés qui ont déjà été marqués comme étant gérés.  Par conséquent, ce mécanisme de gestion de classe active l'un de deux choix suivants :  
  
-   Les classes dérivées peuvent compléter la gestion de classe héritée de la classe de base en ajoutant un gestionnaire qui ne marque pas l'événement routé géré, parce que le gestionnaire de classe de base sera appelé quelque temps après le gestionnaire de classe dérivé.  
  
-   Les classes dérivées peuvent remplacer la gestion de classe à partir de la classe de base en ajoutant un gestionnaire de classe qui marque l'événement routé géré.  Vous devez être prudent avec cette approche, parce qu'elle est susceptible de modifier la conception du contrôle de base prévu dans des zones telles que l'apparence visuelle, la logique d'état ainsi que la gestion des entrées et des commandes.  
  
<a name="Class_Handling_of_Routed_Events"></a>   
## Gestion de classe des événements routés par les classes de base de contrôle  
 Sur chaque nœud d'élément donné dans un itinéraire d'événement, les écouteurs de classe ont la possibilité de répondre à l'événement routé avant le moindre écouteur d'instance sur l'élément.  C'est pourquoi les gestionnaires de classe sont parfois utilisés pour supprimer des événements routés qu'une implémentation de classe de contrôle particulière ne souhaite pas propager davantage, ou pour effectuer une gestion spéciale de l'événement routé qui compte parmi les fonctionnalités de la classe.  Par exemple, une classe peut déclencher son propre événement spécifique qui contient davantage de détails sur la signification de la condition d'entrée d'un utilisateur dans le contexte de cette classe particulière.  L'implémentation de classe peut ensuite marquer l'événement routé plus général comme étant géré.  Les gestionnaires de classe sont généralement ajoutés de sorte qu'ils ne soient pas appelés pour les événements routés où les données d'événement partagées ont déjà été marquées comme étant gérées ; toutefois, pour les cas atypiques, il existe également une signature <xref:System.Windows.EventManager.RegisterClassHandler%28System.Type%2CSystem.Windows.RoutedEvent%2CSystem.Delegate%2CSystem.Boolean%29> qui enregistre les gestionnaires de classe à appeler même si les événements routés sont marqués comme étant gérés.  
  
### Virtuels de gestionnaire de classe  
 Certains éléments, en particulier les éléments de base tels que <xref:System.Windows.UIElement>, exposent les méthodes virtuelles "On\*Event" et "OnPreview\*Event" qui correspondent à leur liste d'événements routés publics.  Ces méthodes virtuelles peuvent être remplacées en vue d'implémenter un gestionnaire de classe pour l'événement routé en question.  Les classes d'élément de base enregistrent ces méthodes virtuelles comme leur gestionnaire de classe pour chacun de ces événements routés à l'aide de <xref:System.Windows.EventManager.RegisterClassHandler%28System.Type%2CSystem.Windows.RoutedEvent%2CSystem.Delegate%2CSystem.Boolean%29>, comme décrit précédemment.  Les méthodes virtuelles On\*Event facilitent considérablement l'implémentation de la gestion de classe pour les événements routés pertinents, sans nécessiter aucune initialisation spéciale dans les constructeurs statiques pour chaque type.  Par exemple, vous pouvez ajouter la gestion de classe pour l'événement <xref:System.Windows.UIElement.DragEnter> dans toute classe dérivée <xref:System.Windows.UIElement> en remplaçant la méthode virtuelle <xref:System.Windows.UIElement.OnDragEnter%2A>.  Dans ce remplacement, vous pouvez gérer l'événement routé, déclencher d'autres événements, initialiser la logique spécifique à la classe susceptible de modifier les propriétés de l'élément sur les instances, ou toute combinaison de ces actions.  En règle générale, dans ce type de modifications, vous devez appeler l'implémentation de base même si vous marquez l'événement géré.  Il est vivement recommandé d'appeler l'implémentation de base car la méthode virtuelle se trouve sur la classe de base.  Le modèle virtuel protégé standard mis en œuvre pour appeler les implémentations de base à partir de chaque virtuel remplace essentiellement et met en parallèle un mécanisme similaire natif à la gestion des classes d'événements routés, qui permet d'appeler les gestionnaires de toutes les classes d'une hiérarchie sur n'importe quelle instance donnée, en commençant par le gestionnaire de classe le plus dérivé avant de passer au gestionnaire de classe de base.  Vous ne devez omettre l'appel à l'implémentation de base que si votre classe exige délibérément de modifier la logique de gestion de classe de base.  L'appel de l'implémentation de base avant ou après le remplacement du code dépendra de la nature de votre implémentation.  
  
#### Gestion de classe d'événement d'entrée  
 Les méthodes virtuelles des gestionnaires de classe sont toutes enregistrées de sorte qu'elles ne soient appelées que dans les cas où des données d'événement partagées ne sont pas déjà marquées comme étant gérées.  En outre, pour les événements d'entrée uniquement, les versions de tunneling et de propagation sont généralement déclenchées dans l'ordre et partagent les données d'événement.  Par conséquent, dans le cas d'une paire de gestionnaires de classe d'événements d'entrée donnée, dont l'un des gestionnaires correspond à une version de tunneling et l'autre à une version de propagation, il est possible que vous ne souhaitiez pas marquer l'événement géré immédiatement.  Si vous implémentez la méthode virtuelle de gestion de classe par tunneling pour marquer l'événement géré, alors le gestionnaire de classe de propagation ne pourra pas être appelé \(de même que tout gestionnaire d'instance enregistré normalement pour l'événement de tunneling ou de propagation\).  
  
 Une fois que la gestion de classe sur un nœud est terminée, les écouteurs d'instance sont considérés.  
  
<a name="AddingInstanceHandlersthatAreRaisedEvenWhenEventsareMarkedHandled"></a>   
## Ajout des gestionnaires d'instance déclenchés même lorsque les événements sont marqués comme étant gérés  
 La méthode <xref:System.Windows.UIElement.AddHandler%2A> fournit une surcharge particulière qui vous permet d'ajouter des gestionnaires qui seront appelés par le système d'événement chaque fois qu'un événement atteint l'élément de gestion dans l'itinéraire, même si quelque autre gestionnaire a déjà ajusté les données d'événement pour marquer cet événement comme étant géré.  Il ne s'agit pas de la procédure courante.  En général, les gestionnaires peuvent être écrits pour ajuster toutes les zones de code d'application susceptibles d'être influencées par un événement, indépendamment de l'emplacement de leur gestion dans une arborescence d'éléments, même si plusieurs résultats finaux sont souhaités.  En outre, un seul élément, en règle générale, doit véritablement répondre à cet événement, et la logique d'application appropriée s'est déjà exécutée.  Toutefois, la surcharge `handledEventsToo` est disponible pour les cas exceptionnels où quelque autre élément dans une arborescence d'éléments ou composition de contrôle a déjà marqué un événement comme étant géré, mais d'autres éléments situés plus haut ou plus bas dans l'arborescence d'éléments \(selon l'itinéraire\) souhaitent encore que leurs propres gestionnaires soient appelés.  
  
#### Quand marquer des événements gérés comme non gérés  
 En général, les événements routés marqués comme étant gérés ne doivent pas être marqués comme non gérés \(<xref:System.Windows.RoutedEventArgs.Handled%2A> a de nouveau la valeur `false`\), y compris par les gestionnaires qui agissent sur `handledEventsToo`.  Toutefois, certains événements d'entrée ont des représentations d'événement de niveaux supérieur et inférieur qui peuvent se chevaucher lorsque l'événement de niveau supérieur est vu à une position dans l'arborescence et l'événement de niveau inférieur à une autre position.  Par exemple, considérez le cas où un élément enfant écoute un événement de touche de niveau supérieur, tel que <xref:System.Windows.UIElement.TextInput>, pendant qu'un élément parent écoute un événement de niveau inférieur tel que <xref:System.Windows.UIElement.KeyDown>.  Si l'élément parent gère l'événement de niveau inférieur, l'événement de niveau supérieur peut être supprimé y compris dans l'élément enfant qui intuitivement aurait dû en premier avoir la possibilité de gérer l'événement.  
  
 Dans ce cas, il peut s'avérer nécessaire d'ajouter des gestionnaires aux éléments parents et enfants pour l'événement de niveau inférieur.  L'implémentation du gestionnaire d'éléments enfants peut marquer l'événement de niveau inférieur comme étant géré, tandis que l'implémentation du gestionnaire d'éléments parents le définirait encore comme étant non géré afin que les éléments situés plus haut dans l'arborescence \(ainsi que l'événement de niveau supérieur\) puissent avoir la possibilité de répondre.  Ce cas de figure devrait être assez rare.  
  
<a name="Deliberately_Suppressing_Input_Events_for_Control"></a>   
## Suppression délibérée d'événements d'entrée pour la composition de contrôle  
 La gestion de classe d'événements routés est principalement utilisée pour les événements d'entrée et les contrôles composés.  Un contrôle composé est par définition composé de plusieurs contrôles pratiques ou classes de base de contrôle.  L'auteur du contrôle souhaite souvent amalgamer tous les événements d'entrée possibles que chacun des sous\-composants peut déclencher, pour signaler l'intégralité du contrôle comme la source d'un événement singulier.  Dans certains cas, l'auteur du contrôle peut souhaiter supprimer totalement les événements des composants ou remplacer un événement défini par un composant qui véhicule davantage d'informations ou implique un comportement plus spécifique.  L'exemple canonique immédiatement visible à tout auteur de composant est la manière dont un [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] <xref:System.Windows.Controls.Button> gère tout événement de souris qui se résoudra en fin de compte en l'événement intuitif que possèdent tous les boutons : un événement <xref:System.Windows.Controls.Primitives.ButtonBase.Click>.  
  
 La classe de base <xref:System.Windows.Controls.Button> \(<xref:System.Windows.Controls.Primitives.ButtonBase>\) est dérivée de <xref:System.Windows.Controls.Control>, qui à son tour dérive de <xref:System.Windows.FrameworkElement> et <xref:System.Windows.UIElement>. En outre, une grande partie de l'infrastructure d'événement requise pour le traitement des entrées de commande est disponible au niveau de <xref:System.Windows.UIElement>.  En particulier, <xref:System.Windows.UIElement> traite les événements <xref:System.Windows.Input.Mouse> généraux qui gèrent les tests de positionnement du curseur de la souris dans ses limites et fournit des événements distincts pour les actions de bouton les plus courantes, telles que <xref:System.Windows.UIElement.MouseLeftButtonDown>.  <xref:System.Windows.UIElement> fournit également un <xref:System.Windows.UIElement.OnMouseLeftButtonDown%2A> virtuel vide en tant que gestionnaire de classe préenregistré pour <xref:System.Windows.UIElement.MouseLeftButtonDown>, et <xref:System.Windows.Controls.Primitives.ButtonBase> le remplace.  De la même façon, <xref:System.Windows.Controls.Primitives.ButtonBase> utilise des gestionnaires de classe pour <xref:System.Windows.UIElement.MouseLeftButtonUp>.  Dans les substitutions, auxquelles sont transmises les données d'événement, les implémentations marquent cette instance <xref:System.Windows.RoutedEventArgs> comme étant gérée en affectant à <xref:System.Windows.RoutedEventArgs.Handled%2A> la valeur `true`, et les mêmes données d'événement poursuivent leur chemin sur le reste de l'itinéraire vers d'autres gestionnaires de classe ainsi que des gestionnaires d'instance ou des accesseurs Set d'événement.  En outre, la substitution <xref:System.Windows.Controls.Primitives.ButtonBase.OnMouseLeftButtonUp%2A> déclenchera ensuite l'événement <xref:System.Windows.Controls.Primitives.ButtonBase.Click>.  Le résultat final pour la plupart des écouteurs sera que les événements <xref:System.Windows.UIElement.MouseLeftButtonDown> et <xref:System.Windows.UIElement.MouseLeftButtonUp> "disparaissent" et sont remplacés par <xref:System.Windows.Controls.Primitives.ButtonBase.Click>, événement plus significatif car on sait qu'il émane d'un bouton véritable et non de quelque pièce composite du bouton ou entièrement de quelque autre élément.  
  
<a name="WorkingAroundEventSuppressionByControls"></a>   
### Résolution des problèmes liés à la suppression d'événements par des contrôles  
 Ce comportement de suppression d'événement dans des contrôles individuels peut parfois interférer avec certaines intentions plus générales de la logique de gestion des événements de votre application.  Par exemple, si pour une raison quelconque votre application avait un gestionnaire pour <xref:System.Windows.UIElement.MouseLeftButtonDown>, qui se situait au niveau de l'élément racine de l'application, vous avez pu observer que tout clic avec la souris sur un bouton n'appelait pas le gestionnaire <xref:System.Windows.UIElement.MouseLeftButtonDown> ni le gestionnaire <xref:System.Windows.UIElement.MouseLeftButtonUp> au niveau de la racine.  L'événement proprement dit s'est bien propagé \(encore une fois, les itinéraires d'événement ne sont pas véritablement terminés, mais le système d'événement routé modifie leur comportement d'appel de gestionnaire après qu'ils ont été marqués comme étant gérés\).  Lorsque l'événement routé a atteint le bouton, la gestion de classe <xref:System.Windows.Controls.Primitives.ButtonBase> a marqué <xref:System.Windows.UIElement.MouseLeftButtonDown> comme étant géré car il a souhaité remplacer l'événement <xref:System.Windows.Controls.Primitives.ButtonBase.Click> par davantage de signification.  Par conséquent, tout gestionnaire <xref:System.Windows.UIElement.MouseLeftButtonDown> standard qui se trouve plus haut dans l'itinéraire ne sera pas appelé.  Deux techniques vous permettent de veiller à ce que vos gestionnaires soient appelés dans ce cas.  
  
 La première consiste à ajouter délibérément le gestionnaire à l'aide de la signature `handledEventsToo` de <xref:System.Windows.UIElement.AddHandler%28System.Windows.RoutedEvent%2CSystem.Delegate%2CSystem.Boolean%29>.  Cette technique implique une limitation, à savoir que le gestionnaire d'événements ne peut être joint qu'à partir du code et non d'une balise.  La syntaxe simple de la spécification du nom de gestionnaire d'événements comme une valeur d'attribut d'événement via le [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] n'active pas ce comportement.  
  
 La deuxième technique est uniquement valable pour les événements d'entrée, où les versions de tunneling et de propagation de l'événement routé sont associées par paire.  Pour ces événements routés, vous pouvez plutôt ajouter des gestionnaires à l'événement routé d'aperçu\/[de tunneling](GTMT) équivalent.  Cet événement routé créera un tunnel le long de l'itinéraire, en commençant à la racine, afin que le code de gestion de classe du bouton ne l'intercepte pas, présumant que vous avez joint le gestionnaire d'aperçu à quelque niveau d'élément ancêtre dans l'arborescence d'éléments de l'application.  Si vous choisissez cette approche, soyez prudent lors du marquage d'un événement d'aperçu géré.  Dans l'exemple donné avec <xref:System.Windows.UIElement.PreviewMouseLeftButtonDown> qui est géré au niveau de l'élément racine, si vous avez marqué l'événement comme <xref:System.Windows.RoutedEventArgs.Handled%2A> dans l'implémentation du gestionnaire, vous supprimeriez alors vraiment l'événement <xref:System.Windows.Controls.Primitives.ButtonBase.Click>.  Ce comportement n'est généralement pas souhaitable.  
  
## Voir aussi  
 <xref:System.Windows.EventManager>   
 [Aperçu des événements](../../../../docs/framework/wpf/advanced/preview-events.md)   
 [Créer un événement routé personnalisé](../../../../docs/framework/wpf/advanced/how-to-create-a-custom-routed-event.md)   
 [Vue d'ensemble des événements routés](../../../../docs/framework/wpf/advanced/routed-events-overview.md)