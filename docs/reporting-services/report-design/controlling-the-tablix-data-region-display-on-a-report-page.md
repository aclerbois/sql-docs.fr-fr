---
title: Contrôle de l’affichage de la région de données de tableau matriciel sur une page de rapport (Générateur de rapports version 3.0) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: f81c48cc-f038-4f57-988d-e9a3cbb46424
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 05ebc27d8ca5ac641819e01c0e9cf3e5cbc27457
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="controlling-the-tablix-data-region-display-on-a-report-page"></a>Contrôle de l'affichage de la région de données de tableau matriciel sur une page de rapport
Découvrez les propriétés que vous pouvez définir dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] pour une région de données de table, de matrice ou de liste, pour modifier la façon dont elle apparaît quand vous affichez le rapport.  
   
## <a name="controlling-the-appearance-of-data"></a>Contrôle de l'apparence des données  
Les régions de données de table, de matrice et de liste sont toutes des exemples de régions de données de *tableau matriciel* . Les fonctionnalités suivantes permettent de contrôler l'apparence d'une région de données de tableau matriciel :  
  
-   **Mise en forme de données.** Pour mettre en forme les données d'une table, d'une matrice ou d'une liste, définissez les propriétés de format de la zone de texte dans la cellule. Vous pouvez définir les propriétés pour plusieurs cellules à la fois. Pour mettre en forme les données dans un graphique, définissez les propriétés de mise en forme sur la série. Pour plus d’informations, consultez [Mise en forme des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md) et [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md).  
  
-   **Écriture d'expressions**. Pour plus d’informations, consultez [Utilisation d’expressions dans les rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md) et [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md).  
  
-   **Contrôle de l'ordre de tri**. Pour contrôler l’ordre de tri, vous définissez des expressions de tri sur la région de données. Pour contrôler l’ordre de tri pour les lignes et colonnes associées à un groupe, vous définissez des expressions de tri sur le groupe, y compris les groupes de détails. Vous pouvez également ajouter des boutons de tri interactifs pour permettre à l'utilisateur de trier une région de données de tableau matriciel ou ses groupes. Pour plus d’informations, consultez [Trier des données dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md).  
  
-   **Affichage d'un message en l'absence de données**. Lorsqu'aucune donnée n'existe pour un dataset du rapport au moment de l'exécution, vous pouvez écrire votre propre message à afficher au lieu de la région de données. Pour plus d’informations, consultez [Définir un message d’absence de données pour une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/set-a-no-data-message-for-a-data-region-report-builder-and-ssrs.md).  
  
-   **Masquage conditionnel de données**. Pour contrôler de manière conditionnelle l’affichage ou le masquage d’une région de données totale ou partielle, vous pouvez affecter à la propriété Hidden la valeur **True** ou une expression. Les expressions peuvent inclure des références aux paramètres du rapport. Vous pouvez également spécifier un élément de bascule, afin que l'utilisateur puisse décider d'afficher ou non les données de détail. Pour plus d’informations, consultez [Action d’exploration &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/drilldown-action-report-builder-and-ssrs.md).  
  
-   **Fusion de cellules.** Plusieurs cellules contiguës d'un tableau peuvent être combinées pour n'en former qu'une seule. Cette opération se nomme recouvrement de colonnes ou fusion de cellules. Les cellules peuvent être combinées uniquement verticalement ou horizontalement. Lors de la fusion, seules les données de la première cellule sont préservées. Les données des autres cellules sont supprimées. Les cellules fusionnées peuvent être fractionnées de sorte que les colonnes initiales soient rétablies. Pour plus d’informations, consultez [Fusionner des cellules dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/merge-cells-in-a-data-region-report-builder-and-ssrs.md).  
  
## <a name="controlling-tablix-data-region-position-and-expansion-on-a-page"></a>Contrôle de la position et de l'expansion d'une région de données de tableau matriciel sur une page  
 Les fonctionnalités suivantes permettent de contrôler la façon dont une région de données de tableau matriciel s'affiche dans un rapport rendu :  
  
-   **Contrôle de la position d'une région de données de tableau matriciel par rapport à d'autres éléments de rapport**. Une région de données de tableau matriciel peut être positionnée au-dessus, à côté, ou au-dessous d'autres éléments de rapport sur l'aire de conception du rapport. Au moment de l'exécution, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] développe la région de données de tableau matriciel autant que nécessaire pour accueillir les données extraites pour le dataset lié, décalant en fonction des besoin les éléments de rapport homologues. Pour ancrer un tableau matriciel à côté d’un autre élément de rapport, vous les rendez homologues et ajustez leurs positions relatives. Pour plus d’informations, consultez [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
-   **Modification de la direction d'expansion**. Pour contrôler si une région de données de tableau matriciel se développe de gauche à droite ou de droite à gauche à travers la page, utilisez la propriété Direction, accessible par le biais de la fenêtre Propriétés. Pour plus d’informations, consultez [Rendu des régions de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-data-regions-report-builder-and-ssrs.md).  
  
## <a name="controlling-how-a-tablix-data-region-renders-on-a-page"></a>Contrôle du rendu d'une région de données de tableau matriciel sur une page  
 La liste suivante décrit les façons dont vous pouvez contrôler l'apparence d'une région de données de tableau matriciel dans un rapport :  
  
-   **Contrôle de la pagination**. Pour contrôler la quantité de données affichées sur chaque page du rapport, vous pouvez définir des sauts de page sur les régions de données. Vous pouvez également définir des sauts de page sur des groupes. Les sauts de page peuvent affecter les performances de rendu à la demande en réduisant la quantité de données qui doivent être traitées sur chaque page. Pour plus d’informations, consultez [Pagination dans Reporting Services &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md) et [Ajouter un saut de page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-page-break-report-builder-and-ssrs.md).  
  
-   **Affichage de données de part et d'autre des en-têtes de ligne**. Vous n'êtes pas tenu d'afficher les en-têtes de ligne sur le côté de la région de données du tableau matriciel. Vous pouvez les placer entre les colonnes, de sorte que des colonnes de données soient placées avant ces en-têtes. Pour ce faire, modifiez la propriété GroupsBeforeRowHeaders de la matrice. Vous pouvez accéder à cette propriété par le biais de la fenêtre Propriétés. La valeur de cette propriété est un entier. La valeur 2, par exemple, affiche deux instances de groupes de données de colonne de la région de données avant d'afficher la colonne contenant les en-têtes de ligne.  
  
## <a name="controlling-how-tablix-row-and-column-groups-render"></a>Contrôle du rendu de groupes de lignes et de colonnes de tableau matriciel  
 Le rendu de groupes de régions de données de tableau matriciel dépend de la structure de groupe. Une région de données du tableau matriciel peut comporter quatre zones, comme indiqué dans l'illustration suivante :  
  
 ![Tablix data region areas](../../reporting-services/report-design/media/rs-tablixareas.gif "Tablix data region areas")  
  
 La zone de groupe de lignes et la zone de groupe de colonnes contiennent des en-têtes de groupe. Lorsqu'une région de données de tableau matriciel possède des en-têtes de groupe, vous contrôlez la façon dont les lignes et les colonnes se répètent en définissant des propriétés dans la page **Général** de la boîte de dialogue **Propriétés du tableau matriciel** .  
  
 Si une région de données de tableau matriciel possède uniquement un corps de tableau matriciel, il n'y a aucun en-tête de groupe. Il existe uniquement des membres de tableau matriciel statiques et dynamiques. Un membre statique s'affiche une fois pour un groupe de lignes ou de colonnes de tableau matriciel. Un membre dynamique se répète une fois pour chaque valeur de groupe unique. Par exemple, dans une région de données de tableau matriciel qui affiche une commande, les noms de colonnes dans la commande peuvent être affichés sur un membre de ligne statique. Chaque ligne de la commande est affichée sur un membre de ligne dynamique.  
  
 Vous pouvez contrôler le rendu d'un membre de tableau matriciel en définissant des propriétés dans le volet Propriétés. Pour plus d’informations, consultez « Mode avancé » dans [Volet de regroupement &#40;Générateur de rapports&#41;](../../reporting-services/report-design/grouping-pane-report-builder.md).  
  
 La liste suivante décrit les façons dont vous pouvez contrôler l'apparence d'une région de données de tableau matriciel dans un rapport :  
  
-   **En-têtes de ligne et de colonne se répétant sur plusieurs pages**. Vous pouvez afficher des en-têtes de ligne et de colonne sur chaque page faisant partie de la région de données du tableau matriciel. Pour plus d’informations, consultez [Afficher des en-têtes de ligne et de colonne sur plusieurs pages &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/display-row-and-column-headers-on-multiple-pages-report-builder-and-ssrs.md).  
  
-   **En-têtes de ligne et de colonne visibles dans la vue lors du défilement**. Vous pouvez décider de conserver les en-têtes de ligne et de colonne dans la vue lorsque vous faites défiler un rapport à l'aide d'un navigateur. Pour plus d’informations, consultez [Laisser les en-têtes visibles lors du défilement d’un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/keep-headers-visible-when-scrolling-through-a-report-report-builder-and-ssrs.md).  
  
 Pour plus d’informations sur la façon dont l’exportation d’un rapport dans différents formats influe sur le rendu d’une région de données de tableau matriciel sur une page, consultez [Comportements de rendu &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Liaison de plusieurs régions de données à un même dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)   
 [Régions de données imbriquées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md)   
 [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md)   
 [Contrôle des sauts de page, des en-têtes, des colonnes et des lignes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/controlling-page-breaks-headings-columns-and-rows-report-builder-and-ssrs.md)   
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md)   
 [Créer une matrice](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)   
 [Créer des factures et des formulaires avec des listes](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
