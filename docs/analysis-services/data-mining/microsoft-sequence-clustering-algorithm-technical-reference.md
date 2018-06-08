---
title: Microsoft Sequence Clustering techniques algorithme | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cf5f652cc2cec77fdbcb488710886441788a0631
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-sequence-clustering-algorithm-technical-reference"></a>Références techniques relatives à l'algorithme MSC (Microsoft Sequence Clustering)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'algorithme MSC (Microsoft Sequence Clustering) est un algorithme hybride qui utilise l'analyse en chaîne de Markov pour identifier les séquences ordonnées et associe les résultats de cette analyse aux techniques de clustering pour générer des clusters basés sur les séquences et les autres attributs du modèle. Cette rubrique décrit l'implémentation de l'algorithme, la personnalisation de l'algorithme et les besoins spéciaux pour les modèles Sequence Clustering.  
  
 Pour plus d'informations générales à propos de l'algorithme, y compris les procédures permettant de parcourir et d'interroger des modèles Sequence Clustering, consultez [Microsoft Sequence Clustering Algorithm](../../analysis-services/data-mining/microsoft-sequence-clustering-algorithm.md).  
  
## <a name="implementation-of-the-microsoft-sequence-clustering-algorithm"></a>Implémentation de l'algorithme Microsoft Sequence Clustering  
 Le modèle Microsoft Sequence Clustering utilise des modèles de Markov pour identifier des séquences et déterminer la probabilité des séquences. Un modèle de Markov est un graphique qui stocke les transitions entre des états différents. L'algorithme MSC (Microsoft Sequence Clustering) utilise des chaînes de Markov d'ordre n, pas un modèle de Markov masqué.  
  
 Le nombre d'ordres dans une chaîne de Markov vous indique le nombre d'états utilisés pour déterminer la probabilité des états actuels. Dans un modèle de Markov de premier ordre, la probabilité de l'état actuel dépend uniquement de l'état précédent. Dans une chaîne de Markov de deuxième-ordre, la probabilité d'un état dépend des deux états précédents, etc. Pour chaque chaîne de Markov, une matrice de transition stocke les transitions de chaque combinaison d'états. À mesure que la longueur de la chaîne de Markov augmente, la taille de la matrice augmente également de façon exponentielle et la matrice devient extrêmement allouée. Le temps de traitement augmente aussi proportionnellement.  
  
 IL peut être utile de visualiser la chaîne en utilisant l'exemple d'analyse des parcours de visite, qui analyse les visites aux pages Web d'un site. Chaque utilisateur crée une longue séquence de clics pour chaque session. Lorsque vous créez un modèle pour analyser le comportement de l'utilisateur d'un site Web, le jeu de données utilisé pour l'apprentissage est une séquence d'URL, convertie en un graphique qui inclut le nombre de toutes les instances du même chemin d'accès de clic. Par exemple, le graphique contient la probabilité que l'utilisateur aille de la page 1 à la page 2 (10 %), la probabilité que l'utilisateur aille de la page 1 à la page 3 (20 %), etc. Lorsque vous réunissez tous les chemins d'accès et les éléments des chemins d'accès possibles, vous obtenez un graphique qui peut être beaucoup plus long et plus complexe qu'un chemin d'accès unique observé.  
  
 Par défaut, L'algorithme Microsoft Sequence Clustering utilise la méthode de clustering EM (Expectation Maximization). Pour plus d’informations, consultez [Références techniques relatives à l’algorithme de gestion de clusters Microsoft](../../analysis-services/data-mining/microsoft-clustering-algorithm-technical-reference.md).  
  
 Les cibles du clustering sont à la fois les attributs séquentiels et non-séquence. Chaque cluster est sélectionné aléatoirement à l'aide d'une fonction de répartition. Chaque cluster a une chaîne de Markov qui représente le jeu complet de chemins d'accès et une matrice qui contient les transitions d'état de séquence et les probabilités. Selon la distribution initiale, la règle de Bayes est utilisée pour calculer la probabilité de tout attribut, y compris une séquence, dans un cluster spécifique.  
  
 L'algorithme MSC (Microsoft Sequence Clustering) prend en charge les attributs non-séquence qui s'ajoutent au modèle. Cela signifie que ces attributs supplémentaires sont associés aux attributs séquentiels pour créer des clusters de cas avec des attributs similaires, comme dans un modèle de clustering classique.  
  
 Un modèle Sequence Clustering a tendance à créer beaucoup plus de clusters qu'un modèle de clustering typique. Par conséquent, l'algorithme MSC (Microsoft Sequence Clustering) effectue une *décomposition de cluster*pour séparer les clusters selon les séquences et d'autres attributs.  
  
### <a name="feature-selection-in-a-sequence-clustering-model"></a>Sélection des fonctionnalités dans un modèle Sequence Clustering  
 La sélection des fonctionnalités n'est pas appelée lors de la génération de séquences. Toutefois, elle s'applique à l'étape de clustering.  
  
|Type du modèle|Méthode de sélection des fonctionnalités|Commentaires|  
|----------------|------------------------------|--------------|  
|Sequence Clustering|Non utilisée|La sélection des fonctionnalités n'est pas appelée. Toutefois, vous pouvez contrôler le comportement de l'algorithme en définissant la valeur des paramètres MINIMUM_SUPPORT et MNIMUM_PROBABILITY.|  
|Clustering|Score d'intérêt et de pertinence|Bien que l'algorithme de clustering puisse utiliser des algorithmes discrets ou discrétisés, le score de chaque attribut est calculé en tant que distance et est continu. Par conséquent, le score d'intérêt et de pertinence est utilisé.|  
  
 Pour plus d'informations, consultez [Feature Selection](http://msdn.microsoft.com/library/73182088-153b-4634-a060-d14d1fd23b70).  
  
### <a name="optimizing-performance"></a>Optimisation des performances  
 L'algorithme MSC (Microsoft Sequence Clustering) prend en charge différentes méthodes pour optimiser le traitement :  
  
-   Contrôle du nombre de clusters généré, en définissant une valeur pour le paramètre CLUSTER_COUNT.  
  
-   Réduction du nombre de séquences incluses comme attributs, en augmentant la valeur du paramètre MINIMUM_SUPPORT. En conséquence, les séquences rares sont éliminées.  
  
-   Réduction de la complexité avant le traitement du modèle, en groupant des attributs associés.  
  
 En général, vous pouvez optimiser les performances d’un mode de chaîne de Markov d’ordre n de plusieurs façons :  
  
-   Contrôle de la longueur des séquences possibles.  
  
-   Réduction par programmation de la valeur de n.  
  
-   Unique stockage des probabilités qui dépassent un seuil spécifié.  
  
 Une discussion approfondie sur ces méthodes dépasse l'objectif de cette rubrique.  
  
## <a name="customizing-the-sequence-clustering-algorithm"></a>Personnalisation de l'algorithme Sequence Clustering  
 L'algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering prend en charge des paramètres qui affectent le comportement, les performances et la précision du modèle d'exploration de données résultant. Vous pouvez également modifier le comportement du modèle terminé en définissant des indicateurs de modélisation qui contrôlent la façon dont l'algorithme traite les données d'apprentissage.  
  
### <a name="setting-algorithm-parameters"></a>Définition des paramètres de l'algorithme  
 Le tableau suivant décrit les paramètres qui peuvent être utilisés avec l'algorithme Microsoft Sequence Clustering.  
  
 CLUSTER_COUNT  
 Spécifie le nombre approximatif de clusters que l'algorithme doit générer. S'il est impossible de générer ce nombre approximatif de clusters à partir des données, l'algorithme génère autant de clusters que possible. Si le paramètre CLUSTER_COUNT est défini sur 0, l'algorithme utilise l'heuristique pour déterminer de manière optimale le nombre de clusters à générer.  
  
 La valeur par défaut est 10.  
  
> [!NOTE]  
>  Lorsqu'un nombre différent de zéro est spécifié, l'algorithme cherche à trouver le nombre spécifié, mais peut  trouver un nombre plus ou moins élevé.  
  
 MINIMUM_SUPPORT  
 Spécifie le nombre minimal de cas requis pour la prise en charge un attribut en vue de créer un cluster.  
  
 La valeur par défaut est 10.  
  
 MAXIMUM_SEQUENCE_STATES  
 Spécifie le nombre maximal d'états qu'une séquence peut posséder.  
  
 Si cette valeur est supérieure à 100, l'algorithme peut créer un modèle qui ne fournit pas d'informations significatives.  
  
 La valeur par défaut est 64.  
  
 MAXIMUM_STATES  
 Spécifie le nombre maximal d'états pour un attribut non-séquence que l'algorithme prend en charge. Si le nombre d’états pour un attribut non-séquence est supérieur au nombre maximal d’états, l’algorithme emploie les états les plus utilisés de l’attribut et traite les autres états comme **Manquant**.  
  
 La valeur par défaut est 100.  
  
### <a name="modeling-flags"></a>Indicateurs de modélisation  
 Les indicateurs de modélisation suivants sont pris en charge pour une utilisation avec l'algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering.  
  
 NOT NULL  
 Indique que la colonne ne peut pas contenir de valeur Null. Une erreur est générée si Analysis Services rencontre une valeur NULL au cours de l'apprentissage du modèle.  
  
 S'applique à la colonne de structure d'exploration de données.  
  
 MODEL_EXISTENCE_ONLY  
 Signifie que la colonne sera considérée comme ayant deux états possibles : **Missing** et **Existing**. La valeur Null est traitée comme une valeur **Missing** .  
  
 S'applique à la colonne du modèle d'exploration de données.  
  
 Pour plus d’informations sur l’utilisation de valeurs manquantes dans les modèles d’exploration de données et pour savoir comment les valeurs manquantes affectent les scores de probabilité, consultez [Valeurs manquantes &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/missing-values-analysis-services-data-mining.md).  
  
## <a name="requirements"></a>Spécifications  
 La table de cas doit avoir une colonne d'ID de cas. La table de cas peut éventuellement contenir d'autres colonnes qui stockent des attributs  propos du cas.  
  
 L'algorithme MSC (Microsoft Sequence Clustering) requiert des informations de séquence, stockées en tant que table imbriquée. La table imbriquée doit avoir une seule colonne Key Sequence. Une colonne **Key Sequence** peut contenir tout type de données pouvant être triées, y compris les types de données de chaîne, mais la colonne doit contenir des valeurs uniques pour chaque cas. De plus, avant de traiter le modèle, vous devez vérifier que la table de cas et la table imbriquée sont toutes les deux triées par ordre croissant sur la clé qui associe les tables.  
  
> [!NOTE]  
>  Si vous créez un modèle qui utilise l'algorithme Microsost Sequence mais n'utilise pas de colonne de séquence, le modèle résultant ne contient pas de séquences, mais groupe simplement des cas selon d'autres attributs inclus dans le modèle.  
  
### <a name="input-and-predictable-columns"></a>Colonnes d'entrée et prédictibles  
 L'algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering prend en charge les colonnes d'entrée et les colonnes prédictibles répertoriées dans le tableau suivant. Pour plus d’informations sur la signification des types de contenu utilisés dans un modèle d’exploration de données, consultez [Types de contenu &#40;exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md).  
  
|Colonne|Types de contenu|  
|------------|-------------------|  
|Attribut d'entrée|Continu, cyclique, discret, discrétisé, clé, séquence de clés, table et trié|  
|Attribut prédictible|Continu, cyclique, discret, discrétisé, table et trié|  
  
## <a name="remarks"></a>Notes  
  
-   Utilisez la fonction [PredictSequence &#40;DMX&#41;](../../dmx/predictsequence-dmx.md) pour la prédiction des séquences. Pour plus d’informations sur les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui prennent en charge la prédiction de séquence, consultez [fonctionnalités prises en charge par les éditions de SQL Server 2012](http://go.microsoft.com/fwlink/?linkid=232473) (http://go.microsoft.com/fwlink/?linkid=232473).  
  
-   L’algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering ne prend pas en charge l’utilisation du langage PMML (Predictive Model Markup Language) pour créer des modèles d’exploration de données.  
  
-   L'algorithme [!INCLUDE[msCoName](../../includes/msconame-md.md)] Sequence Clustering prend en charge l'extraction, l'utilisation de modèles d'exploration de données OLAP et l'utilisation de dimensions d'exploration de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme de Microsoft Sequence Clustering](../../analysis-services/data-mining/microsoft-sequence-clustering-algorithm.md)   
 [Sequence Clustering Model Query Examples](../../analysis-services/data-mining/sequence-clustering-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour les modèles de Clustering de séquence & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-sequence-clustering-models.md)  
  
  
