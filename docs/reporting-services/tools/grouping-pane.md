---
title: Volet de regroupement | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: tools
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- "10033"
- sql13.rtp.rptdesigner.group.f1
helpviewer_keywords:
- Grouping Pane dialog box
ms.assetid: 8b4bd0b3-ec97-48f8-8bfb-82a53a2f35a1
caps.latest.revision: 22
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: af8410f48f43e656946930df78df2e519dc41154
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="grouping-pane"></a>Volet de regroupement
Quand vous concevez des rapports [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , le volet de regroupement affiche les groupes de lignes et les groupes de colonnes de la région de données de tableau matriciel sélectionnée. Le volet de regroupement n'est pas disponible pour les régions de données Graphique et Jauge. Le volet de regroupement se compose d’un volet Groupes de lignes et d’un volet Groupes de colonnes. Le volet de regroupement comporte deux modes : par défaut et avancé. Le mode par défaut affiche une vue hiérarchique des membres dynamiques pour les groupes de lignes et de colonnes. Le mode avancé affiche à la fois les membres statiques et dynamiques pour les groupes de lignes et de colonnes. Un groupe est un jeu de données nommé extrait d'un dataset de rapport qui est affiché sur une région de données. Les groupes sont organisés en hiérarchies qui incluent des membres statiques et dynamiques. Pour plus d’informations, consultez [Fonctionnement des groupes&#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
  ![ssrs_fyi_note](../../analysis-services/instances/install-windows/media/ssrs-fyi-note.png) Si le volet de regroupement n’est pas visible, dans le menu **Rapport** , cliquez sur **Regroupement**.
  
 Les cellules situées dans les zones de groupes de lignes et de colonnes peuvent être des membres statiques ou dynamiques d'un groupe. Les membres statiques ne sont utilisés qu'à une seule reprise par groupe et ne contiennent généralement que des étiquettes ou des totaux. Les membres dynamiques ne sont utilisés qu'à une seule reprise par instance de groupe et contiennent généralement les valeurs uniques de l'expression de groupe. Quand vous sélectionnez des cellules de tableau matriciel dans la zone de groupes de lignes ou de groupes de colonnes, le membre du groupe correspondant est sélectionné dans le volet Groupes de lignes ou Groupes de colonnes. Inversement, si vous sélectionnez des groupes dans le volet de regroupement, la cellule correspondante associée au membre du groupe est sélectionnée sur l'aire de conception. Pour plus d’informations sur les zones de groupes de lignes et de colonnes de tableau matriciel, consultez [Zones de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-areas-report-builder-and-ssrs.md).  
  
 Le volet de regroupement prend en charge les modes suivants :  
  
-   **Par défaut** : utilisez le mode par défaut pour ajouter, modifier ou supprimer des groupes. Vous pouvez ajouter des groupes parents, enfants et des groupes de détails en faisant glisser des champs du volet des données de rapport et en les insérant dans la hiérarchie des groupes. Pour ajouter un groupe adjacent, vous devez utiliser le raccourci **Ajouter un groupe** . Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md).  
  
-   **Avancé**: Utilisez le **Mode avancé** pour consulter tous les membres des groupes de lignes et colonnes, et pour définir des propriétés sur les membres statiques. Quand vous créez des groupes ou ajoutez des totaux, les propriétés qui contrôlent la manière dont la région de données de tableau matriciel génère les lignes et les colonnes sur chaque page de rapport sont définies automatiquement. Pour modifier ces propriétés manuellement, vous devez les définir sur le membre du tableau matriciel. Pour plus d’informations, consultez [Contrôle de l’affichage de la région de données de tableau matriciel sur une page de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md).  
  
## <a name="default-mode"></a>Mode par défaut  
 Dans le mode par défaut, les volets Groupes de lignes et Groupes de colonnes affichent une vue hiérarchique pour tous les groupes parents, enfants et adjacents. Un groupe enfant apparaît en retrait sous son groupe parent. Un groupe adjacent apparaît au même niveau de retrait que ses groupes frères. L'illustration suivante montre une région de données de tableau matriciel avec des groupes de lignes imbriqués, et des groupes de colonnes imbriqués et adjacents.  
  
 ![Tableau matriciel, groupes de lignes et de colonnes adjacentes imbriqués](../../reporting-services/report-design/media/rs-basictablixdesigngroupingpane.gif "Tableau matriciel, groupes de lignes et de colonnes adjacentes imbriqués")  
  
 Le volet de regroupement affiche les groupes de lignes et de colonnes correspondants. Dans l'illustration suivante, le groupe basé sur la sous-catégorie a été sélectionné dans le volet Groupes de lignes, et la cellule de regroupement [Subcat] est sélectionnée dans la région de données de tableau matriciel :  
  
 ![Volet de regroupement pour les groupes de lignes et de colonnes imbriqués](../../reporting-services/report-design/media/rs-basictablixdesigngroupingpanedefaultview.gif "Volet de regroupement pour les groupes de lignes et de colonnes imbriqués")  
  
 Dans le volet Groupes de lignes, le groupe basé sur la sous-catégorie est un enfant du groupe basé sur la catégorie. Dans le volet Groupes de colonnes, le groupe de pays/région est un enfant du groupe de géographie. Le groupe d'année et les groupes de pays/région sont des groupes adjacents.  
  
 Pour plus d’informations, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
## <a name="advanced-mode"></a>Mode avancé  
En Mode avancé, vous pouvez afficher tous les membres statiques et dynamiques d'un groupe. Quand vous sélectionnez un membre, la fenêtre Propriétés affiche les propriétés du **Membre du tableau matriciel**sélectionné.  
  
![ssrs_fyi_note](../../analysis-services/instances/install-windows/media/ssrs-fyi-note.png) Pour activer/désactiver le **Mode avancé**, cliquez avec le bouton droit sur la flèche vers le bas en regard du volet Groupes de colonnes, puis cliquez sur **Mode avancé**.  
  
Dans la plupart des cas, les propriétés qui contrôlent l'affichage des lignes et des colonnes de groupe statiques et dynamiques sont définies automatiquement lorsque vous créez un groupe ou ajoutez des totaux. 

Pour modifier les valeurs par défaut, vous devez sélectionner le membre de groupe dans le volet Groupes de lignes ou Groupes de colonnes, et modifier les valeurs dans la fenêtre Propriétés. Si le volet des propriétés n’est pas visible, dans le menu **Affichage** , cliquez sur **Propriétés** , ou appuyez sur **F4**.  Les propriétés suivantes sont disponibles :  
  
-   **FixedData**: propriété booléenne. Pour les en-têtes de ligne et de colonne externes. Fige la zone de groupe de lignes en cas de défilement vertical ou la zone de groupe de colonnes en cas de défilement horizontal dans un convertisseur tel que HTML.  
  
-   **HideIfNoRows**: Propriété booléenne. Pour les membres statiques uniquement. Si cette propriété est définie, Hidden et ToggleItem sont ignorés. Masque ce membre si la région de données de tableau matriciel ne contient aucune ligne de données.  
  
-   **KeepTogether**.  
  
-   **KeepWithGroup**. Propriété booléenne. Pour les membres de lignes statiques uniquement. Conserve, dans la mesure du possible, cette ligne avec le membre dynamique frère précédent ou suivant, s'il n'est pas masqué.  
  
-   **RepeatOnNewPage**. Propriété booléenne. Pour les membres de lignes statiques uniquement et quand KeepWithGroup n’a pas la valeur None. Répète, dans la mesure du possible, cette ligne statique sur chaque page qui comporte au moins une instance du membre dynamique spécifiée par KeepWithGroup.  
  
-   **Hidden**. Propriété booléenne. Indique si la ligne ou la colonne doit être masquée initialement.  
  
-   **ToggleItem.** Chaîne. Nom de la zone de texte à laquelle ajouter l'image bascule. Cette zone de texte doit être située dans la même étendue de groupe ou dans une étendue contenante.  
  
 Pour plus d’informations sur la manière dont ce comportement peut être contrôlé dans une région de données de tableau matriciel, consultez [Contrôle de l’affichage de régions de données de tableau matriciel sur une page de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md).  
  
 Chaque membre statique ne comporte pas un en-tête correspondant à une cellule sur l'aire de conception. Dans le volet de regroupement, la convention suivante indique si un membre statique n'a aucun en-tête :  
  
-   **Static** : indique un membre statique comportant une cellule d’en-tête.  
  
-   **(Static)** : indique un membre statique sans cellule d’en-tête, également appelé statique masqué.  
  
## <a name="see-also"></a> Voir aussi  
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)  
  
  
