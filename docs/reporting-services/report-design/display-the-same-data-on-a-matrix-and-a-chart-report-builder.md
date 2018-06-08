---
title: Afficher les mêmes données dans une matrice et sur un graphique (Générateur de rapports) | Microsoft Docs
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
ms.assetid: 1262f283-9fc2-4bc1-9c79-457f7642abc7
caps.latest.revision: 6
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: b342fa9f55f0494d90f7e85aa7b09bb82b32ae2c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="display-the-same-data-on-a-matrix-and-a-chart-report-builder"></a>Afficher les mêmes données dans une matrice et sur un graphique (Générateur de rapports)
  Pour afficher les mêmes données dans une matrice et sur un graphique, vous devez définir des propriétés sur ces deux régions de données afin de spécifier le même dataset, ainsi que les mêmes expressions pour les filtres, les groupes, les tris et les données.  
  
 Étant donné que les deux régions de données auront le même ancêtre de données (à savoir le dataset du rapport), vous pouvez ajouter à la matrice un bouton de tri interactif qui permettra aux utilisateurs de modifier l'ordre de tri à la fois pour la matrice et le graphique. Pour plus d’informations, consultez [Ajouter un tri interactif à un tableau ou une matrice &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-interactive-sort-to-a-table-or-matrix-report-builder-and-ssrs.md).  
  
 Pour utiliser les valeurs de groupe de colonnes de matrice comme légende pour le graphique, vous devez spécifier les couleurs des données de série sur le graphique, puis utiliser les mêmes couleurs que les couleurs de remplissage pour l'arrière-plan des zones de texte de la cellule de matrice qui affiche les valeurs de groupe. Pour plus d’informations, consultez [Spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs.md).  
  
 Au moment de l'exécution, votre rapport peut paraître surchargé s'il contient un trop grand nombre de valeurs de groupe pour vos définitions de groupe. Il peut s'avérer nécessaire de filtrer des valeurs, de combiner des groupes ou d'ajuster le seuil pour que le graphique combine les groupes à votre place. Pour plus d’informations, consultez [Liaison de plusieurs régions de données à un même dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
### <a name="to-add-a-matrix-and-chart-to-display-the-same-data"></a>Pour ajouter une matrice et un graphique affichant les mêmes données  
  
1.  Ouvrez un rapport en mode Conception.  
  
2.  Sous l’onglet **Insérer** , dans le groupe **Régions de données** , cliquez sur **Matrice**, puis cliquez dans le corps du rapport ou dans un rectangle du corps du rapport. Une matrice est ajoutée au rapport.  
  
3.  Sous l’onglet **Insérer** , dans le groupe **Régions de données** , cliquez sur **Graphique**, puis sélectionnez le type de graphique. Un graphique est ajouté au rapport.  
  
4.  (Facultatif) Sous l’onglet **Insérer** , dans le groupe **Éléments de rapport** , cliquez sur **Rectangle**, puis cliquez sur le rapport. Un rectangle est ajouté au rapport. Faites glisser la matrice et le graphique des étapes 2 et 3 dans le rectangle.  
  
     Le fait de placer plusieurs régions de données dans le conteneur rectangle vous permet de contrôler plus facilement le rendu de la matrice et du graphique lors de la visualisation du rapport.  
  
     Aux étapes suivantes, vous allez choisir le même champ de dataset à afficher dans la matrice et dans le graphique.  
  
5.  À partir du volet Données du rapport, faites glisser un champ de dataset numérique vers la cellule de données de la matrice.  
  
     Par défaut, la fonction d’agrégation Sum est utilisée pour calculer la valeur de groupe. Si vous modifiez la fonction d'agrégation dans la matrice, vous devez également la modifier dans le graphique.  
  
6.  Dans la matrice, cliquez avec le bouton droit sur la cellule contenant les données, cliquez sur **Propriétés de la zone de texte**, puis sur **Nombre**. Choisissez un format approprié pour la valeur de champ de dataset.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
8.  Faites glisser le même champ de dataset que celui sélectionné à l’étape 3 vers la zone **Valeurs** sur le graphique.  
  
9. Sur le graphique, cliquez avec le bouton droit sur l’axe Y, cliquez sur **Propriétés de l’axe**, puis sur **Nombre**. Choisissez pour les données le même format que celui sélectionné à l'étape 4.  
  
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Aux étapes suivantes, vous devez affecter au groupe de lignes de la matrice et au groupe de séries du graphique la même expression, ainsi que définir l'ordre de tri du groupe de séries du graphique.  
  
11. À partir du volet Données du rapport, faites glisser vers le volet Groupe de lignes le champ de dataset selon lequel vous souhaitez effectuer le regroupement des lignes de la matrice.  
  
     Par défaut, le groupe de lignes de la matrice ajoute une expression de tri identique à l'expression de groupe.  
  
12. Faites glisser le même champ de dataset que celui utilisé à l’étape 9 vers la zone **Groupes de séries** du graphique.  
  
13. Cliquez avec le bouton droit sur le groupe dans la zone **Groupes de séries** , puis sélectionnez **Propriétés du groupe de séries**.  
  
14. Cliquez sur **Tri**.  
  
15. Cliquez sur **Ajouter**. Une nouvelle ligne apparaît dans la grille d'expressions de tri.  
  
16. Dans **Trier par**, dans la liste déroulante, sélectionnez le champ de dataset selon lequel vous avez choisi d’effectuer le regroupement à l’étape 9.  
  
17. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
     Aux étapes suivantes, vous devez affecter au groupe de colonnes de la matrice et au groupe de catégories du graphique la même expression, ainsi que définir l'ordre de tri du groupe de catégories du graphique.  
  
18. À partir du volet Données du rapport, faites glisser vers le volet Groupes de colonnes le champ de dataset selon lequel vous souhaitez effectuer le regroupement des colonnes de la matrice.  
  
     Par défaut, le groupe de colonnes de la matrice ajoute une expression de tri identique à l'expression de groupe.  
  
19. Faites glisser le même champ de dataset que celui utilisé à l’étape 16 vers la zone **Groupes de catégories** du graphique.  
  
20. Cliquez avec le bouton droit sur le groupe dans la zone **Groupes de catégories** , puis cliquez sur **Propriétés du groupe de catégories**.  
  
21. Cliquez sur **Tri**.  
  
22. Cliquez sur **Ajouter**. Une nouvelle ligne apparaît dans la grille d'expressions de tri.  
  
23. Dans **Trier par**, dans la liste déroulante, sélectionnez le champ de dataset selon lequel vous avez choisi d’effectuer le regroupement à l’étape 16.  
  
24. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
25. Affichez un aperçu du résultat. Les groupes de lignes et de colonnes de la matrice affichent les mêmes données que les groupes de catégories et de séries du graphique.  
  
## <a name="see-also"></a> Voir aussi  
 [Liaison de plusieurs régions de données à un même dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)   
 [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
  
