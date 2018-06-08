---
title: Afficher la formule d’une série chronologique (exploration de données) de modèle | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0aaa1be07dcd5857585e7db3dbd4a78a44d41ff3
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="view-the-formula-for-a-time-series-model-data-mining"></a>Afficher la formule d'un modèle de série chronologique (exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Si vous avez créé un modèle de série chronologique à l’aide de l’exploration de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le moyen le plus simple pour afficher l’équation de régression pour le modèle est d’utiliser la **légende d’exploration de données** de la [visionneuse de l’algorithme MTS (Microsoft Time Series)](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-time-series-viewer.md), qui présente toutes les constantes dans un format lisible.  
  
### <a name="to-view-the-artxp-regression-formula-for-a-time-series-model"></a>Pour consulter la formule de régression ARTXP d'un modèle de série chronologique  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez le modèle de série chronologique à afficher, puis cliquez sur **Parcourir**.  
  
     -- ou --  
  
     Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], sélectionnez le modèle de série chronologique, puis cliquez sur l’onglet **Visionneuse de modèle d’exploration de données** .  
  
2.  Cliquez sur l’onglet **Modèle** .  
  
3.  Si le modèle contient plusieurs arborescences, sélectionnez une arborescence unique dans la liste déroulante **Arborescence** .  
  
    > [!NOTE]  
    >  Un modèle aura toujours plusieurs arborescences si vous avez plusieurs séries de données. Toutefois, vous ne verrez pas autant d’arborescences dans la **visionneuse de l’algorithme MTS** que dans la [Visionneuse de l’arborescence de contenu générique Microsoft](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c). Cela s'explique par le fait que la visionneuse de l'algorithme MTS associe les informations ARIMA et ARTXP de chaque série de données dans une représentation unique.  
  
4.  Cliquez sur n'importe quel nœud terminal de l'arborescence.  
  
     Les nœuds étiquetés comme **Série de données** sont toujours des nœuds terminaux et peuvent contenir une équation. Si un nœud **(Tout)** n’a pas de nœuds enfants, il peut également contenir une équation.  
  
5.  Si la **Légende d’exploration de données** n’est pas disponible, cliquez avec le bouton droit sur le nœud, puis sélectionnez **Afficher la légende**.  
  
     La formule ARTXP est affichée dans la première moitié de la **Légende d’exploration de données**, comme **l’équation du nœud d’arbre**.  
  
     ![affichage de la formule de série chronologique dans la légende](../../analysis-services/data-mining/media/ssdm-timeserieslegend.png "affichage de la formule de série chronologique dans la légende")  
  
### <a name="to-view-the-arima-formula-for-a-time-series-model"></a>Pour consulter la formule ARIMA d'un modèle de série chronologique  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez le modèle de série chronologique à afficher, puis cliquez sur **Parcourir**.  
  
     -- ou --  
  
     Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], sélectionnez le modèle de série chronologique, puis cliquez sur l’onglet **Visionneuse de modèle d’exploration de données** .  
  
2.  Cliquez sur l’onglet **Modèle** .  
  
3.  Si le modèle contient plusieurs arborescences, sélectionnez une arborescence unique dans la liste déroulante **Arborescence** .  
  
    > [!NOTE]  
    >  Le modèle aura toujours plusieurs arborescences si vous incluez plusieurs séries de données.  
  
4.  Cliquez sur n'importe quel nœud de l'arborescence.  
  
     La formule ARIMA est affichée dans la seconde moitié de la **Légende d’exploration de données**, comme **Équation ARIMA**.  
  
5.  Si la **Légende d’exploration de données** n’est pas disponible, cliquez avec le bouton droit sur le nœud, puis sélectionnez **Afficher la légende**.  
  
### <a name="to-get-the-coefficients-and-terms-for-the-equation"></a>Pour obtenir les coefficients et les termes de l’équation  
  
1.  Vous pouvez également obtenir les termes et les coefficients de la formule de régression pour un modèle de série chronologique en créant une **requête de contenu** sur le contenu du modèle.  
  
     Pour plus d’informations, consultez [Exemples de requêtes de modèle de série chronologique](../../analysis-services/data-mining/time-series-model-query-examples.md).  
  
2.  Vous pouvez également parcourir les modèles de série chronologique et rechercher les termes et les coefficients à l’aide de la [Visionneuse de l’arborescence de contenu générique Microsoft](http://msdn.microsoft.com/library/751b4393-f6fd-48c1-bcef-bdca589ce34c).  
  
     Pour plus d’informations, consultez [Contenu du modèle d’exploration de données pour les modèles de séries chronologiques &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-time-series-models-analysis-services-data-mining.md).  
  
    > [!NOTE]  
    >  Si vous parcourez le contenu d’un modèle mixte qui utilise des modèles ARTXP et ARIMA, les deux modèles se trouvent dans des arborescences distinctes, jointes au niveau du nœud racine qui représentant le modèle. Bien que les modèles ARTXP et ARIMA soient présentés dans une seule visionneuse pour des raisons pratiques, les structures sont très différentes, comme le sont les équations, qui ne peuvent pas être associées ou comparées. Par exemple, l’arbre ARTXP s’apparente plutôt à un arbre de décision, alors que l’arbre ARIMA représente une série de moyennes mobiles.  
  
## <a name="see-also"></a>Voir aussi  
 [Tâches de la visionneuse modèle d’exploration de données et de procédures](../../analysis-services/data-mining/mining-model-viewer-tasks-and-how-tos.md)   
 [Parcourir un modèle à l’aide de la visionneuse de série Microsoft Time](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-time-series-viewer.md)  
  
  
