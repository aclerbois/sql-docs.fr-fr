---
title: Graphiques à base de formes (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 4b8404c1-aa89-4350-8bd6-203bc0446ee4
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 06335772043251b1e1a2923444e432f910ef22b4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="shape-charts-report-builder-and-ssrs"></a>Graphiques à base de formes (Générateur de rapports et SSRS)
  Un graphique à base de formes affiche des valeurs sous la forme de pourcentages par rapport à un total. Les graphiques à base de formes sont utilisés en général pour afficher des comparaisons proportionnelles entre des valeurs différentes d'un ensemble. Les catégories sont représentées par les segments individuels de la forme. La taille du segment varie en fonction de la valeur. Les graphiques à base de formes sont semblables aux graphiques en secteurs, sauf qu'ils classent les catégories de la plus grande à la plus petite.  
  
 Un graphique en entonnoir affiche les valeurs comme des proportions qui décroissent progressivement. La taille d'une zone est déterminée par la valeur de la série sous la forme d'un pourcentage du total de toutes les valeurs. Par exemple, vous pouvez utiliser un graphique en entonnoir pour afficher des tendances relatives à la visite d'un site Web. Il est probable que le graphique en entonnoir affichera une large zone en haut, indiquant le nombre de visites sur la page d'accueil et les autres zones seront proportionnellement plus petites. Pour plus d’informations sur l’ajout de données à un graphique en entonnoir, consultez [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md).  
  
 L'illustration suivante montre un exemple de graphique en entonnoir.  
  
 ![Graphique en entonnoir](../../reporting-services/report-design/media/rs-funnelchart.gif "Graphique en entonnoir")  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="variations"></a>Variantes  
  
-   **Pyramide**. Un graphique en pyramide affiche des données proportionnelles afin que le graphique ressemble à une pyramide.  
  
## <a name="data-considerations-for-shape-charts"></a>Considérations relatives aux données pour les graphiques à base de formes  
  
-   Les graphiques à base de formes sont courants dans les rapports en raison de leur impact visuel. Toutefois, les graphiques à base de formes sont un type de graphique très simplifié qui peut ne pas représenter au mieux vos données. Pensez à utiliser un graphique à base de formes une fois l'agrégation de données en sept points de données maximum terminée. En général, utilisez le graphique à base de formes pour afficher une seule catégorie par région de données.  
  
-   Les graphiques à base de formes affichent chaque groupe de données comme un segment séparé du graphique. Vous devez ajouter au moins un champ de données et un champ de catégorie. Si plusieurs champs de données sont ajoutés à un graphique à base de formes, le graphique à base de formes affichera les champs de données dans le même graphique.  
  
-   Les graphiques à base de formes sont idéaux pour représenter des pourcentages proportionnels en respectant un ordre de tri. Toutefois, par souci de cohérence, le graphique ne trie pas les valeurs de votre dataset par défaut. Pensez à organiser vos valeurs de la plus élevée à la plus basse pour représenter le plus correctement possible vos données sous la forme d'un graphique en entonnoir ou en pyramide. Pour plus d’informations, consultez [Filtrer, regrouper et trier des données &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md).  
  
-   Les valeurs null, vides, négatives n'ont aucun effet lors du calcul de ratios C'est pourquoi, ces valeurs ne sont pas affichées sur un graphique à base de formes. Si vous souhaitez indiquer visuellement ces types de valeurs sur votre graphique, modifiez le type de graphique. Pour plus d’informations sur l’ajout de points vides à un graphique qui n’est pas à base de formes, consultez [Ajouter des points vides à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md).  
  
-   Si vous définissez vos propres couleurs sur un graphique à base de formes à l'aide d'une palette personnalisée, assurez-vous de disposer de suffisamment de couleurs dans votre palette pour mettre en surbrillance chaque point de données avec sa propre couleur. Pour plus d’informations, consultez [Mise en forme des couleurs des séries d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md).  
  
-   Contrairement à tous les autres types de graphiques, un graphique à base de formes affichera des points de données individuels, et non des séries individuelles, dans sa légende.  
  
-   Les paramètres les axes des abscisses et des ordonnées sont ignorés dans les graphiques en entonnoir. Si vous avez plusieurs groupes de catégories et de séries, les étiquettes de groupes sont affichées dans la légende du graphique.  
  
-   Les types de graphique à base de formes ne peuvent pas être combinés avec un autre type de graphique dans la même zone de graphique. Si vous devez afficher des comparaisons entre des données affichées sur un graphique à base de formes et des données affichées sur un autre type de graphique, vous devrez ajouter une deuxième zone de graphique.  
  
-   Vous pouvez appliquer des styles de dessin supplémentaires aux graphiques en secteurs et aux graphiques en anneau pour impact visuel amélioré. Pour plus d’informations, consultez [Mise en forme des couleurs des séries d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Points de données vides et Null dans les graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/empty-and-null-data-points-in-charts-report-builder-and-ssrs.md)   
 [Graphiques à secteurs (Générateur de rapports et SSRS)](../../reporting-services/report-design/pie-charts-report-builder-and-ssrs.md)  
  
  
