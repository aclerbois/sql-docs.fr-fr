---
title: Ajouter un filtre (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 10ae54e7-0e8a-4dff-995d-05516c51d076
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: c7138aa01512abe74931e7e0fd3510a597baaa0e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-a-filter-report-builder-and-ssrs"></a>Ajouter un filtre (Générateur de rapports et SSRS)
  Ajoutez un filtre à un dataset, une région de données ou un groupe lorsque vous souhaitez inclure ou exclure des valeurs spécifiques pour des calculs ou l'affichage. Les filtres sont appliqués dans un premier temps au moment de l'exécution sur le dataset, puis sur la région de données, puis sur le groupe, dans l'ordre de haut en bas des hiérarchies de groupe. Dans une table, une matrice ou une liste, les filtres des groupes de lignes, des groupes de colonnes et des groupes adjacents sont appliqués indépendamment. Dans un graphique, les filtres des groupes de catégories et des groupes de séries sont appliqués indépendamment.  
  
 Pour ajouter un filtre, vous devez spécifier une ou plusieurs équations de filtre. Une équation de filtre est composée d'une expression qui identifie les données que vous souhaitez filtrer, d'un opérateur et de la valeur de comparaison. Les types de données des données filtrées et de la valeur doivent correspondre. Le tri sur des valeurs agrégées n'est pas pris en charge pour les datasets ou les régions de données.  
  
 Pour filtrer des points de données dans un graphique, vous pouvez définir un filtre sur un groupe de catégories ou un groupe de séries. Par défaut, le graphique utilise la fonction intégrée Sum pour agréger les valeurs appartenant au même groupe en un point de données individuel dans la série. Si vous modifiez la fonction d'agrégation d'une série, vous devez modifier la fonction d'agrégation dans l'expression de filtre.  
  
 Pour plus d’informations sur le filtrage de datasets incorporés et partagés, consultez [Ajouter un filtre à un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-set-a-filter-on-a-data-region"></a>Pour définir un filtre sur une région de données  
  
1.  Ouvrez un rapport en mode **Conception** .  
  
2.  Sur l’aire de conception, sélectionnez la région de données, puis cliquez avec le bouton droit sur *\<Propriétés de **région_de_données>***. Pour une jauge, sélectionnez **Propriétés du panneau de jauge**. La boîte de dialogue *\<***Propriétés de région_de_données>** s’ouvre.  
  
    > [!NOTE]  
    >  Dans une région de données de tableau matriciel, cliquez avec le bouton droit sur la cellule d’angle ou sur une poignée de ligne ou de colonne, puis cliquez sur **Propriétés du tableau matriciel**.  
  
3.  Cliquez sur **Filtres**. La liste actuelle des équations de filtre s'affiche. Par défaut, elle est vide.  
  
4.  Cliquez sur **Ajouter**. Une nouvelle équation de filtre vierge apparaît.  
  
5.  Dans **Expression**, tapez ou sélectionnez l'expression pour le champ à filtrer. Pour modifier l’expression, cliquez sur le bouton d’expression (*fx*).  
  
6.  Dans la liste déroulante, sélectionnez le type de données correspondant au type de données de l'expression que vous avez créée à l'étape 5.  
  
7.  Dans la zone **Opérateur** , sélectionnez l'opérateur que le filtre doit utiliser pour comparer les valeurs des zones **Expression** et **Valeur** . L'opérateur sélectionné détermine le nombre de valeurs utilisées pour l'étape suivante.  
  
8.  Dans la zone **Valeur** , tapez l'expression ou la valeur en fonction de laquelle le filtre doit évaluer la valeur indiquée dans **Expression**.  
  
     Pour obtenir des exemples d’équations de filtre, consultez [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-set-a-filter-on-a-tablix-row-or-column-group"></a>Pour ajouter un filtre à un groupe de lignes ou de colonnes de tableau matriciel  
  
1.  Ouvrez un rapport en mode **Conception** .  
  
2.  Sur l'aire de conception, cliquez avec le bouton droit sur la région de données de table, de matrice ou de liste pour la sélectionner. Le volet Regroupement affiche les groupes de l'élément sélectionné.  
  
3.  Dans le volet Regroupement, cliquez avec le bouton droit sur le groupe, puis cliquez sur **Modifier le groupe**. La boîte de dialogue **Groupe de tableaux matriciels** s'affiche.  
  
4.  Cliquez sur **Filtres**. La liste actuelle des équations de filtre s'affiche. Par défaut, elle est vide.  
  
5.  Cliquez sur **Ajouter**. Une nouvelle équation de filtre vierge apparaît.  
  
6.  Dans **Expression**, tapez ou sélectionnez l'expression pour le champ à filtrer. Pour modifier l’expression, cliquez sur le bouton d’expression (*fx*).  
  
7.  Dans la liste déroulante, sélectionnez le type de données correspondant au type de données de l'expression que vous avez créée à l'étape 5.  
  
8.  Dans la zone **Opérateur** , sélectionnez l'opérateur que le filtre doit utiliser pour comparer les valeurs des zones **Expression** et **Valeur** . L'opérateur sélectionné détermine le nombre de valeurs utilisées pour l'étape suivante.  
  
9. Dans la zone **Valeur** , tapez l'expression ou la valeur en fonction de laquelle le filtre doit évaluer la valeur indiquée dans **Expression**.  
  
     Pour obtenir des exemples d’équations de filtre, consultez [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-set-a-filter-on-a-chart-category-group"></a>Pour définir un filtre sur un groupe de catégories de graphique.  
  
1.  Ouvrez un rapport en mode **Conception** .  
  
2.  Sur l'aire de conception, cliquez deux fois sur le graphique pour appeler les zones de dépôt des champs de données, de série et de catégorie.  
  
3.  Cliquez avec le bouton droit sur un champ de la zone de dépôt du champ de catégorie, puis sélectionnez **Propriétés du groupe de catégories**.  
  
4.  Cliquez sur **Filtres**. La liste actuelle des équations de filtre s'affiche. Par défaut, elle est vide.  
  
5.  Cliquez sur **Ajouter**. Une nouvelle équation de filtre vierge apparaît.  
  
6.  Dans **Expression**, tapez ou sélectionnez l'expression pour le champ à filtrer. Pour modifier l’expression, cliquez sur le bouton d’expression (*fx*).  
  
7.  Dans la liste déroulante, sélectionnez le type de données correspondant au type de données de l'expression que vous avez créée à l'étape 5.  
  
8.  Dans la zone **Opérateur** , sélectionnez l'opérateur que le filtre doit utiliser pour comparer les valeurs des zones **Expression** et **Valeur** . L'opérateur sélectionné détermine le nombre de valeurs utilisées pour l'étape suivante.  
  
9. Dans la zone **Valeur** , tapez l'expression ou la valeur en fonction de laquelle le filtre doit évaluer la valeur indiquée dans **Expression**.  
  
     Pour obtenir des exemples d’équations de filtre, consultez [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-set-a-filter-on-a-chart-series-group"></a>Pour définir un filtre sur un groupe de série de graphique.  
  
1.  Ouvrez un rapport en mode **Conception** .  
  
2.  Sur l'aire de conception, cliquez deux fois sur le graphique pour appeler les zones de dépôt des champs de données, de série et de catégorie.  
  
3.  Cliquez avec le bouton droit sur un champ de la zone de dépôt du champ de série, puis sélectionnez **Propriétés du groupe de séries**.  
  
4.  Cliquez sur **Filtres**. La liste actuelle des équations de filtre s'affiche. Par défaut, elle est vide.  
  
5.  Cliquez sur **Ajouter**. Une nouvelle équation de filtre vierge apparaît.  
  
6.  Dans **Expression**, tapez ou sélectionnez l'expression pour le champ à filtrer. Pour modifier l’expression, cliquez sur le bouton d’expression (*fx*).  
  
7.  Dans la liste déroulante, sélectionnez le type de données correspondant au type de données de l'expression que vous avez créée à l'étape 5.  
  
8.  Dans la zone **Opérateur** , sélectionnez l'opérateur que le filtre doit utiliser pour comparer les valeurs des zones **Expression** et **Valeur** . L'opérateur sélectionné détermine le nombre de valeurs utilisées pour l'étape suivante.  
  
9. Dans la zone **Valeur** , tapez l'expression ou la valeur en fonction de laquelle le filtre doit évaluer la valeur indiquée dans **Expression**.  
  
     Pour obtenir des exemples d’équations de filtre, consultez [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Jauges &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
  
