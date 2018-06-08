---
title: Algorithme de régression linéaire Microsoft | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 20d052ad91a00a7b70b658ff9118dfb73736a410
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-linear-regression-algorithm"></a>Algorithme MLR (Microsoft Linear Regression)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) est une variante de l’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) qui vous permet de calculer une relation linéaire entre une variable dépendante et indépendante, puis d’utiliser cette relation pour la prédiction.  
  
 La relation se présente sous la forme d'une équation correspondant à la droite représentant le mieux une série de données. Par exemple, la droite dans le diagramme suivant est la meilleure représentation linéaire possible des données.  
  
 ![Une ligne qui modélise un jeu de données](../../analysis-services/data-mining/media/linear-regression.gif "une ligne qui modélise un jeu de données")  
  
 Pour chaque point de données du diagramme, une erreur est associée à la distance entre le point et la droite de régression. Les coefficients a et b de l’équation de régression ajustent l’angle et l’emplacement de la droite de régression. Vous pouvez obtenir l’équation de régression en ajustant a et b jusqu’à ce que la somme des erreurs associées à tous les points atteigne le plus petit nombre possible.  
  
 Il existe d'autres types de régression qui font appel à plusieurs variables ainsi que les méthodes non linéaires de régression. Toutefois, la régression linéaire est une méthode utile et connue pour modéliser une réponse à une modification dans certain facteur sous-jacent.  
  
## <a name="example"></a>Exemple  
 Vous pouvez utiliser la régression linéaire pour déterminer une relation entre deux colonnes continues. Par exemple, vous pouvez utiliser la régression linéaire pour calculer une courbe de tendance à partir de données de fabrication ou de ventes. Vous pouvez aussi utiliser la régression linéaire en précurseur du développement de modèles d'exploration de données plus complexes afin d'évaluer les relations parmi les colonnes de données.  
  
 Même s’il y a de nombreuses méthodes pour calculer la régression linéaire qui ne nécessitent pas d’outils d’exploration de données, l’avantage de l’utilisation de l’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) pour cette tâche est de pouvoir calculer et tester automatiquement toutes les relations possibles parmi les variables. Vous n'êtes pas obligé de sélectionner une méthode de calcul, telle que la résolution des moindres carrés. Toutefois, la régression linéaire peut simplifier à l'extrême les relations dans les scénarios où plusieurs facteurs affectent le résultat.  
  
## <a name="how-the-algorithm-works"></a>Fonctionnement de l'algorithme  
 L’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) est une variante de l’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees). Quand vous sélectionnez l’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression), un cas spécial de l’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) est appelé, avec des paramètres qui limitent le comportement de l’algorithme et requièrent certains types de données d’entrée. De plus, dans un modèle de régression linéaire, le jeu de données entier est utilisé pour calculer des relations dans le passage initial, alors qu'un modèle d'arbres de décision standard fractionne à plusieurs reprises les données en sous-ensembles ou arborescences plus petits.  
  
## <a name="data-required-for-linear-regression-models"></a>Données requises pour les modèles de régression linéaire  
 Lorsque vous préparez des données à utiliser dans un modèle de régression linéaire, vous devez comprendre les spécifications liées à l'algorithme. Cela comprend la quantité de données requise et le mode d'utilisation de ces données. Les spécifications pour ce type de modèle sont les suivantes :  
  
-   **Colonne à index unique** Chaque modèle doit contenir une colonne numérique ou une colonne de texte qui identifie de façon unique chaque enregistrement. Les clés composées ne sont pas autorisées.  
  
-   **Colonne prédictible** Nécessite au moins une colonne prédictible. Vous pouvez inclure dans un modèle plusieurs attributs prédictibles, mais ces attributs doivent être des types de données numériques continues. Vous ne pouvez pas utiliser un type de données datetime comme attribut prédictible même si le stockage natif pour les données est numérique.  
  
-   **Colonnes d’entrée** Les colonnes d’entrée doivent contenir des données numériques continues et recevoir le type de données approprié.  
  
 Pour plus d’informations, consultez la section Configuration requise de [Références techniques relatives à l’algorithme MLR (Microsoft Linear Regression)](../../analysis-services/data-mining/microsoft-linear-regression-algorithm-technical-reference.md).  
  
## <a name="viewing-a-linear-regression-model"></a>Affichage d'un modèle de régression linéaire  
 Pour explorer le modèle, utilisez la **visionneuse d’arborescences Microsoft**. L'arborescence d'un modèle de régression linéaire est très simple, toutes les informations relatives à l'équation de régression sont contenues dans un nœud unique. Pour plus d’informations, consultez [Explorer un modèle à l’aide de la visionneuse d’arborescences Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-tree-viewer.md).  
  
 Si vous voulez en savoir plus sur l’équation, vous pouvez également afficher les coefficients et autres informations à l’aide de la [visionneuse de l’arborescence de contenu générique Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-generic-content-tree-viewer.md).  
  
 Pour un modèle de régression linéaire, le contenu du modèle inclut des métadonnées, la formule de régression et les statistiques relatives à la distribution de valeurs d'entrée. Pour plus d’informations, consultez [Contenu du modèle d’exploration de données pour les modèles de régression linéaire &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md).  
  
## <a name="creating-predictions"></a>Création de prédictions  
 Une fois le modèle traité, les résultats sont stockés sous la forme d'un jeu de statistiques avec le formulaire de régression linéaire que vous pouvez utiliser pour élaborer des prédictions. Pour obtenir des exemples de requêtes à utiliser avec un modèle de régression linéaire, consultez [Exemples de requête de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md).  
  
 Pour obtenir des informations générales sur la création de requêtes sur des modèles d’exploration de données, consultez [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md).  
  
 En plus de créer un modèle de régression linéaire en sélectionnant l’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression), si l’attribut prédictible est un type de données numérique continu, vous pouvez créer un modèle d’arbre de décision qui contient des régressions. Dans ce cas, l'algorithme fractionne les données lorsqu'il recherche des points de séparation appropriés, mais pour certaines régions de données, il crée à la place une formule de régression. Pour plus d’informations sur les arbres de régression dans un modèle d’arbres de décision, consultez [Contenu du modèle d’exploration de données pour les modèles d’arbre de décision &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-decision-tree-models-analysis-services-data-mining.md).  
  
## <a name="remarks"></a>Notes  
  
-   Ne prend pas en charge l’utilisation du langage PMML (Predictive Model Markup Language) pour créer des modèles d’exploration de données.  
  
-   Ne prend pas en charge la création de dimensions d’exploration de données.  
  
-   Prend en charge l’extraction.  
  
-   Prend en charge l'utilisation de modèles d'exploration de données OLAP.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Référence technique de Microsoft Linear Regression algorithme](../../analysis-services/data-mining/microsoft-linear-regression-algorithm-technical-reference.md)   
 [Exemples de requêtes de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour les modèles de régression linéaire & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md)  
  
  
