---
title: Ajouter ou supprimer un groupe dans une région de données (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4de53c3c-c6fc-49ce-b692-3609fc0b3ec5
caps.latest.revision: 10
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: f6109f996e719784a393fa7fc7864ecbcf8e0f8d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs"></a>Ajouter ou supprimer un groupe dans une région de données (Générateur de rapports et SSRS)
Dans les rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , ajoutez un groupe à une région de données pour organiser les données selon une valeur spécifique ou un ensemble d’expressions à des fins d’affichage et de calculs. Un groupe porte un nom et possède une expression qui identifient les données d'un dataset qui appartient à ce groupe. Pour plus d’informations sur les groupes, consultez [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
 Dans une région de données de tableau matriciel, cliquez sur la table, la matrice ou la liste pour afficher le volet de regroupement. Faites glisser des champs du dataset vers les volets Groupe de lignes et Groupe de colonnes pour créer des groupes parents ou enfants. Cliquez avec le bouton droit sur un groupe existant pour ajouter un groupe adjacent. Par définition, le groupe de détails est le groupe le plus profond et ne peut être ajouté qu'en tant que groupe enfant. Cliquez avec le bouton droit sur un groupe existant pour le supprimer. Les lignes et les colonnes dans lesquelles des valeurs de groupe seront affichées sont automatiquement ajoutées. Pour plus d’informations, consultez [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md).  
  
 Dans une région de données de graphique, cliquez sur le graphique pour afficher les zones de dépôt. Créez des groupes en faisant glisser des champs du dataset vers les zones de dépôt des catégories et des séries. Pour ajouter des groupes imbriqués, ajoutez plusieurs champs à la zone de dépôt.  
  
 Ces groupes ne sont pas définis dans une jauge par défaut. Le comportement par défaut de la jauge consiste à agréger toutes les valeurs du champ spécifié en une seule valeur qui est indiquée sur la jauge. Pour plus d’informations, consultez [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-add-a-parent-or-child-row-or-column-group-to-a-tablix-data-region"></a>Pour ajouter un groupe de lignes ou de colonnes parent ou enfant à une région de données de tableau matriciel  
  
1.  Faites glisser un champ du volet **Données du rapport** vers le volet **Groupes de lignes** ou le volet **Groupes de colonnes** .  
  
    > [!NOTE]  
    >  Si le volet de regroupement n’est pas visible, sous l’onglet Affichage, cliquez sur **Regroupement**.  
  
2.  Déplacez le champ situé au-dessus ou en dessous de la hiérarchie de groupes à l'aide de la barre de repère pour placer le groupe en tant que groupe parent ou groupe enfant dans un groupe existant.  
  
     Le groupe est ajouté avec un nom, une expression de groupe et une expression de tri par défaut reposant sur le nom du champ.  
  
## <a name="to-add-an-adjacent-row-or-column-group-to-a-tablix-data-region"></a>Pour ajouter un groupe de lignes ou de colonnes adjacent à une région de données de tableau matriciel  
  
1.  Dans le volet de regroupement, cliquez avec le bouton droit sur un groupe homologue du groupe que vous souhaitez ajouter. Cliquez sur **Ajouter un groupe**, puis sur **Adjacent avant** ou sur **Adjacent après** pour spécifier l’emplacement auquel ajouter le groupe. La boîte de dialogue **Groupe de tableaux matriciels** s'affiche.  
  
2.  Dans **Nom**, tapez le nom du groupe.  
  
3.  Dans **Expression Groupe**, tapez une expression ou cliquez sur le bouton Expression (**fx**) pour créer une expression.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Un nouveau groupe est ajouté au volet de regroupement et une ligne ou une colonne dans laquelle des valeurs de groupe seront affichées est ajoutée à la région de données de tableau matriciel sur l'aire de conception.  
  
## <a name="to-add-a-details-group-to-a-tablix-data-region"></a>Pour ajouter un groupe de détails à une région de données de tableau matriciel  
  
1.  Dans le volet de regroupement, cliquez avec le bouton droit sur un groupe qui est le groupe enfant le plus profond, mais pas le groupe **Détails** . Cliquez sur **Ajouter un groupe**, puis sur **Groupe enfant**. La boîte de dialogue **Groupe de tableaux matriciels** s'affiche.  
  
2.  Dans **Expression Groupe**, laissez l'expression vide. Un groupe de détails ne possède aucune expression.  
  
3.  Sélectionnez **Afficher les données de détail**.  
  
4.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Un nouveau groupe de détails est ajouté en tant que groupe enfant dans le volet Regroupement et le descripteur de ligne du groupe que vous avez sélectionné au cours de l'étape 1 affiche l'icône du groupe de détails. Pour plus d’informations sur les descripteurs, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
## <a name="to-edit-a-row-or-column-group-in-a-tablix-data-region"></a>Pour modifier un groupe de lignes ou de colonnes dans une région de données de tableau matriciel  
  
1.  Sur l'aire de conception du rapport, cliquez n'importe où dans la région de données de tableau matriciel pour la sélectionner. Le volet Regroupement affiche les groupes de lignes et de colonnes.  
  
2.  Cliquez avec le bouton droit sur le groupe, puis cliquez sur **Propriétés du groupe**.  
  
3.  Dans **Nom**, tapez le nom du groupe.  
  
4.  Dans **Expressions Groupe**, tapez ou sélectionnez une expression simple ou cliquez sur le bouton Expression (**fx**) pour créer une expression de groupe.  
  
5.  Cliquez sur **Ajouter** pour créer d’autres expressions. Toutes les expressions que vous spécifiez sont combinées à l'aide d'une logique AND pour spécifier les données de ce groupe.  
  
6.  (Facultatif) Cliquez sur **Sauts de page** pour définir des options de saut de page.  
  
7.  (Facultatif) Cliquez sur **Tri** pour sélectionner ou taper des expressions qui spécifient l’ordre de tri des valeurs du groupe.  
  
8.  (Facultatif) Cliquez sur **Visibilité** pour sélectionner les options de visibilité de l’élément.  
  
9. (Facultatif) Cliquez sur **Filtres** pour définir les filtres de ce groupe.  
  
10. (Facultatif) Cliquez sur **Variables** pour définir les variables étendues à ce groupe et accessibles à partir de tous les groupes enfants.  
  
11. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-delete-a-group-from-a-tablix-data-region"></a>Pour supprimer un groupe d’une région de données de tableau matriciel  
  
1.  Dans le volet de regroupement, cliquez avec le bouton droit sur le groupe, puis cliquez sur **Supprimer le groupe**.  
  
2.  Dans la boîte de dialogue **Supprimer le groupe** , sélectionnez l’une des options suivantes :  
  
    -   **Supprimer le groupe et les colonnes et lignes associées** Choisissez cette option pour supprimer la définition de groupe et toutes les lignes associées qui affichent des données de groupe. Pour les groupes de détails, si une même ligne appartient à la fois aux données de détail et de groupe, seules les lignes de données de détail sont supprimées.  
  
    -   **Supprimer le groupe uniquement** Choisissez cette option pour conserver la structure de la région de données de tableau matriciel et ne supprimer que la définition de groupe.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-delete-a-details-group-from-a-tablix-data-region"></a>Pour supprimer un groupe de détails d’une région de données de tableau matriciel  
  
1.  Dans le volet de regroupement, cliquez avec le bouton droit sur le groupe de détails, puis cliquez sur **Supprimer le groupe**.  
  
2.  Dans la boîte de dialogue **Supprimer le groupe** , sélectionnez l’une des options suivantes :  
  
    -   **Supprimer le groupe et les colonnes et lignes associées** Choisissez cette option pour supprimer la définition de groupe et toutes les lignes associées qui affichent des données de groupe. Pour les groupes de détails, si une même ligne appartient à la fois aux données de détail et de groupe, seules les lignes de données de détail sont supprimées.  
  
    -   **Supprimer le groupe uniquement** Choisissez cette option pour conserver la structure de la région de données de tableau matriciel et ne supprimer que la définition de groupe.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Le groupe de détails est supprimé.  
  
    > [!NOTE]  
    >  Lorsque vous supprimez une ligne de détails, vérifiez que l'expression dans chaque cellule spécifie, le cas échéant, une expression d'agrégation. Si nécessaire, modifiez l'expression pour spécifier les fonctions d'agrégation appropriées.  
  
## <a name="see-also"></a> Voir aussi  
 [Références à des collections de variables de rapport et de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md)   
 [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)   
 [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md)   
 [Matrices &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)   
 [Listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
