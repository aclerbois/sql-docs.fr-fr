---
title: Ajout de données à une région de données de tableau matriciel (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 8f1d0a76-afed-480f-98fb-89e2d4eb09b1
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 8f788ced74e5833607f490012685b05ecef7b704
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="adding-data-to-a-tablix-data-region-report-builder-and-ssrs"></a>Ajout de données à une région de données de tableau matriciel (Générateur de rapports et SSRS)
Dans les rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , pour afficher les données d’un dataset de rapport dans une table ou une matrice, spécifiez dans chaque cellule de données le nom d’un champ de dataset à afficher. Vous pouvez afficher des données de détail ou des données groupées. Si vous ajoutez des groupes à une table ou à une matrice, les lignes et les colonnes des valeurs de groupe et les données de groupe sont automatiquement ajoutées. Vous pouvez ensuite ajouter des totaux et des sous-totaux pour vos données.  
  
 Toutes les données d'une région de données appartiennent à au moins un groupe. Les données de détail font partie du groupe de détails. Pour plus d’informations sur les données de détail et les données groupées, consultez [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="adding-detail-data"></a>Ajout de données de détail  
 Les données de détail correspondent à toutes les données d'un dataset de rapport une fois les filtres appliqués au dataset, à la région de données et au groupe de détails. Toutes les données de détail affichées dans une même région de données de tableau matriciel doivent provenir du même dataset de rapport.  
  
 Pour afficher des données de détail d'un dataset de rapport dans une région de données de tableau matriciel, faites glisser un champ de dataset du volet des données de rapport vers chaque cellule de la ligne de détails. Pour les cellules existantes d'une région de données de tableau matriciel, vous pouvez ajouter ou modifier une expression de champ de dataset à l'aide du sélecteur de champ dans chaque cellule ou en faisant glisser un champ du volet des données de rapport vers la cellule. Pour créer des colonnes supplémentaires, vous pouvez faire glisser le champ du volet des données de rapport vers une région de données de tableau matriciel existante.  
  
 Par défaut, au moment de l'exécution, une cellule figurant dans la ligne de détails affiche des données de détail et une cellule figurant dans une ligne de groupe affiche une valeur d'agrégation. Pour plus d’informations sur les lignes et colonnes de tableau matriciel, consultez [Cellules, lignes et colonnes de région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-cells-rows-and-columns-report-builder-and-ssrs.md).  
  
 Un modèle de table et un modèle de liste fournissent une ligne de détails. Un modèle de matrice ne possède aucune ligne de détails. Si la région de données de tableau matriciel ne possède aucune ligne de détails, vous pouvez en ajouter une en définissant un groupe de détails. Pour plus d’informations, consultez [Ajouter un groupe de détails &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-details-group-report-builder-and-ssrs.md).  
  
## <a name="adding-grouped-data"></a>Ajout de données groupées  
 Les données groupées correspondent à toutes les données de détail spécifiées par une expression de groupe une fois les filtres appliqués au dataset, à la région de données et au groupe. Pour organiser les données de détail en groupes, faites glisser les champs du volet des données de rapportvers le volet Regroupement. Quand vous ajoutez un groupe, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ajoute automatiquement les lignes ou les colonnes associées à la région de données de tableau matriciel sur laquelle afficher des données groupées. Les cellules figurant dans ces lignes ou ces colonnes sont associées aux données groupées. Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md).  
  
 Par défaut, lorsque vous ajoutez un champ de dataset qui représente des données numériques à une cellule dans une ligne ou une colonne de groupe, la valeur de la cellule correspond à la somme des données groupées étendues aux membres du groupe de lignes et de colonnes le plus profond pour la cellule. Vous pouvez remplacer la fonction d’agrégation par défaut Sum par une autre fonction d’agrégation (Avg ou Count, par exemple). Vous pouvez également modifier l'étendue par défaut d'un calcul d'agrégats (pour calculer le pourcentage que représente une valeur par rapport à un groupe de lignes, par exemple). Pour plus d’informations, consultez [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
 Par défaut, toutes les données groupées proviennent du même dataset de rapport. Dans une région de données de tableau matriciel, vous pouvez inclure des valeurs d’agrégation d’un autre dataset en spécifiant le nom de dataset comme étendue. Vous pouvez spécifier plusieurs valeurs d'agrégation de plusieurs datasets dans une même région de données de tableau matriciel. Pour plus d’informations, consultez [Informations de référence sur les fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md).  
  
## <a name="adding-subtotals-and-totals"></a>Ajout de sous-totaux et de totaux  
 Pour ajouter des sous-totaux pour un groupe et des totaux généraux pour la région de données, utilisez la fonctionnalité Ajouter un total dans le menu contextuel d'une cellule ou dans le volet Regroupement. Les lignes et les colonnes sur lesquelles afficher les totaux sont automatiquement ajoutées. Les expressions de sous-total et de total utilisent par défaut la fonction d’agrégation [Sum](../../reporting-services/report-design/report-builder-functions-sum-function.md) . Une fois que vous avez ajouté l'expression, vous pouvez remplacer la fonction par défaut par une autre fonction. Pour plus d’informations, consultez [Ajouter un total à un groupe ou à une région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-total-to-a-group-or-tablix-data-region-report-builder-and-ssrs.md) et [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
## <a name="adding-labels"></a>Ajout d'étiquettes  
 Pour ajouter des étiquettes pour un groupe ou pour la région de données, ajoutez une ligne ou une colonne à l'extérieur du groupe que vous souhaitez étiqueter. Les lignes et les colonnes d'étiquette ressemblent aux lignes et aux colonnes que vous ajoutez pour afficher des totaux. Pour plus d’informations, consultez [Insérer ou supprimer une ligne &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-row-report-builder-and-ssrs.md) ou [Insérer ou supprimer une colonne &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-column-report-builder-and-ssrs.md).  
  
## <a name="adding-an-existing-tablix-data-region-from-another-report"></a>Ajout d'une région de données de tableau matriciel existante à partir d'un autre rapport  
 Vous pouvez copier une région de données d'un autre rapport et la coller dans un nouveau rapport ou un rapport existant. Une fois la région de données collée, vous devez vérifier que le dataset utilisé par la région de données est défini et que les champs du dataset portent les mêmes noms et possèdent les mêmes types de données que dans le rapport d'origine. Vous ne pouvez pas copier de datasets d'un rapport vers un autre, mais si vos rapports utilisent des sources de données partagées, vous pouvez dupliquer rapidement le dataset dans l'autre rapport. Vous pouvez également importer le texte des requêtes qui récupèrent les données du dataset, ce qui facilite la duplication des requêtes dans les rapports. Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Paramètres de rapport &#40;Générateur de rapports et Concepteur de rapports&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Tri interactif, Explorateurs de documents et liens &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/interactive-sort-document-maps-and-links-report-builder-and-ssrs.md)   
 [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md)   
 [Ajouter une expression &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-expression-report-builder-and-ssrs.md)  
  
  
