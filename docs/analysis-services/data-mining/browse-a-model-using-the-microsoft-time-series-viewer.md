---
title: Parcourir un modèle à l’aide de la visionneuse de série Microsoft Time | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fd57ae140adee0909c0f00647a334bd62f26c170
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="browse-a-model-using-the-microsoft-time-series-viewer"></a>Explorer un modèle à l'aide de la visionneuse de l'algorithme MTS (Microsoft Time Series)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La Visionneuse de l’algorithme MTS ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] affiche les modèles d’exploration de données qui sont générés à l’aide de l’algorithme MTS ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series). L’algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) est un algorithme de régression qui crée des modèles d’exploration de données pour la prédiction des colonnes continues, telles que les ventes de produits, dans un scénario de prédiction. Ces modèles de série chronologiques peuvent inclure des informations basées sur des algorithmes différents :  
  
-   L'algorithme ARTxp, qui est optimisé pour les prédictions à court terme.  
  
-   L'algorithme ARIMA, optimisé pour les prédictions à long terme.  
  
-   Un mélange entre les algorithmes ARTxp et ARIMA.  
  
 Pour plus d’informations sur ces algorithmes, consultez [Algorithme MTS (Microsoft Time Series)](../../analysis-services/data-mining/microsoft-time-series-algorithm.md) et [Références techniques relatives à l’algorithme MTS (Microsoft Time Series)](../../analysis-services/data-mining/microsoft-time-series-algorithm-technical-reference.md).  
  
> [!NOTE]  
>  La visionneuse de l'arborescence de contenu générique [!INCLUDE[msCoName](../../includes/msconame-md.md)] permet de consulter des informations détaillées relatives aux équations utilisées dans le modèle et les motifs découverts. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse de l’arborescence de contenu générique Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-generic-content-tree-viewer.md) ou [Visionneuse de l’arborescence de contenu générique Microsoft &#40;exploration de données&#41;](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c).  
  
##  <a name="BKMK_ViewerTabs"></a> Onglets de la visionneuse  
 Lorsque vous parcourez un modèle d'exploration de données dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], le modèle s'affiche sous l'onglet **Visionneuse de modèle d'exploration de données** du Concepteur d'exploration de données, à l'aide de la visionneuse appropriée pour ce modèle. La Visionneuse de l'algorithme MTS ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Time Series) inclut les onglets suivants :  
  
-   [Modèle](#BKMK_Tree)  
  
-   [Graphiques](#BKMK_Charts)  
  
 **Remarque** Les informations affichées pour le contenu du modèle et dans la Légende d’exploration de données dépendent de l’algorithme que le modèle utilise. Toutefois, les onglets **Modèle** et **Graphiques** sont les mêmes indépendamment de la combinaison d’algorithmes.  
  
###  <a name="BKMK_Tree"></a> Modèle  
 Quand vous générez un modèle de série chronologique, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] présente le modèle terminé sous forme d’arborescence. Si vos données contiennent plusieurs séries de cas, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère une arborescence séparée pour chaque série. Supposons, par exemple, que vous prévoyiez des ventes pour la zone Pacifique, l'Amérique du Nord et des régions d'Europe. Les prédictions pour chacune de ces régions sont des séries de cas. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère une arborescence distincte pour chacune de ces séries. Pour afficher une série particulière, sélectionnez-la série dans la liste **Arborescence** .  
  
 Pour chaque arborescence, le modèle de série chronologique contient un nœud **Tous** , puis il le fractionne en une série des nœuds qui représentent des structures périodiques découvertes par l’algorithme. Vous pouvez cliquer sur chaque nœud pour afficher des statistiques, telles que le nombre de cas et l'équation.  
  
 Si vous avez créé le modèle à l’aide d’ARTxp uniquement, la **Légende d’exploration de données** pour le nœud racine contient uniquement le nombre total de cas. Pour chaque nœud non racine, la **Légende d’exploration de données** contient plus d’informations détaillées concernant le fractionnement de l’arborescence. Par exemple, l’équation pour le nœud et le nombre de cas peuvent être indiqués. La *règle* dans la légende contient des informations qui identifient la série et la tranche de temps d’application de la règle. Par exemple, le texte de légende `M200 Europe Amount -2` indique que le nœud représente le modèle de la série M200 Europe, à une période équivalente à deux tranches de temps en arrière.  
  
 Si vous avez créé le modèle à l’aide d’ARIMA uniquement, l’onglet **Modèle** contient un nœud unique avec la légende **Tous**. La **Légende d’exploration de données** pour le nœud racine contient l’équation ARIMA.  
  
 Si vous avez créé un modèle mixte, le nœud racine contient le nombre de cas et l'équation ARIMA uniquement. Après le nœud racine, l'arborescence est fractionnée en différents nœuds pour chaque structure périodique. Pour chaque nœud non racine, la Légende d'exploration de données contient à la fois les algorithmes ARTxp et ARIMA, l'équation pour le nœud et le nombre de cas dans le nœud. L'équation ARTxp est répertoriée en premier et étiquetée comme équation de nœud d'arborescence. Elle est suivie par l'équation ARIMA. Pour plus d’informations sur l’interprétation de ces informations, consultez, [Références techniques relatives à l’algorithme MTS (Microsoft Time Series)](../../analysis-services/data-mining/microsoft-time-series-algorithm-technical-reference.md).  
  
 En général, le graphique d’arbre de décision affiche le fractionnement le plus important, le nœud **Tous** , à gauche de la visionneuse. Dans les arbres de décision, le fractionnement situé après le nœud **Tous** est le plus important, car il contient la condition qui sépare de la façon la plus importante les données d’apprentissage. Dans un modèle de série chronologique, le branchement principal indique le cycle saisonnier le plus probable. Les fractionnements situés après le nœud **Tous** apparaissent à droite de la branche.  
  
 Vous pouvez développer ou réduire certains nœuds dans l'arbre pour afficher ou masquer les fractionnements qui apparaissent après chaque nœud. Vous pouvez également utiliser les options de l’onglet **Arbre de décision** pour modifier l’affichage de l’arbre. Utilisez le curseur **Afficher le niveau** pour régler le nombre de niveaux affichés dans l’arbre. Utilisez **Expansion par défaut** pour définir le nombre de niveaux par défaut affichés pour tous les arbres du modèle.  
  
 L'ombrage de la couleur d'arrière-plan pour chaque nœud indique le nombre de cas qui se trouvent dans le nœud. Pour trouver le nombre exact de cas dans un nœud, placez le pointeur sur le nœud pour afficher une info-bulle sur le nœud.  
  
 [Retour au début](#BKMK_ViewerTabs)  
  
###  <a name="BKMK_Charts"></a> Graphiques  
 L’onglet **Graphiques** affiche un graphique qui représente le comportement de l’attribut prévu sur le temps, avec cinq valeurs futures prévues. L'axe vertical du graphique représente la valeur de la série et l'axe horizontal le temps.  
  
> [!NOTE]  
>  Les tranches de temps utilisées sur l'axe temporel dépendent des unités utilisées dans vos données : elles peuvent représenter des jours, des mois ou même des secondes.  
  
 Utilisez le bouton **Abs** pour basculer entre des courbes absolues et relatives. Si votre graphique contient plusieurs modèles, l'échelle des données pour chaque modèle peut être très différente. Si vous utilisez une courbe absolue, un modèle peut apparaître sous forme de ligne à deux dimensions (flat), alors qu'un autre modèle affiche des modifications significatives. Cela est dû au fait que l'échelle de l'un des modèles est plus importante que celle de l'autre modèle. En basculant vers une courbe relative, vous modifiez l'échelle pour afficher le pourcentage de modification au lieu de valeurs absolues. Cela le simplifie la comparaison des modèles basés sur ses échelles différentes.  
  
 Si le modèle d'exploration de données contient plusieurs séries chronologiques, vous pouvez sélectionner une ou plusieurs séries à afficher dans le graphique. Il vous suffit de cliquer sur la liste à droite de la visionneuse et de sélectionner la série de votre choix dans celle-ci. Si le graphique devient trop complexe, vous pouvez filtrer les séries affichées en activant ou en désactivant les cases à cocher des séries dans la légende.  
  
 Le graphique affiche à la fois les données historiques et les données futures. Les données futures sont grisées, afin de les distinguer des données d'historique. Les valeurs de données apparaissent sous forme de lignes pleines pour les données historiques et de lignes en pointillés pour les prédictions. Vous pouvez modifier la couleur des lignes utilisées pour chaque série en définissant les propriétés dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations, consultez [Modifier les couleurs utilisées dans la visionneuse d’exploration de données](../../analysis-services/data-mining/change-the-colors-used-in-the-data-mining-viewer.md).  
  
 Vous pouvez régler la plage de temps affichée en utilisant les options de zoom. Vous pouvez également afficher une plage de temps spécifique en cliquant sur le graphique, en faisant glisser une plage de temps dans le graphique, puis en cliquant de nouveau pour effectuer un zoom avant sur la plage sélectionnée.  
  
 Vous pouvez sélectionner le nombre **d’étapes** chronologiques futures que doit contenir le modèle, en utilisant **Étapes de la prévision**. Si vous cochez la case **Afficher les écarts** , la visionneuse affiche des barres d’erreur pour que vous puissiez voir le niveau de précision de la valeur prédite.  
  
 [Retour au début](#BKMK_ViewerTabs)  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches de la visionneuse modèle d’exploration de données et de procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)   
 [Algorithme de série chronologique de Microsoft](../../analysis-services/data-mining/microsoft-time-series-algorithm.md)   
 [Exemples de requête de modèle de série de temps](../../analysis-services/data-mining/time-series-model-query-examples.md)   
 [Visionneuses de modèle d’exploration de données](../../analysis-services/data-mining/data-mining-model-viewers.md)  
  
  
