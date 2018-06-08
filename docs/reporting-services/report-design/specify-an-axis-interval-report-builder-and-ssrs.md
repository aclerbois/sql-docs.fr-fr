---
title: Spécifier un intervalle d’axe (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 09/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ae46712d-a5bf-44c0-9929-e30ccc1e7e33
caps.latest.revision: 14
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: f3f228b9c0f6010c0db3654f81aa297d169a425d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-an-axis-interval-report-builder-and-ssrs"></a>Spécifier un intervalle d'axe (Générateur de rapports et SSRS)
Apprenez à modifier le nombre d’étiquettes et les graduations sur l’axe des catégories (abscisses) dans un graphique, en définissant l’intervalle d’axe dans un rapport paginé [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] .
 
Sur l’axe des valeurs (généralement, l’axe des ordonnées), les intervalles de l’axe fournissent une mesure cohérente des points de données sur le graphique. 

Mais sur l’axe des catégories (généralement l’axe des abscisses), un intervalle d’axe automatique se traduit parfois par des catégories sans étiquettes des axes. Vous pouvez spécifier le nombre d’intervalles souhaité dans la propriété Intervalle de l’axe. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] calcule le nombre d'intervalles au moment de l'exécution en fonction des données contenues dans le jeu de résultats. Pour plus d’informations sur le calcul des intervalles de l’axe, consultez [Mise en forme des étiquettes des axes sur un graphique](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md).  

Pour essayer de définir l’intervalle d’axe avec des exemples de données, consultez [Didacticiel : ajouter un histogramme à un rapport](Tutorial:%20Add%20a%20Column%20Chart%20to%20Your%20Report%20\(Report%20Builder\).md).
  
> [!NOTE]  
>  L'axe des abscisses est généralement l'axe horizontal, ou axe des X. Toutefois, pour les graphiques à barres, l'axe des abscisses est l'axe vertical, ou axe des Y.  
>
> Cette rubrique ne s’applique pas aux éléments suivants :
>-   Valeurs de date ou d’heure sur l’axe des catégories. Par défaut, les valeurs **DateTime** apparaissent sous la forme de jours. Vous pouvez spécifier une date ou un intervalle de temps différents, par exemple un mois ou un intervalle de temps. Pour plus d’informations, consultez [Mettre en forme les étiquettes des axes en tant que dates ou devises](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md).  
>-  Graphiques à secteurs, en anneau, en entonnoir ni en pyramide, qui n’ont pas d’axes. 
  
## <a name="to-show-all-the-category-labels-on-the-x-axis"></a>Pour afficher toutes les étiquettes de catégories sur l’axe des abscisses  

Dans cet histogramme, l’intervalle des étiquettes horizontales est défini sur Auto.

![report-builder-column-chart-preview-x-axis-interval-auto](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-auto.png)
  
1.  Cliquez avec le bouton droit sur l’axe des catégories, puis cliquez sur **Propriétés de l’axe horizontal**.   

    ![report-builder-column-chart-x-axis-labels](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-labels.png)
  
2.  Dans la boîte de dialogue **Propriétés de l’axe horizontal** > onglet **Options de l’axe**, définissez **Intervalle** sur **1** pour afficher chaque étiquette de groupe de catégorie. Pour afficher une étiquette de groupe de catégories sur deux sur l’axe des abscisses, tapez **2**. 

     ![report-builder-column-chart-x-axis-interval-one](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-interval-one.png)
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

    L’histogramme affiche maintenant toutes les étiquettes de son axe horizontal.

    ![report-builder-column-chart-preview-x-axis-interval-one](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-one.png)
  
    > [!NOTE]  
    >  Lorsque vous définissez un intervalle d’axe, tout l’étiquetage automatique est désactivé. Si vous spécifiez une valeur pour l’intervalle d’axe, vous pouvez constater un comportement d’étiquetage imprévisible en fonction du nombre de catégories sur l’axe des abscisses.  

## <a name="change-the-label-interval-in-properties-pane"></a>Modifier l’intervalle des étiquettes dans le volet Propriétés

Vous pouvez également définir l’intervalle des étiquettes dans le volet Propriétés.

1.  Dans l’affichage Conception de rapport, cliquez sur le graphique, puis sélectionnez les étiquettes de l’axe horizontal.

3. Dans le volet Propriétés, affectez la valeur **1**à LabelInterval.

    ![report-builder-column-chart-set-label-interval](../../reporting-services/media/report-builder-column-chart-set-label-interval.png)

    Le graphique a la même apparence en mode Conception. 
    
5.  Cliquez sur **Exécuter** pour afficher un aperçu du rapport.

    ![report-builder-column-chart-label-interval-one-preview](../../reporting-services/media/report-builder-column-chart-label-interval-one-preview.png)
    
    Le graphique affiche maintenant toutes ses étiquettes.
  
## <a name="to-enable-a-variable-interval-calculation-on-an-axis"></a>Pour activer un calcul d'intervalle variable sur un axe  

Par défaut, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] définit l’intervalle d’axe sur Auto. Cette procédure explique comment réaffecter la valeur par défaut. 
  
1.  Cliquez avec le bouton droit sur l’axe de graphique à modifier, puis cliquez sur **Propriétés de l’axe**. 
  
2.  Dans la boîte de dialogue **Propriétés de l’axe horizontal** > onglet **Options de l’axe**, définissez **Intervalle** sur **Auto**. Le graphique affiche le nombre optimal d'étiquettes de catégories qui peuvent s'ajuster le long de l'axe.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Mise en forme d’un graphique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Mise en forme des points de données sur un graphique (Générateur de rapports et SSRS)](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
 [Trier des données dans une région de données (Générateur de rapports et SSRS)](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md)   
 [Boîte de dialogue Propriétés de l’axe, Options de l’axe &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/b276e210-7a12-48ae-971b-7dabae51df11)   
 [Spécifier une échelle logarithmique &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-a-logarithmic-scale-report-builder-and-ssrs.md)   
 [Tracer des données sur un axe secondaire &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/plot-data-on-a-secondary-axis-report-builder-and-ssrs.md)  
  
  
