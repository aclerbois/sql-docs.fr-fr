---
title: Liaison de plusieurs régions de données à un même dataset (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 90c94a91-8fb2-42cb-b998-563691f30d2d
caps.latest.revision: 10
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 3934de69be96c17fe421836413899526df58f9c7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs"></a>Liaison de plusieurs régions de données à un même dataset (Générateur de rapports et SSRS)

Vous pouvez ajouter plusieurs régions de données à un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour proposer des vues différentes des données d’un même dataset de rapport. Par exemple, vous pouvez afficher des données dans une table et les représenter dans un graphique. Pour ce faire, vous devez utiliser des étendues et des expressions identiques pour les expressions de filtre, les expressions de tri et les expressions de groupe appropriées.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 Pour savoir s'il convient d'utiliser un graphique avec une table ou une matrice pour représenter des données identiques, il peut s'avérer utile d'établir un parallèle entre une table et les graphiques à base de formes d'une part et une matrice et les graphiques en aires, les graphiques à barres et les histogrammes d'autre part. Une table avec un seul groupe de lignes peut facilement être représentée sous forme de graphique à secteurs. À mesure que vous ajoutez des groupes de lignes, vous pouvez choisir différents types de graphiques pour représenter au mieux les groupes imbriqués. L'ajout de groupes de lignes imbriqués à un graphique à secteurs augmente le nombre de secteurs dans le graphique. Vous devez déterminer si le nombre d'instances de groupe correspondant au groupe parent et au groupe enfant réunis est trop important pour être représenté dans un seul graphique à secteurs. Lorsque plusieurs valeurs de groupe sont représentées sous forme de petits secteurs dans un graphique à secteurs, vous pouvez définir une propriété de telle sorte que toutes les valeurs en dessous d'un certain seuil soient représentées sous forme de graphique à secteurs à part entière. Pour plus d’informations, consultez [Regrouper des petits secteurs sur un graphique à secteurs](../../reporting-services/report-design/collect-small-slices-on-a-pie-chart-report-builder-and-ssrs.md).  
  
 Une table comportant plusieurs groupes de lignes peut être représentée sous forme d'histogramme avec plusieurs groupes de catégorie. Pour plus d’informations, consultez [Afficher les mêmes données dans une matrice et sur un graphique](../../reporting-services/report-design/display-the-same-data-on-a-matrix-and-a-chart-report-builder.md). Pour obtenir un exemple de table et de graphique présentant différentes vues d'un même dataset de rapport, consultez le rapport Product Line Sales dans Exemples de rapports AdventureWorks. Comme la table et le graphique sont liés au même dataset dans ce rapport, lorsque vous cliquez sur le bouton de tri interactif d'Employee Name dans la table Top Employees, le nouvel ordre de tri est automatiquement répercuté dans le graphique Top Employees. Pour plus d'informations sur le téléchargement de cet exemple de rapport et d'autres rapports, consultez [Exemples de rapports du Générateur de rapports et du Concepteur de rapports](http://go.microsoft.com/fwlink/?LinkId=198283).  
  
 La représentation d'une matrice qui comporte plusieurs groupes de lignes et de colonnes peut être améliorée à l'aide d'un graphique en aires, d'un graphique à barres ou d'un histogramme avec des groupes de catégories et de séries. Utilisez les mêmes expressions de groupe pour les groupes de colonnes de la matrice et les groupes de catégories du graphique et les mêmes expressions de groupe pour les groupes de lignes de la matrice et les groupes de séries du graphique. Gardez à l'esprit que le nombre d'instances de groupe influe sur la lisibilité du graphique. Vous pouvez définir des groupes en fonction de valeurs de plages pour réduire le nombre d'instances de groupe dans un rapport. Pour plus d’informations, consultez [Exemples d’expressions de groupe](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md).  
  
## <a name="next-steps"></a>Étapes suivantes

[Graphiques](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Tables, matrices et listes](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
[Régions de données imbriquées](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
