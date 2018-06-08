---
title: Test et Validation (exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e4ce983ed5d5a645f32466ab38d5c67ff1c68897
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="testing-and-validation-data-mining"></a>Test et validation (exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La validation est le processus consistant à évaluer les performances de vos modèles d'exploration de données sur des données réelles. Il est important de valider les modèles d'exploration de données en comprenant leurs qualité et caractéristiques avant de les déployer dans un environnement de production.  
  
 Cette section présente des concepts de base liés à la qualité des modèles, et décrit les stratégies de validation de modèles fournies dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour obtenir une vue d’ensemble de la façon dont la validation de modèle s’adapte à des processus d’exploration de données plus importants, consultez [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md).  
  
## <a name="methods-for-testing-and-validation-of-data-mining-models"></a>Méthodes de test et de validation des modèles d'exploration de données  
 De nombreuses approches permettent d'évaluer la qualité et les caractéristiques d'un modèle d'exploration de données.  
  
-   Utilisez diverses mesures de validité statistique pour déterminer la présence ou non de problèmes au niveau des données ou du modèle.  
  
-   Scindez les données en jeux d'apprentissage et de test pour tester la précision des prédictions.  
  
-   Demandez à des experts d'examiner les résultats du modèle d'exploration de données pour déterminer si les modèles découverts ont un sens dans le scénario d'entreprise ciblé  
  
 Toutes ces méthodes sont utiles dans la méthodologie d'exploration de données et sont utilisées de manière itérative lorsque vous créez, testez et affinez des modèles pour répondre à un problème spécifique. Aucune règle exhaustive unique ne peut indiquer lorsqu'un modèle est assez perfectionné ou lorsque vous avez suffisamment de données.  
  
## <a name="definition-of-criteria-for-validating-data-mining-models"></a>Définition des critères pour valider les modèles d'exploration de données  
 Les mesures de l'exploration de données s'expriment généralement en termes de précision, de fiabilité et d'utilité.  
  
 La*précision* mesure le degré de corrélation du modèle entre un résultat et les attributs des données fournies. Il existe différentes mesures de précision, mais toutes dépendent des données utilisées. Dans la réalité, les valeurs peuvent être absentes ou approximatives ; plusieurs processus peuvent avoir aussi modifié les données. Dans la phase d'exploration et de développement en particulier, vous pouvez décider d'accepter une certaine quantité d'erreurs dans les données, surtout si les données présentent des caractéristiques assez uniformes. Par exemple, un modèle qui prédit les ventes d'un magasin donné en fonction des ventes passées peut présenter un degré de corrélation très élevé et être très précis, même si ce magasin a utilisé régulièrement une mauvaise méthode de comptabilité. Par conséquent, les mesures de précision doivent être compensées par les évaluations de fiabilité.  
  
 La*fiabilité* évalue le fonctionnement d’un modèle d’exploration de données sur différents jeux de données. Un modèle d'exploration de données est fiable s'il génère le même type de prédictions ou trouve les mêmes types généraux de modèles quelles que soient les données de test fournies. Par exemple, le modèle que vous générez pour le magasin qui a utilisé la mauvaise méthode de comptabilité ne se généraliserait pas bien à d'autres magasins et ne serait par conséquent pas fiable.  
  
 *L’utilité* inclut diverses mesures indiquant si le modèle fournit des informations utiles. Par exemple, un modèle d'exploration de données qui met en corrélation l'emplacement du magasin et les ventes peut être précis et fiable, mais ne pas être utile, parce que vous ne pouvez pas généraliser ce résultat en ajoutant d'autres magasins au même emplacement. De plus, il ne répond pas à la question fondamentale de savoir pourquoi certains emplacements enregistrent plus de ventes. Vous pouvez également découvrir qu'un modèle semblant satisfaisant est en fait inutile, car il est basé sur des corrélations mutuelles des données.  
  
## <a name="tools-for-testing-and-validation-of-mining-models"></a>Outils de test et de validation des modèles d'exploration de données  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] propose plusieurs approches de validation des solutions d'exploration de données qui prennent en charge toutes les phases de la méthodologie de test de l'exploration de données.  
  
-   Partitionnement des données en jeux d'apprentissage et de test.  
  
-   Filtrage de modèles pour effectuer l'apprentissage et tester différentes combinaisons des mêmes données sources.  
  
-   Mesure de *élévation* et du *gain*. Un *graphique de courbes d’élévation* permet de visualiser l’amélioration que vous obtenez de l’utilisation d’un modèle d’exploration de données, quand vous la comparez à une estimation aléatoire.  
  
-   Exécution de la *validation croisée* des jeux de données.  
  
-   Génération de *matrices de classification*. Ces graphiques trient les estimations exactes et inexactes dans une table, de sorte que vous puissiez rapidement et facilement évaluer la précision avec laquelle le modèle prédit la valeur cible.  
  
-   Création de *nuages de points* pour évaluer l’adéquation d’une formule de régression.  
  
-   Création de *graphiques des bénéfices* qui associent le gain financier ou les coûts avec l’utilisation d’un modèle d’exploration de données, afin que vous puissiez évaluer la valeur des recommandations.  
  
 Ces mesures ne visent pas à répondre à la question de savoir si le modèle d'exploration de données répond aux besoins de votre entreprise ; en revanche, elles fournissent des mesures objectives que vous pouvez utiliser pour évaluer la fiabilité de vos données pour des analyses prévisionnelles, et pour guider votre choix en faveur d'une itération particulière sur le processus de développement.  
  
 Les rubriques de cette section fournissent une vue d'ensemble de chaque méthode et vous guident tout au long du processus de mesure de la précision des modèles que vous créez à l'aide de l'exploration de données SQL Server.  
  
### <a name="related-topics"></a>Rubriques connexes  
  
|Rubriques|Liens|  
|------------|-----------|  
|Découvrez comment configurer un jeu de données de test à l'aide d'un Assistant ou de commandes DMX|[Apprentissage et jeux de données de test](../../analysis-services/data-mining/training-and-testing-data-sets.md)|  
|Apprenez à tester la distribution et la représentativité des données dans une structure d'exploration de données|[La Validation croisée & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/cross-validation-analysis-services-data-mining.md)|  
|En savoir plus sur les types de graphique d’analyse de précision fournies.|[Graphique de courbes d’élévation & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/lift-chart-analysis-services-data-mining.md)<br /><br /> [Graphique des bénéfices & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/profit-chart-analysis-services-data-mining.md)<br /><br /> [Nuage de points & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/scatter-plot-analysis-services-data-mining.md)|  
|Apprenez à créer une matrice de classification, parfois appelée matrice de confusion, pour évaluer le nombre d'éléments positifs et négatifs vrais ou faux.|[Matrice de classification & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/classification-matrix-analysis-services-data-mining.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Outils d’exploration de données](../../analysis-services/data-mining/data-mining-tools.md)   
 [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md)   
 [Test et de tâches de Validation et de procédures & #40 ; exploration de données & #41 ;](../../analysis-services/data-mining/testing-and-validation-tasks-and-how-tos-data-mining.md)  
  
  
