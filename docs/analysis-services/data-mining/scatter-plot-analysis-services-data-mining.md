---
title: Nuage de points (Analysis Services - Exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fa64802b565034829030869dd6242eb57525a0f4
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="scatter-plot-analysis-services---data-mining"></a>Nuage de points (Analysis Services - Exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Un *nuage de points* représente graphiquement les valeurs réelles de vos données par rapport aux valeurs prédites par le modèle. Le nuage de points affiche les valeurs réelles le long de l'axe des X et les valeurs prédites le long de l'axe des Y. Il affiche également une ligne qui illustre la prédiction parfaite, où la valeur prédite correspond exactement à la valeur réelle. La distance d'un point par rapport à cette ligne à un angle idéal de 45 degrés indique le niveau d'exactitude ou d'inexactitude de la prédiction.  
  
## <a name="understanding-the-scatter-plot"></a>Fonctionnement du nuage de points  
 Considérons un modèle dans lequel le département marketing prédit les ventes quotidiennes basées sur le nombre de clics sur un lien envoyé dans un message électronique promotionnel. Étant donné que le nombre de clics et le volume des ventes sont des valeurs numériques continues, vous pouvez tracer le nombre de clics en tant que variable indépendante et les ventes en tant que variable dépendante. Ce faisant, la ligne droite affiche la relation linéaire attendue, et les points éparpillés autour de cette ligne montrent comment les données réelles divergent de celles attendues. Cette analyse vous indique, d'un seul coup d'œil, le degré de corrélation d'un ensemble de résultats avec une entrée particulière, et le degré de variation par rapport au modèle idéal.  
  
## <a name="interpreting-the-results"></a>Interprétation des résultats  
 Le diagramme suivant présente un exemple de nuage de points, créé pour le scénario qui vient d'être décrit.  
  
 ![exemple d’un nuage de points pour régression linéaire](../../analysis-services/data-mining/media/scatterplot-callctr.gif "exemple d’un nuage de points pour régression linéaire")  
  
 Vous pouvez positionner la souris sur n'importe quel point éparpillé autour de la ligne pour afficher les valeurs prédite et réelle dans une info-bulle. Il n’y a aucune **légende d’exploration de données** pour un nuage de points ; toutefois, le graphique lui-même contient une légende qui affiche le score associé au modèle. Pour plus d’informations sur l’interprétation du score, consultez [Contenu du modèle d’exploration de données pour les modèles de régression linéaire &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md).  
  
 Vous pouvez copier la représentation visuelle du graphique dans le Presse-papiers, mais pas la formule ou les données sous-jacentes. Si vous voulez afficher la formule de régression pour la ligne, vous pouvez créer une requête de contenu sur le modèle. Pour plus d’informations, consultez [Exemples de requête de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md).  
  
## <a name="restrictions-on-scatter-plots"></a>Restrictions sur les nuages de points  
 Un nuage de points peut être créé uniquement si le modèle que vous choisissez sous l’onglet **Sélection d’entrée** contient un attribut prédictible continu. Vous ne devez effectuer aucune sélection supplémentaire. Le type de graphique de nuage de points est automatiquement affiché sous l’onglet **Graphique de courbes d’élévation** en fonction du modèle et du type d’attribut.  
  
 Bien que les modèles de séries chronologiques prédisent des nombres continus, vous ne pouvez pas mesurer la précision d'un modèle de série chronologique en utilisant un nuage de points. Il existe d'autres méthodes que vous pouvez utiliser, comme la réservation d'une partie des données historiques. Pour plus d’informations, consultez [Exemples de requêtes de modèle de série chronologique](../../analysis-services/data-mining/time-series-model-query-examples.md).  
  
## <a name="related-content"></a>Contenu connexe  
 Les rubriques suivantes contiennent davantage d'informations sur la façon dont vous pouvez créer et utiliser les nuages de points et graphiques d'analyse de précision associés.  
  
|Rubriques|Liens|  
|------------|-----------|  
|Propose une procédure pas à pas permettant de créer un graphique de courbes d'élévation pour le modèle de publipostage ciblé.|[Didacticiel d’exploration de données de base de données](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c)<br /><br /> [Test de la précision avec des graphiques de courbes d’élévation & #40 ; Didacticiel d’exploration de données de base de données & #41 ;](http://msdn.microsoft.com/library/822d414b-4a39-473f-80c3-53476e30655a)|  
|Explique les types de graphique associés.|[Graphique de courbes d’élévation & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/lift-chart-analysis-services-data-mining.md)<br /><br /> [Graphique des bénéfices & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/profit-chart-analysis-services-data-mining.md)<br /><br /> [Matrice de classification & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/classification-matrix-analysis-services-data-mining.md)|  
|Décrit les utilisations de la validation croisée pour les modèles et les structures d'exploration de données.|[La Validation croisée & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md)|  
|Décrit les étapes permettant de créer des graphiques de courbes d'élévation et d'autres graphiques d'analyse de précision.|[Test et de tâches de Validation et de procédures & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-tasks-and-how-tos-data-mining.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Test et Validation & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-data-mining.md)  
  
  
