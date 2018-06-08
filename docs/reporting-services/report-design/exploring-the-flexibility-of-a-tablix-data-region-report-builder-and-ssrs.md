---
title: Exploration de la souplesse d’une région de données de tableau matriciel (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: fef19359-a618-4d21-a7e4-e391cdefd4eb
caps.latest.revision: 7
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: f66fa1f10498a014b244725e871ca935f89ee7a2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="exploring-the-flexibility-of-a-tablix-data-region-report-builder-and-ssrs"></a>Exploration de la souplesse d'une région de données de tableau matriciel (Générateur de rapports et SSRS)
Dans un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , quand vous ajoutez une région de données de table, de matrice ou de liste à partir de l’onglet Insérer sur le ruban, vous démarrez avec un modèle initial de région de données de tableau matriciel. Toutefois, vous n’êtes pas limité par ce modèle. Vous pouvez continuer à développer vos affichages de données en ajoutant ou en supprimant des fonctionnalités de région de données de tableau matriciel (groupes, lignes et colonnes, par exemple).  
  
 Lorsque vous supprimez un groupe de lignes ou de colonnes, vous avez la possibilité de supprimer les lignes et les colonnes qui sont utilisées pour afficher les valeurs de groupe. Vous pouvez également ajouter ou supprimer des lignes et des colonnes manuellement. Pour comprendre l’utilisation des lignes et des colonnes pour afficher des données de détail et de groupe, consultez [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md).  
  
 Après avoir modifié la structure de la région de données de tableau matriciel, vous pouvez définir des propriétés pour vous aider à contrôler le rendu de la région de données par le rapport. Par exemple, vous pouvez répéter des en-têtes de colonnes en haut de chaque page ou conserver un en-tête de groupe avec le groupe. Pour plus d’informations, consultez [Contrôle de l’affichage de la région de données de tableau matriciel sur une page de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="changing-a-table-to-a-matrix"></a>Transformation d'une table en matrice  
 Par défaut, une table possède des lignes de détails qui indiquent les valeurs du dataset du rapport. En règle générale, une table inclut des groupes de lignes qui organisent les données de détail par groupe, puis inclut des valeurs de synthèse en fonction de chaque groupe. Pour transformer la table en matrice, ajoutez des groupes de colonnes. Vous supprimez généralement les groupes de détails lorsque la région de données possède des groupes de lignes et de colonnes, de telle sorte que vous ne puissiez afficher que les valeurs de synthèse des groupes. Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md).  
  
 Par définition, lorsque vous créez une matrice, vous ajoutez une cellule d'angle de tableau matriciel. Vous pouvez fusionner des cellules dans cette zone et y ajouter une étiquette. Pour plus d’informations, consultez [Fusionner des cellules dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/merge-cells-in-a-data-region-report-builder-and-ssrs.md).  
  
## <a name="changing-a-matrix-to-a-table"></a>Transformation d'une matrice en table  
 Par défaut, une matrice possède des groupes de lignes et des groupes de colonnes, mais aucun groupe de détails. Pour transformer une matrice en table, supprimez les groupes de colonnes et ajoutez un groupe de détails pour l'afficher sur la ligne de détails. Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md) et [Ajouter un groupe de détails &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-details-group-report-builder-and-ssrs.md).  
  
## <a name="changing-a-default-list-to-a-grouped-list"></a>Transformation d'une liste par défaut en liste groupée  
 Par défaut, une liste possède des lignes de détails et aucun groupe. Pour modifier la liste en vue d'utiliser une ligne de groupe, renommez le groupe de détails et spécifiez une expression de groupe. Pour plus d’informations, consultez [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md).  
  
## <a name="creating-stepped-displays"></a>Création d'affichages en escalier  
 Par défaut, lorsque vous ajoutez des groupes à une région de données de tableau matriciel, les cellules qui figurent dans la zone d'en-tête de groupe de lignes indiquent les valeurs de groupe d'une colonne. En présence de groupes imbriqués, chaque groupe s'affiche dans une colonne distincte. Pour créer un affichage en escalier, supprimez toutes les colonnes de groupes sauf une et mettez en forme la colonne restante pour afficher la hiérarchie de groupes sous la forme d'un texte mis en retrait. Pour plus d’informations, consultez [Créer un rapport en escalier &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-stepped-report-report-builder-and-ssrs.md).  
  
## <a name="adding-an-adjacent-details-group"></a>Ajout d'un groupe de détails adjacent  
 Par défaut, le groupe de détails est le groupe enfant le plus profond dans une hiérarchie de groupes. Vous ne pouvez pas imbriquer un groupe sous le groupe de détails. Vous pouvez créer d'autres groupes de détails adjacents, pour afficher les 5 premiers produits et les 5 derniers produits par vente, par exemple. Comme vous pouvez ajouter des expressions de filtre et de tri à chaque groupe, vous pouvez afficher deux vues de données de détail du même dataset dans une région de données de tableau matriciel. Pour plus d’informations, consultez [Fonctionnement des groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md), [Ajouter ou supprimer un groupe dans une région de données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md) et [Ajouter un filtre à un dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Région de données de tableau matriciel &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Tables &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md)   
 [Matrices &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md)   
 [Listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md)   
  
  
