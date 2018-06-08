---
title: Mode Création de rapport (Générateur de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- "10440"
- "10426"
- "10439"
- "10434"
- "10438"
- "10436"
helpviewer_keywords:
- reports, creating
- user interface
- overview of Report Builder
ms.assetid: 1544472c-2803-448d-af52-e901cb457a00
caps.latest.revision: 23
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 38b8c4c77f81a9454ba21c4e3b4fbc7fa8de7ff3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-design-view-report-builder"></a>Vue Conception de rapport (Générateur de rapports)
  La fenêtre du Générateur de rapports est conçu pour vous aider à organiser facilement vos ressources de rapport et à générer rapidement les rapports paginés dont vous avez besoin. L’aire de conception se trouve au centre de la fenêtre, avec le ruban et les volets autour d’elle. L'aire de conception est l'espace où vous ajoutez et organisez vos éléments de rapport. Cet article décrit les volets que vous utilisez pour ajouter, sélectionner et organiser vos ressources de rapport, ainsi que pour modifier les propriétés des éléments de rapport.  
  
 ![Mode Création du Générateur de rapports](../../reporting-services/report-builder/media/ssrb-designview.png "Mode Création du Générateur de rapports")  
  
1.  Ruban  
  
2.  [Volet Paramètres](#Ribbon)  
  
3.  [Bibliothèque de parties de rapports](#ReptPartGallery)  
  
4.  [Propriétés, volet](#PropertiesPane)  
  
5.  [Aire de conception du rapport](#RptDesignSurface)  
  
6.  [Volet Données du rapport](#ReptDataPane)  
  
7.  [Volet de regroupement](#GroupPane)  
  
8.  Barre d’état du rapport actif  
  
##  <a name="Ribbon"></a> Volet Paramètres  
 Avec les paramètres de rapport, vous pouvez contrôler les données du rapport, interconnecter des rapports associés et varier la présentation des rapports. Le volet Paramètres fournit une disposition souple pour les paramètres du rapport.  
  
 En savoir plus sur le [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
  
##  <a name="RptDesignSurface"></a> Aire de conception de rapport  
 L'aire de conception de rapport du Générateur de rapports est la principale zone de travail pour concevoir vos rapports. Pour placer des éléments de rapport, tels que des régions de données, des sous-rapports, des zones de texte, des images, des rectangles et des lignes dans votre rapport, vous devez les ajouter en les faisant glisser du ruban ou de la bibliothèque de parties de rapports vers l’aire de conception. À partir de là, vous pouvez ajouter des groupes, des expressions, des paramètres, des filtres, des actions, une visibilité et une mise en forme à vos éléments de rapport.  
  
 Vous pouvez également modifier les éléments suivants :  
  
-   les propriétés de corps du rapport, telles que la bordure et la couleur de remplissage, en cliquant avec le bouton droit sur la zone blanche de l’aire de conception, en dehors de tout élément de rapport, puis en cliquant sur **Propriétés du corps**;  
  
-   les propriétés d’en-tête et de pied de page, telles que la bordure et la couleur de remplissage, en cliquant avec le bouton droit sur la zone blanche de l’aire de conception dans la zone d’en-tête ou de pied de page, en dehors de tout élément de rapport, puis en cliquant sur **Propriétés d’en-tête** ou **Propriétés du pied de page**;  
  
-   les propriétés du rapport lui-même, telles que la mise en page, en cliquant avec le bouton droit sur la zone grise située autour de l’aire de conception, puis en cliquant sur **Propriétés du rapport**;  
  
-   les propriétés des éléments de rapport en cliquant dessus avec le bouton droit, puis en cliquant sur **Propriétés**.  
  
 Pour plus d’informations sur l’utilisation du clavier pour manipuler des éléments sur l’aire de conception, consultez [Raccourcis clavier &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/keyboard-shortcuts-report-builder.md).  
  
### <a name="design-surface-size-and-print-area"></a>Taille et zone d'impression de l'aire de conception  
 La taille de l'aire de conception peut être différente de la zone d'impression de taille de page que vous spécifiez pour imprimer le rapport. Le fait de modifier la taille de l'aire de conception ne modifie pas la zone d'impression de votre rapport. Quelle que soit la taille que vous définissez pour la zone d'impression de votre rapport, la taille globale de l'aire de conception ne change pas. Pour plus d’informations, consultez [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
> [!TIP]  
>  Pour afficher la règle, sous l’onglet **Affichage**, cochez la case **Règle**.  
  
  
##  <a name="ReptDataPane"></a> The Report Data Pane  
 À partir du volet Données du rapport, vous définissez les données de rapport et les ressources de rapport dont vous avez besoin pour un rapport avant de concevoir la disposition de votre rapport. Vous pouvez, par exemple, y ajouter des sources de données, des datasets, des champs calculés, des paramètres de rapport et des images.  
  
 Après avoir ajouté des éléments dans le volet Données du rapport, faites glisser des champs vers des éléments de rapport dans l'aire de conception afin de déterminer où apparaissent les données dans le rapport.  
  
> [!TIP]  
>  Si vous faites glisser un champ du volet Données du rapport directement vers l'aire de conception de rapport au lieu de le placer dans une région de données telle qu'un tableau ou un graphique, lors de l'exécution du rapport, vous voyez s'afficher uniquement la première valeur des données dans ce champ.  
  
 Vous pouvez également faire glisser des champs prédéfinis du volet Données du rapport vers l'aire de conception de rapport. Une fois le rendu effectué, ces champs fournissent des informations sur les rapports, notamment le nom du rapport, le nombre total de pages qu'il contient et le numéro de la page affichée.  
  
 Certains éléments sont ajoutés automatiquement au volet Données du rapport lorsque vous effectuez un ajout dans l'aire de conception de rapport. Par exemple, si vous ajoutez une partie de rapport à partir de la bibliothèque de parties de rapports, et si la partie de rapport est une région de données, le dataset est ajouté automatiquement au volet Données du rapport. Pour plus d’informations, consultez [Parties de rapports et datasets dans le Générateur de rapports](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md). En outre, si vous incorporez une image dans votre rapport, elle est ajoutée au dossier Images dans le volet Données du rapport.  
  
> [!NOTE]  
>  Vous pouvez utiliser le bouton **Nouveau** pour ajouter un nouvel élément au volet Données du rapport. Vous pouvez ajouter au rapport plusieurs datasets issus d'une même source de données ou de différentes sources de données. Vous pouvez ajouter des datasets partagés à partir du serveur de rapports. Pour ajouter un nouveau dataset à partir de la même source de données, cliquez avec le bouton droit sur une source de données, puis cliquez sur **Ajouter un dataset**.  
  
 Pour plus d’informations sur les éléments du volet Données du rapport, consultez les rubriques suivantes :  
  
-   [Références à des champs Globals et Users prédéfinis &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md)  
  
-   [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)  
  
-   [Images &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/images-report-builder-and-ssrs.md)  
  
-   [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
  
-   [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
-   [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
  
  
##  <a name="ReptPartGallery"></a> Bibliothèque de parties de rapports  
 La façon la plus simple de créer un rapport est de rechercher une partie de rapport existante, par exemple un tableau ou un graphique, sur le serveur de rapports ou un serveur de rapports intégré à un site SharePoint.  
  
 Cliquez sur **Parties de rapports** sous l’onglet Insertion pour ouvrir la bibliothèque de parties de rapports. Vous pouvez y rechercher des parties de rapports à ajouter à votre rapport. Vous pouvez filtrer les parties de rapports en fonction de leur nom ou d'une partie de celui-ci, de leur créateur, de la personne qui a apporté la dernière modification ou du moment de cette dernière modification, de l'emplacement de stockage ou de leur type. Par exemple, vous pourriez rechercher tous les graphiques créés la semaine dernière par l'un de vos collègues.  
  
> [!NOTE]  
>  Pour afficher la bibliothèque de parties de rapports, vous devez être connecté à un serveur.  
  
 Vous pouvez afficher les résultats de la recherche sous la forme de miniatures ou d'une liste et les trier par nom, par dates de création et de modification, et par créateur. Pour plus d’informations, consultez [Publication de parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
  
##  <a name="PropertiesPane"></a> Le volet Propriétés (Générateur de rapports)  
 Chaque élément d’un rapport, notamment les régions de données, les images, les zones de texte et le corps du rapport lui-même, est associé à des propriétés. Par exemple, la propriété BorderColor d'une zone de texte affiche la valeur de couleur de la bordure de la zone de texte et la propriété PageSize du rapport affiche la taille de page du rapport.  
  
 Ces propriétés sont affichées dans le volet Propriétés. Les propriétés du volet varient en fonction de l'élément de rapport que vous sélectionnez.  
  
 Pour afficher le volet Propriétés, sous l'onglet Affichage, dans le groupe Afficher/Masquer, cliquez sur Propriétés.  
  
### <a name="changing-property-values"></a>Modification des valeurs de propriété  
 Dans le Générateur de rapports, vous pouvez modifier les propriétés des éléments de rapport de plusieurs façons :  
  
-   en cliquant sur des boutons et des listes dans le ruban ;  
  
-   en modifiant les paramètres dans les boîtes de dialogue ;  
  
-   en modifiant les valeurs de propriété dans le volet Propriétés.  
  
 Les propriétés les plus couramment utilisées sont disponibles dans les boîtes de dialogue et dans le ruban.  
  
 En fonction de la propriété, vous pouvez définir une valeur de propriété dans une liste déroulante, taper la valeur ou cliquer sur `<Expression>` pour créer une expression.  
  
### <a name="changing-the-properties-pane-view"></a>Modification de l'affichage du volet Propriétés  
 Par défaut, les propriétés affichées dans le volet Propriétés sont classées en grandes catégories, par exemple Action, Bordure, Remplissage, Police et Général. Un ensemble de propriétés est associé à chaque catégorie. Par exemple, les propriétés suivantes sont répertoriées dans la catégorie Police : Color, FontFamily, FontSize, FontStyle, FontWeight, LineHeight et TextDecoration. Si vous préférez, vous pouvez classer toutes les propriétés répertoriées dans le volet par ordre alphabétique. Les catégories sont ainsi supprimées et toutes les propriétés sont classées par ordre alphabétique, quelle que soit la catégorie.  
  
 Trois boutons figurent en haut du volet Propriétés : Catégorie, Alphabétiser et Pages de propriétés. Cliquez sur les boutons Catégorie et Alphabétiser pour basculer entre les affichages du volet Propriétés. Cliquez sur le bouton **Pages de propriétés** pour ouvrir la boîte de dialogue des propriétés pour un élément de rapport sélectionné.  
  
  
##  <a name="GroupPane"></a> Volet de regroupement (Générateur de rapports)  
 Les groupes sont utilisés pour hiérarchiser vos données de rapport de façon visuelle et pour calculer des totaux. Vous pouvez afficher les groupes de lignes et de colonnes d'une région de données sur l'aire de conception et également dans le volet de regroupement. Le volet de regroupement comprend deux volets : Groupes de lignes et Groupes de colonnes. Lorsque vous sélectionnez une région de données, le volet de regroupement affiche tous les groupes de cette région de données sous forme de liste hiérarchique : les groupes enfants apparaissent en retrait sous leurs groupes parents.  
  
 ![Groupes de lignes du Générateur de rapports](../../reporting-services/report-builder/media/ssrb-rowgroups.png "Groupes de lignes du Générateur de rapports")  
  
 Vous pouvez créer des groupes en faisant glisser des champs du volet Données du rapport et en les plaçant sur l'aire de conception ou dans le volet de regroupement. Dans le volet de regroupement, vous pouvez ajouter des groupes parents, enfants et adjacents, modifier les propriétés de groupe et supprimer des groupes.  
  
 Le volet de regroupement s’affiche par défaut, mais vous pouvez le fermer en décochant la case Volet de regroupement sous l’onglet Affichage. Le volet de regroupement n'est pas disponible pour les régions de données Graphique et Jauge.  
  
 Pour plus d’informations, consultez [Volet de regroupement &#40;Générateur de rapports&#41;](../../reporting-services/report-design/grouping-pane-report-builder.md) et [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
  
##  <a name="RunMode"></a> Affichage de l'aperçu de votre rapport en mode exécution  
 En mode création de rapport, vous n'utilisez pas des données réelles, mais une représentation des données indiquées par le nom ou l'expression du champ. Lorsque vous voulez afficher les données réelles dans le contexte du rapport que vous avez conçu, vous pouvez exécuter le rapport afin d'afficher un aperçu des données de la base de données sous-jacente dans la mise en page du rapport. Vous pouvez passer du mode Création au mode Exécution de votre rapport pour ajuster sa conception et voir les résultats immédiatement. Pour afficher un aperçu de votre rapport, cliquez sur **Exécuter** dans le groupe **Vues** dans le ruban.  
  
 Quand vous cliquez sur **Exécuter**, le Générateur de rapports se connecte aux sources de données du rapport, met en cache les données sur votre ordinateur, combine les données et la disposition, puis effectue le rendu du rapport dans la Visionneuse HTML. Vous pouvez exécuter le rapport aussi souvent que vous le souhaitez au fil de sa conception. Lorsque vous êtes satisfait du rapport, vous pouvez l'enregistrer sur le serveur de rapports, où les autres utilisateurs dotés des autorisations appropriées peuvent le visualiser.  
   
 En savoir plus sur [Aperçu des rapports dans le Générateur de rapports](../../reporting-services/report-builder/previewing-reports-in-report-builder.md).  
  
### <a name="running-a-report-with-parameters"></a>Exécution d'un rapport avec des paramètres  
 Lorsque vous exécutez votre rapport, celui-ci est traité automatiquement. Si le rapport contient des paramètres, ils doivent tous avoir des valeurs par défaut pour que le rapport puisse s'exécuter automatiquement. Si un paramètre ne possède pas de valeur par défaut, vous devez choisir une valeur pour chaque paramètre lors de l’exécution du rapport, puis cliquer sur **Afficher le rapport** sous l’onglet Exécuter. Pour plus d’informations, consultez [Report Parameters &#40;Report Builder and Report Designer&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
### <a name="print-preview"></a>Aperçu avant impression  
 Lorsque vous affichez un aperçu du rapport en mode exécution, le rapport ressemble à un rapport généré en HTML. L'aperçu n'est pas en HTML, mais la présentation et la pagination du rapport sont similaires à un affichage HTML. Vous pouvez changer la vue pour afficher le rapport tel qu'il sera imprimé en activant le mode d'aperçu avant impression. Cliquez sur le bouton **Aperçu avant impression** sous l’onglet **Exécuter** . Le rapport s'affiche comme sur une véritable page. Ce que vous voyez ressemble au résultat que produisent les extensions de rendu Image et PDF. L’aperçu avant impression n’est pas une image ni un fichier PDF, mais la disposition et la pagination du rapport sont similaires à la sortie de ces formats.  
  
  
## <a name="see-also"></a> Voir aussi  
 [Recherche, affichage et gestion des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
 [Générateur de rapports dans SQL Server 2016](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  
  
