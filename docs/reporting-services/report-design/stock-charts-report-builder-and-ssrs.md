---
title: Graphiques boursiers (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: f75ca11e-b7f5-4ac0-ba17-fe6f82742dad
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 35a1efc843cc5decd0ab1e7775aa694be173fb78
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="stock-charts-report-builder-and-ssrs"></a>Graphiques boursiers (Générateur de rapports et SSRS)

  Un graphique boursier est conçu spécialement pour les données financières ou scientifiques qui utilisent jusqu'à quatre valeurs par point de données. Ces valeurs sont alignées avec les valeurs maximales, minimales, d'ouverture et de clôture utilisées pour tracer des données boursières financières. Ce type de graphique affiche les valeurs d'ouverture et de clôture à l'aide de marqueurs, en règle générale des lignes ou des triangles. Dans l'exemple suivant, les valeurs d'ouverture sont indiquées par les marqueurs sur la gauche, et les valeurs de clôture sont indiquées par les marqueurs sur la droite.  
  
 ![Graphique boursier](../../reporting-services/report-design/media/rs-stockchart.gif "Graphique boursier")  
  
 Un exemple de graphique boursier est disponible dans les exemples de rapports du Générateur de rapports. Pour plus d'informations sur le téléchargement de cet exemple de rapport et d'autres rapports, consultez [Exemples de rapports du Générateur de rapports et du Concepteur de rapports](http://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="variations"></a>Variantes  
  
-   **Bougies**. Le graphique en bougies est un type spécial de graphique boursier, où des zones sont utilisées pour afficher la plage entre les valeurs d'ouverture et de clôture. Comme le graphique boursier, le graphique en bougies peut afficher jusqu'à quatre valeurs par point de données.  
  
## <a name="data-considerations-for-stock-charts"></a>Considérations relatives aux données pour les graphiques boursiers  
  
-   Lors de la présentation de nombreux points de données boursiers, tels que la tendance du cours de l'action annuelle, il est difficile de distinguer chaque valeur d'ouverture, de clôture, maximale et minimale pour chaque point de données. Dans ce scénario, pensez à utiliser un graphique en courbes au lieu d'un graphique boursier.  
  
-   Lorsque les étiquettes d'axe sont générées, l'étiquetage commence à zéro.  En règle générale, les cours de l'action ne fluctuent pas de la même façon que d'autres datasets. C'est pourquoi, vous pouvez empêcher le démarrage des étiquettes d'axe à zéro, afin d'obtenir une meilleure vue de vos données. Pour ce faire, définissez **IncludeZero** à **false** dans la boîte de dialogue **Propriétés de l'axe** ou dans la fenêtre Propriétés. Pour plus d’informations sur la façon dont le graphique génère les étiquettes de l’axe, consultez [Mise en forme des étiquettes des axes sur un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md).  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit de nombreuses formules calculées à utiliser avec des graphiques boursiers, notamment un indicateur de prix, un indicateur de force relative, MACD, etc.  

## <a name="next-steps"></a>Étapes suivantes

[Graphiques d’étendue](../../reporting-services/report-design/range-charts-report-builder-and-ssrs.md)   
[Graphiques](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Mise en forme d’un graphique](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[Boîte de dialogue Propriétés de l’axe, Options de l’axe](http://msdn.microsoft.com/library/b276e210-7a12-48ae-971b-7dabae51df11)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](http://go.microsoft.com/fwlink/?LinkId=620231)
