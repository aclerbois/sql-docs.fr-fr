---
title: 3D, biseau et autres effets dans un graphique (Générateur de rapports et SSRS) | Microsoft Docs
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
f1_keywords:
- "10156"
ms.assetid: 18ef2119-2931-43ae-9078-f39b460462dd
caps.latest.revision: 10
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 4170454a934fb8f35fff8c353f4bfa42eeec589d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="chart-effects---3d-bevel-and-other-report-builder"></a>Effets de graphique - 3D, biseau et autres (Générateur de rapports)
  Des effets 3D (en trois dimensions) peuvent être utilisés pour donner de la profondeur et augmenter l’impact visuel des graphiques de vos rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Par exemple, si vous voulez insister sur un secteur particulier d'un graphique à secteurs éclatés, vous pouvez faire pivoter le graphique et changer sa perspective pour que les lecteurs remarquent ce secteur en premier. Lorsque des effets 3D sont appliqués à votre graphique, les couleurs de dégradé et les styles de hachurage sont désactivés.  
  
 Des effets en trois dimensions peuvent être appliqués aux graphiques individuels et il est possible d'afficher à la fois des graphiques en deux et trois dimensions sur le même rapport.  
  
 Pour tous les types de graphiques, vous pouvez ajouter des effets en trois dimensions à une zone de graphique dans la boîte de dialogue **Propriétés de la zone de graphique** en sélectionnant **Afficher en 3D**. Pour plus d’informations, consultez [Ajouter des effets 3D à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-effects-add-3d-effects-report-builder.md).  
  
 Il est également possible d'améliorer l'impact visuel des graphiques en ajoutant des styles de biseau, de relief et de texture dans les graphiques à barres, à secteurs, en anneau, et les histogrammes. Pour plus d’informations, consultez [Ajouter des styles de biseau, du relief et des textures à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-effects-add-bevel-emboss-or-texture-report-builder.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="coordinate-based-three-dimensional-charts"></a>Graphiques en trois dimensions basés sur les coordonnées  
 Lors de l'utilisation de types de graphiques basés sur les coordonnées (histogramme, graphique à barres, graphique en aires, graphique à points, graphique en courbes et graphique d'étendue), les effets en trois dimensions affichent le graphique avec un troisième axe, connu sous le nom d'« axe des Z ». L'introduction de ce troisième axe vous permet d'appliquer diverses améliorations visuelles au graphique.  
  
### <a name="changing-the-white-space-in-a-3d-chart"></a>Modification de l'espace dans un graphique 3D  
 Lorsque vous affichez une zone de graphique en trois dimensions, chaque série est affichée sur une ligne séparée le long de l'axe des Z du graphique. Pour changer la quantité d’espace entre chaque série, modifiez la profondeur de l’intervalle des points du graphique en modifiant la propriété **Profondeur de l’intervalle** dans la boîte de dialogue Effets 3D.  
  
### <a name="changing-the-projection-of-a-3d-chart"></a>Modification de la projection d'un graphique 3D  
 Il existe deux types de projections 3D : oblique et en perspective. Une projection oblique ajoute une dimension de profondeur à un graphique en deux dimensions. L'axe des Z est tracé à des angles égaux à partir des axes horizontal et vertical, qui restent perpendiculaires l'un par rapport à l'autre comme dans un graphique en deux dimensions.  
  
 La projection en perspective transforme le graphique en estimant un plan de vue et en traçant à nouveau le graphique comme s'il était visualisé à partir de ce point. La valeur **Rotation** déplace verticalement la vue du « niveau du sol » situé à 0 jusqu’au plafond situé à 90. La valeur **Inclinaison** déplace l’angle de vue vers la gauche ou vers la droite. La valeur 0 est équivalente à une vue en deux dimensions du graphique. La valeur **Perspective** définit le pourcentage de distorsion qui sera utilisé lors de l’affichage de la projection. Ce type de projection conserve les proportions du graphique, mais son apparence se déforme et il est plus efficace d'utiliser un degré inférieur de perspective.  
  
> [!NOTE]  
>  Les projections oblique et en perspective étant des types distincts de projections, elles ne peuvent pas être utilisées ensemble sur le même graphique.  
  
### <a name="clustering-data"></a>Données de clustering  
 Dans les graphiques 2D, plusieurs séries de données s'affichent côte à côte. Le clustering affiche les séries individuelles en lignes séparées sur un graphique 3D. Par exemple, si vous avez un graphique qui contient trois séries de points de données, le clustering affichera chacune des trois séries sur une ligne séparée le long de l'axe des Z. Par défaut, tous les types de graphiques affichés en 3D sont en cluster.  
  
 Le clustering peut être désactivé pour les graphiques à barres et histogrammes. Lorsque le clustering est désactivé, les séries de plusieurs graphiques à barres et histogrammes sont affichées côte à côte sur une ligne.  
  
## <a name="shape-based-three-dimensional-charts"></a>Graphiques en trois dimensions basés sur les formes  
 Moins d'effets en trois dimensions sont disponibles pour les types de graphiques basés sur les formes (graphiques à secteurs, en anneau, en entonnoir, en pyramide). Lorsque vous utilisez des types de graphiques basés sur les formes, vous pouvez modifier les valeurs de rotation et d'inclinaison uniquement.  
  
## <a name="rotations"></a>Rotations  
 Les graphiques peuvent subir une rotation horizontale ou verticale entre -90 et 90 degrés. Un angle horizontal positif fera pivoter le graphique dans le sens inverse des aiguilles d'une montre autour de l'axe des abscisses, tandis qu'un angle vertical positif le fera pivoter dans le sens des aiguilles d'une montre autour de l'axe des ordonnées.  
  
## <a name="highlighting-3d-effects"></a>Mise en surbrillance des effets 3D  
 Vous pouvez ajouter des styles de mise en surbrillance à un graphique 3D via la propriété **Ombrage** qui apparaît sous Area3DStyle dans le volet Propriétés quand la zone de graphique est sélectionnée. Un style d'éclairage simple applique la même teinte aux éléments de zone de graphique. Un style réaliste modifie les teintes des éléments de zone de graphique selon un angle d'éclairage spécifié.  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme des étiquettes des axes sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Ajouter des effets 3D à un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/chart-effects-add-3d-effects-report-builder.md)  
  
  
