---
title: Ajouter des points vides à un graphique (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: 2b056119-439f-494f-83cf-ee0c05dc6487
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: dfa20f918ae7c5c932e298c371ee9253af7b8d6e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-empty-points-to-a-chart-report-builder-and-ssrs"></a>Ajouter des points vides au graphique (Générateur de rapports et SSRS)
Les valeurs Null sont affichées sur un graphique sous la forme d'espaces ou d'intervalles vides entre les points de données d'une série. Dans les rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , les points vides sont des points de données qui peuvent être insérés dans l’espace vide créé par des valeurs Null.  
  
 Par défaut, les points vides sont calculés en faisant la moyenne des points de données précédent et suivant qui contiennent une valeur. Vous pouvez modifier ce comportement afin que tous les points vides soient insérés à zéro.  
  
 Pour plus d’informations, consultez [Points de données vides et Null dans les graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/empty-and-null-data-points-in-charts-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-specify-empty-points-on-a-chart"></a>Pour spécifier des points vides sur un graphique  
  
1.  Ouvrez le volet Propriétés.  
  
2.  Sur l'aire de conception, cliquez avec le bouton droit sur la série qui contient des valeurs Null. Les propriétés de la série sont affichées dans le volet Propriétés.  
  
3.  Développez le nœud **EmptyPoint** .  
  
4.  Sélectionnez une valeur de couleur pour la propriété Color.  
  
5.  Dans le nœud **EmptyPoint** , développez le nœud Marqueur.  
  
6.  Sélectionnez un type de marqueur pour la propriété MarkerType.  
  
    > [!NOTE]  
    >  Vous devez sélectionner un type de marqueur pour ajouter des points vides à un graphique à barres, à un histogramme ou un graphique à nuages de points. Toutefois, la sélection d'un type de marqueur est facultative pour les graphiques en aires, en courbes et en radar, car le graphique remplit l'espace ou intervalle vide sans nécessiter la spécification d'un marqueur.  
  
7.  Définissez la valeur du point vide.  
  
    1.  Dans le volet Propriétés, développez le nœud **CustomAttributes** .  
  
    2.  Définissez la propriété EmptyPointValue. Pour insérer des points vides à une moyenne des points de données précédent et suivant, sélectionnez **Moyenne**. Pour insérer des points vides à zéro, sélectionnez **Zéro**.  
  
## <a name="see-also"></a> Voir aussi  
 [Ajouter des filtres de datasets, des filtres de régions de données et des filtres de groupes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Types de graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-types-report-builder-and-ssrs.md)   
 [Ajouter des séparateurs d’échelle à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-scale-breaks-to-a-chart-report-builder-and-ssrs.md)   
 [Graphiques (Générateur de rapports et SSRS)](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)  
  
  
