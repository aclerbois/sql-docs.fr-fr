---
title: Référence technique de Microsoft Linear Regression algorithme | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 84d0d6609538bb9abdbca61e75c6691c25a45950
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-linear-regression-algorithm-technical-reference"></a>Références techniques relatives à l'algorithme MLR (Microsoft Linear Regression)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) est une version spéciale de l’algorithme MDT (Microsoft Decision Trees) qui est optimisée pour la modélisation des paires d’attributs continus. Cette rubrique explique l'implémentation de l'algorithme, décrit la façon de personnaliser le comportement de l'algorithme et fournit des liens vers des informations supplémentaires sur l'interrogation des modèles.  
  
## <a name="implementation-of-the-linear-regression-algorithm"></a>Implémentation de l'algorithme MLR (Microsoft Linear Regression)  
 L'algorithme MDT (Microsoft Decision Trees) peut être utilisé pour de nombreuses tâches : régression linéaire, classification ou analyse d'associations. Pour implémenter cet algorithme en vue d'une régression linéaire, les paramètres de l'algorithme sont contrôlés pour restreindre la croissance de l'arborescence et conserver toutes les données du modèle dans un nœud unique. En d'autres termes, bien que la régression linéaire soit basée sur un arbre de décision, l'arborescence contient uniquement une racine unique et aucune branche : toutes les données résident dans le nœud racine.  
  
 Pour cela, le paramètre *MINIMUM_LEAF_CASES* de l’algorithme est défini de façon à être supérieur ou égal au nombre total de cas utilisés par l’algorithme pour l’apprentissage du modèle d’exploration de données. Si le paramètre est défini de cette manière, l'algorithme ne crée jamais de division et effectue par conséquent une régression linéaire.  
  
 L’équation qui représente la droite de régression est de type y = ax + b et est appelée « équation de régression ». La variable Y représente la variable de sortie, X représente la variable d’entrée, et a et b sont des coefficients ajustables. Vous pouvez récupérer les coefficients, les ordonnées et d'autres informations à propos de la formule de régression en interrogeant le modèle d'exploration de données terminé. Pour plus d’informations, consultez [Exemples de requête de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md).  
  
### <a name="scoring-methods-and-feature-selection"></a>Résultat des méthodes et sélection des fonctionnalités  
 Tous les algorithmes d'exploration de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilisent automatiquement la sélection des fonctionnalités pour améliorer l'analyse et réduire la charge de traitement. La méthode utilisée pour la sélection des fonctionnalités dans la régression linéaire est le score d'intérêt et de pertinence, car le modèle prend uniquement en charge les colonnes continues. À titre de référence, la table suivante affiche la différence dans la sélection de fonctionnalités entre l'algorithme MLR (Microsoft Linear Regression) et l'algorithme MDT (Microsoft Decision Trees).  
  
|Algorithm|Méthode d'analyse|Commentaires|  
|---------------|------------------------|--------------|  
|Régression linéaire|Score d'intérêt et de pertinence|Valeur par défaut.<br /><br /> Les autres méthodes de sélection de fonctionnalités qui sont disponibles avec l'algorithme MDT s'appliquent uniquement aux variables discrètes. Par conséquent, elles ne peuvent s'appliquer aux modèles de régression linéaire.|  
|Arbres de décision|Score d'intérêt et de pertinence<br /><br /> Entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|Si des colonnes contiennent des valeurs continues non binaires, le score d'intérêt et de pertinence est utilisé pour toutes les colonnes afin de garantir la cohérence. Sinon, la méthode par défaut ou spécifiée est utilisée.|  
  
 Les paramètres d'algorithme qui contrôlent la sélection des fonctionnalités pour un modèle d'arbre de décision sont MAXIMUM_INPUT_ATTRIBUTES et MAXIMUM_OUTPUT.  
  
## <a name="customizing-the-linear-regression-algorithm"></a>Personnalisation de l'algorithme MLR (Microsoft Linear Regression)  
 L’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) prend en charge plusieurs paramètres qui affectent le comportement, les performances et la précision du modèle d’exploration de données obtenu. Vous pouvez également définir des indicateurs de modélisation sur les colonnes du modèle ou de la structure d'exploration de données pour contrôler le mode de traitement des données.  
  
### <a name="setting-algorithm-parameters"></a>Définition des paramètres de l'algorithme  
 Le tableau suivant présente les paramètres fournis pour l'algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression).  
  
|Paramètre| Description|  
|---------------|-----------------|  
|*MAXIMUM_INPUT_ATTRIBUTES*|Spécifie le nombre d'attributs d'entrée que l'algorithme peut traiter avant d'appeler la sélection des fonctionnalités. Attribuez à ce paramètre la valeur 0 pour désactiver la sélection des fonctionnalités.<br /><br /> La valeur par défaut est 255.|  
|*MAXIMUM_OUTPUT_ATTRIBUTES*|Spécifie le nombre d'attributs de sortie que l'algorithme peut traiter avant d'appeler la sélection des fonctionnalités. Attribuez à ce paramètre la valeur 0 pour désactiver la sélection des fonctionnalités.<br /><br /> La valeur par défaut est 255.|  
|*FORCE_REGRESSOR*|Force l'algorithme à utiliser les colonnes indiquées comme régresseurs, quelle que soit leur importance, telle que calculée par l'algorithme.|  
  
### <a name="modeling-flags"></a>Indicateurs de modélisation  
 L’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) prend en charge les indicateurs de modélisation suivants. Lorsque vous créez la structure d'exploration de données ou le modèle d'exploration de données, vous définissez des indicateurs de modélisation pour spécifier la façon dont les valeurs de chaque colonne sont gérées pendant l'analyse. Pour plus d’informations, consultez [Indicateurs de modélisation &#40;Exploration de données&#41;](../../analysis-services/data-mining/modeling-flags-data-mining.md).  
  
|Indicateur de modélisation|Description|  
|-------------------|-----------------|  
|NOT NULL|Indique que la colonne ne peut pas contenir de valeur Null. Une erreur est générée si Analysis Services rencontre une valeur NULL au cours de l'apprentissage du modèle.<br /><br /> S'applique aux colonnes de structure d'exploration de données.|  
|REGRESSOR|Indique que la colonne contient des valeurs numériques continues qui doivent être traitées comme variables indépendantes potentielles pendant l'analyse. S'applique aux colonnes de modèle d'exploration de données.<br /><br /> Remarque : attribuer un indicateur de régresseur sur une colonne ne garantit pas que la colonne sera utilisée comme régresseur dans le modèle final.|  
  
### <a name="regressors-in-linear-regression-models"></a>Régresseurs dans les modèles de régression linéaire  
 Les modèles de régression linéaire sont basés sur l’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees). Cependant, même si vous n’utilisez pas l’algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression), tout modèle d’arbre de décision peut contenir un arbre ou des nœuds qui représentent une régression sur un attribut continu.  
  
 Il est inutile de spécifier qu'une colonne continue représente un régresseur. L’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) partitionne le dataset en régions avec des séquences explicites même si vous ne définissez pas l’indicateur REGRESSOR sur la colonne. La différence réside dans le fait que lorsque vous définissez l'indicateur de modélisation, l'algorithme essaie de rechercher des équations de régression de type `a*C1 + b*C2 + ...` pour faire tenir les séquences dans les nœuds de l'arbre. La somme des résiduels est calculée et, si l'écart est trop grand, l'arbre est fractionné.  
  
 Par exemple, si vous prédisez le comportement d’achat de vos clients en utilisant le revenu comme attribut et que vous définissez l’indicateur de modélisation REGRESSOR sur la colonne [Revenu], l’algorithme essaie dans un premier temps de faire tenir les valeurs en utilisant une formule de régression standard. Si l'écart est trop grand, la formule de régression est abandonnée et l'arbre est fractionné sur un autre attribut. L’algorithme MDT essaie ensuite de faire tenir un régresseur pour le revenu dans chacune des branches après le fractionnement.  
  
 Vous pouvez utiliser le paramètre FORCED_REGRESSOR pour faire en sorte que l'algorithme utilise un régresseur particulier. Ce paramètre peut être utilisé avec les algorithmes MDT et MLR.  
  
## <a name="requirements"></a>Spécifications  
 Un modèle de régression linéaire doit contenir une colonne clé, des colonnes d'entrée et au moins une colonne prédictible.  
  
### <a name="input-and-predictable-columns"></a>Colonnes d'entrée et prédictibles  
 L’algorithme MLR ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) prend en charge les colonnes d’entrée et les colonnes prédictibles répertoriées dans le tableau suivant. Pour plus d’informations sur la signification des types de contenu utilisés dans un modèle d’exploration de données, consultez [Types de contenu &#40;Exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md).  
  
|Colonne|Types de contenu|  
|------------|-------------------|  
|Attribut d'entrée|Continu, Cyclique, Clé, Table et Ordonné|  
|Attribut prédictible|Continu, Cyclique et Ordonné|  
  
> [!NOTE]  
>  Les types de contenu**Cyclique** et **Trié** sont pris en charge, mais l'algorithme les traite comme des valeurs discrètes et n'effectue pas de traitement spécial.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme de régression linéaire Microsoft](../../analysis-services/data-mining/microsoft-linear-regression-algorithm.md)   
 [Exemples de requêtes de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour les modèles de régression linéaire & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md)  
  
  
