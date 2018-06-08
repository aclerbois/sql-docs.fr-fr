---
title: Techniques de l’algorithme d’arbres de décision Microsoft | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 186245b549d8aa9370551311e792067e0131e076
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-decision-trees-algorithm-technical-reference"></a>Références techniques relatives à l'algorithme MDT (Microsoft Decision Trees)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) est un algorithme hybride qui incorpore des méthodes différentes pour créer une arborescence et prend en charge plusieurs tâches analytiques, dont la régression, la classification et l'association. L'algorithme MDT (Microsoft Decision Trees) prend en charge la modélisation des attributs discrets et continus.  
  
 Cette rubrique explique l'implémentation de l'algorithme, décrit la façon de personnaliser le comportement de l'algorithme pour différentes tâches et fournit des liens vers des informations supplémentaires sur l'interrogation des modèles d'arbre de décision.  
  
## <a name="implementation-of-the-decision-trees-algorithm"></a>Implémentation de l'algorithme MDT (Microsoft Decision Trees)  
 L'algorithme MDT (Microsoft Decision Trees) applique l'approche bayésienne pour acquérir les modèles causaux d'interaction en obtenant des distributions ultérieures approximatives pour les modèles. Pour une explication détaillée de cette approche, consultez l'article sur le site Microsoft Research, par [Structure et apprentissage de paramètres](http://go.microsoft.com/fwlink/?LinkId=237640&clcid=0x409).  
  
 La méthodologie d'évaluation de la valeur d'information des *à priori* nécessaires pour l'apprentissage repose sur l' *équivalence de probabilité*. Cette hypothèse indique que les données ne doivent pas aider à discriminer des structures de réseau qui représentent autrement les mêmes assertions d'indépendance conditionnelle. Il est supposé que chaque cas a un réseau antérieur bayésien unique et une mesure unique de confiance pour ce réseau.  
  
 Utilisant ces réseaux antérieurs, l'algorithme calcule ensuite les *probabilités postérieures* relatives de structures de réseau selon les données d'apprentissage actuelles et identifie les structures de réseau qui ont les probabilités postérieures les plus élevées.  
  
 L'algorithme MDT (Microsoft Decision Trees) utilise des méthodes différentes pour calculer la meilleure arborescence. La méthode utilisée dépend de la tâche, qui peut être une régression linéaire, une classification ou une analyse d'association. Un modèle unique peut contenir plusieurs arborescences pour des attributs prédictibles différents. De plus, chaque arborescence peut contenir plusieurs branches, selon le nombre d'attributs et de valeurs existant dans les données. La forme et la profondeur de l'arborescence générée dans un modèle particulier dépendent de la méthode de résultat et d'autres paramètres utilisés. Les modifications des paramètres peuvent également affecter le lieu de fractionnement des nœuds.  
  
### <a name="building-the-tree"></a>Génération de l'arborescence  
 Lorsque l'algorithme MDT (Microsoft Decision Trees) crée le jeu de valeurs d'entrée possibles, il effectue *feature selection* pour identifier les attributs et valeurs qui fournissent le plus d'informations et ne prend pas en considération les valeurs qui sont très rares. L'algorithme regroupe également des valeurs dans des *bacs*pour créer des regroupements de valeurs qui peuvent être traitées comme une unité afin d'optimiser les performances.  
  
 Une arborescence est générée en déterminant les corrélations entre une entrée et le résultat ciblé. Une fois que tous les attributs ont été corrélés, l'algorithme identifie l'attribut unique qui sépare le mieux les résultats. Ce point de meilleure séparation est mesuré en utilisant une équation qui calcule le gain d'informations. L'attribut qui a le meilleur score en termes de gain d'informations est utilisé pour diviser les cas en sous-ensembles, qui subissent ensuite une analyse récursive par le même processus jusqu'à ce que l'arborescence ne puisse plus être fractionnée.  
  
 L'équation exacte utilisée pour évaluer le gain d'informations dépend des paramètres définis lorsque vous avez créé l'algorithme, du type de données de la colonne prédictible et du type de données de l'entrée.  
  
### <a name="discrete-and-continuous-inputs"></a>Entrées discrètes et continues  
 Lorsque l'attribut prédictible est discret et les entrées sont discrètes, le comptage des résultats par entrée consiste à créer une matrice et à générer des scores pour chaque cellule de la matrice.  
  
 Toutefois, lorsque l'attribut prédictible est discret et les entrées sont continues, l'entrée des colonnes continues est automatiquement discrétisée. Vous pouvez accepter la valeur par défaut et faire en sorte que [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] recherche le nombre optimal de bacs, ou vous pouvez contrôler la façon de discrétiser les entrées continues en définissant les propriétés <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationMethod%2A> et <xref:Microsoft.AnalysisServices.ScalarMiningStructureColumn.DiscretizationBucketCount%2A> . Pour plus d’informations, consultez [Modifier la discrétisation d’une colonne dans un modèle d’exploration de données](../../analysis-services/data-mining/change-the-discretization-of-a-column-in-a-mining-model.md).  
  
 Pour les attributs continus, l'algorithme utilise la régression linéaire pour déterminer où un arbre de décision se divise.  
  
 Lorsque l'attribut prédictible est un type de données numériques continues, la sélection des fonctionnalités est également appliquée aux sorties pour réduire le nombre possible de résultats et générer le modèle plus vite. Vous pouvez modifier le seuil pour la sélection des fonctionnalités et augmenter ou réduire ainsi le nombre de valeurs possibles en définissant le paramètre MAXIMUM_OUTPUT_ATTRIBUTES.  
  
 Pour une explication plus détaillée de la façon dont l'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) utilise des colonnes prédictibles discrètes, consultez [Réseaux bayésiens : connaissance et données statistiques](http://research.microsoft.com/en-us/um/people/heckerman/hgc94uai.pdf). Pour plus d’informations sur le fonctionnement de l’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) avec une colonne prédictible continue, consultez l’annexe disponible dans le document [Autoregressive Tree Models for Time-Series Analysis](http://go.microsoft.com/fwlink/?LinkId=45966).  
  
### <a name="scoring-methods-and-feature-selection"></a>Résultat des méthodes et sélection des fonctionnalités  
 L'algorithme MDT (Microsoft Decision Trees) offre trois formules pour définir le score du gain d'informations : l'entropie de Shannon, le réseau bayésien avec a priori K2 et le réseau bayésien de Dirichlet avec a priori uniforme. Les trois méthodes sont bien établies dans le champ d'exploration de données. Nous vous recommandons d'expérimenter des paramètres et des méthodes différents pour déterminer ceux qui fournissent les meilleurs résultats. Pour plus d'informations sur ces méthodes de calcul de score, consultez [Feature Selection](http://msdn.microsoft.com/library/73182088-153b-4634-a060-d14d1fd23b70).  
  
 Tous les algorithmes d'exploration de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilisent automatiquement la sélection des fonctionnalités pour améliorer l'analyse et réduire la charge de traitement. La méthode utilisée pour la sélection des fonctionnalités dépend de l'algorithme utilisé pour générer le modèle. Les paramètres d'algorithme qui contrôlent la sélection des fonctionnalités pour un modèle d'arbre de décision sont MAXIMUM_INPUT_ATTRIBUTES et MAXIMUM_OUTPUT.  
  
|Algorithm|Méthode d'analyse|Commentaires|  
|---------------|------------------------|--------------|  
|Arbres de décision|Score d'intérêt et de pertinence<br /><br /> Entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|Si des colonnes contiennent des valeurs continues non binaires, le score d'intérêt et de pertinence est utilisé pour toutes les colonnes afin de garantir la cohérence. Sinon, la méthode par défaut ou spécifiée est utilisée.|  
|Régression linéaire|Score d'intérêt et de pertinence|La régression linéaire utilise uniquement l'intérêt et la pertinence car elle ne prend en charge que les colonnes continues.|  
  
### <a name="scalability-and-performance"></a>Performances et extensibilité  
 La classification est une stratégie d'exploration de données importante. En général, la quantité d'informations nécessaire pour classifier les cas grandit de façon directement proportionnelle au nombre d'enregistrements d'entrée. Cela limite la taille des données qui peuvent être classées. L'algorithme MDT (Microsoft Decision Trees) utilise les méthodes suivantes pour résoudre ces problèmes, améliorer les performances et éliminer les restrictions de mémoire :  
  
-   Sélection des fonctionnalités pour optimiser la sélection des attributs.  
  
-   Score bayésien pour contrôler la croissance de l'arbre.  
  
-   Optimisation de mise en bac pour les attributs continus.  
  
-   Regroupement dynamique de valeurs d'entrée pour déterminer les valeurs les plus importantes.  
  
 L'algorithme MDT (Microsoft Decision Trees) est rapide et évolutif. Il a été conçu pour être facilement mis en parallèle, ce qui signifie que tous les processeurs fonctionnent ensemble pour générer un seul modèle cohérent. La combinaison de ces caractéristiques fait du classifieur d'arbre de décision un outil idéal pour l'exploration de données.  
  
 Si les contraintes de performances sont importantes, vous pouvez améliorer le temps de traitement pendant l'apprentissage d'un modèle d'arbre de décision en utilisant les méthodes suivantes. Toutefois, dans cette éventualité, sachez que la suppression d'attributs en vue d'améliorer les performances de traitement modifiera les résultats du modèle et le rendra peut-être moins représentatif de la totalité de la population.  
  
-   Augmentez la valeur du paramètre COMPLEXITY_PENALTY pour limiter la croissance de l'arbre.  
  
-   Limitez le numéro d'éléments dans les modèles d'association pour limiter le nombre d'arborescences générées.  
  
-   Augmentez la valeur du paramètre MINIMUM_SUPPORT pour éviter le surajustement.  
  
-   Restreignez le nombre de valeurs discrètes pour tout attribut à 10 ou moins. Vous pouvez essayer de regrouper les valeurs de différentes façons dans les différents modèles.  
  
    > [!NOTE]  
    >  Vous pouvez utiliser les outils d'exploration de données disponibles dans  [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] pour visualiser la distribution de valeurs dans vos données et regrouper convenablement vos valeurs avant de commencer l'exploration de données. Pour plus d’informations, consultez [Tâche de profilage des données et visionneuse](../../integration-services/control-flow/data-profiling-task-and-viewer.md). Vous pouvez également utiliser les [compléments d’exploration de données pour Excel 2007](http://www.microsoft.com/downloads/details.aspx?FamilyID=7C76E8DF-8674-4C3B-A99B-55B17F3C4C51)pour explorer, regrouper et réétiqueter les données dans Microsoft Excel.  
  
## <a name="customizing-the-decision-trees-algorithm"></a>Personnalisation de l'algorithme MDT (Microsoft Decision Trees).  
 L'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) prend en charge des paramètres qui affectent les performances et la précision du modèle d'exploration de données résultant. Vous pouvez également définir des indicateurs de modélisation sur les colonnes du modèle ou de la structure d'exploration de données pour contrôler le mode de traitement des données.  
  
> [!NOTE]  
>  L'algorithme MDT est disponible dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; toutefois, certains paramètres avancés permettant de personnaliser le comportement de l'algorithme MDT sont disponibles uniquement dans les éditions spécifiques de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités qui sont prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [fonctionnalités prises en charge par les éditions de SQL Server 2012](http://go.microsoft.com/fwlink/?linkid=232473) (http://go.microsoft.com/fwlink/?linkid=232473).  
  
### <a name="setting-algorithm-parameters"></a>Définition des paramètres de l'algorithme  
 Le tableau suivant décrit les paramètres que vous pouvez utiliser avec l'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees).  
  
 *COMPLEXITY_PENALTY*  
 Contrôle la croissance de l'arbre de décision. Une valeur faible entraîne l'augmentation du nombre de fractionnements, alors qu'une valeur élevée entraîne la diminution du nombre de fractionnements. La valeur par défaut dépend du nombre d'attributs pour un modèle particulier, comme cela est décrit dans la liste suivante :  
  
-   De 1 à 9 attributs, la valeur par défaut est égale à 0,5.  
  
-   De 10 à 99 attributs, la valeur par défaut est égale à 0,9.  
  
-   À partir de 100 attributs, la valeur par défaut est égale à 0,99.  
  
 *FORCE_REGRESSOR*  
 Force l'algorithme à utiliser les colonnes spécifiées en tant que régresseurs, quelle que soit leur importance selon les calculs de l'algorithme. Ce paramètre est utilisé uniquement pour les arbres de décision qui prévoient un attribut continu.  
  
> [!NOTE]  
>  En définissant ce paramètre, vous forcez l'algorithme à essayer d'utiliser l'attribut comme un régresseur. Toutefois, l'utilisation réelle de l'attribut en tant que régresseur dans le modèle final dépend des résultats d'analyse. Vous pouvez savoir quelles colonnes ont été utilisées comme régresseurs en interrogeant le contenu du modèle.  
  
 [Disponible uniquement dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ]  
  
 *MAXIMUM_INPUT_ATTRIBUTES*  
 Spécifie le nombre d'attributs d'entrée que l'algorithme peut traiter avant d'appeler la sélection des fonctionnalités.  
  
 La valeur par défaut est 255.  
  
 Attribuez à ce paramètre la valeur 0 pour désactiver la sélection des fonctionnalités.  
  
 [Disponible uniquement dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]]  
  
 *MAXIMUM_OUTPUT_ATTRIBUTES*  
 Spécifie le nombre d'attributs de sortie que l'algorithme peut traiter avant d'appeler la sélection des fonctionnalités.  
  
 La valeur par défaut est 255.  
  
 Attribuez à ce paramètre la valeur 0 pour désactiver la sélection des fonctionnalités.  
  
 [Disponible uniquement dans certaines éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]]  
  
 *MINIMUM_SUPPORT*  
 Spécifie le nombre minimal de cas de nœud terminal requis pour générer un fractionnement dans l'arbre de décision.  
  
 La valeur par défaut est 10.  
  
 Vous devrez peut-être augmenter cette valeur si le dataset est très grand, pour éviter le surapprentissage.  
  
 *SCORE_METHOD*  
 Spécifie la méthode utilisée pour calculer le résultat de la division. Les options suivantes sont disponibles :  
  
|ID|Nom|  
|--------|----------|  
|1|Entropie|  
|3|Bayésien avec a priori K2|  
|4|Équivalent bayésien de Dirichlet (BDE) avec à priori uniforme<br /><br /> (par défaut)|  
  
 La valeur par défaut est 4 ou BDE.  
  
 Pour obtenir une explication de ces méthodes, consultez [Feature Selection](http://msdn.microsoft.com/library/73182088-153b-4634-a060-d14d1fd23b70).  
  
 *SPLIT_METHOD*  
 Spécifie la méthode utilisée pour fractionner le nœud. Les options suivantes sont disponibles :  
  
|ID|Nom|  
|--------|----------|  
|1|**Binary:** Indique qu'indépendamment du nombre réel de valeurs pour l'attribut, l'arborescence doit être fractionnée en deux branches.|  
|2|**Complete:** Indique que l'arborescence peut créer autant de divisions qu'il y a de valeurs d'attribut.|  
|3|**Both:** Spécifie qu'Analysis Services peut déterminer si un fractionnement binaire ou complet doit être utilisé pour produire les meilleurs résultats.|  
  
 La valeur par défaut est 3.  
  
### <a name="modeling-flags"></a>Indicateurs de modélisation  
 L'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) prend en charge les indicateurs de modélisation suivants. Lorsque vous créez la structure d'exploration de données ou le modèle d'exploration de données, vous définissez des indicateurs de modélisation pour spécifier la façon dont les valeurs de chaque colonne sont gérées pendant l'analyse. Pour plus d’informations, consultez [Indicateurs de modélisation &#40;Exploration de données&#41;](../../analysis-services/data-mining/modeling-flags-data-mining.md).  
  
|Indicateur de modélisation|Description|  
|-------------------|-----------------|  
|MODEL_EXISTENCE_ONLY|Signifie que la colonne sera considérée comme ayant deux états possibles : **Missing** et **Existing**. Une valeur NULL est une valeur manquante.<br /><br /> S'applique aux colonnes de modèle d'exploration de données.|  
|NOT NULL|Indique que la colonne ne peut pas contenir de valeur Null. Une erreur est générée si Analysis Services rencontre une valeur NULL au cours de l'apprentissage du modèle.<br /><br /> S'applique aux colonnes de structure d'exploration de données.|  
  
### <a name="regressors-in-decision-tree-models"></a>Régresseurs dans les modèles d'arbre de décision  
 Même si vous n'utilisez pas l'algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression), tout modèle d'arbre de décision avec des entrées et des sorties numériques continues peut potentiellement contenir des nœuds qui représentent une régression sur un attribut continu.  
  
 Il est inutile de spécifier qu'une colonne de données numériques continues représente un régresseur. L'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) utilise automatiquement la colonne en tant que régresseur potentiel et partitionne le dataset en régions avec des séquences explicites même si vous ne définissez pas l'indicateur REGRESSOR sur la colonne.  
  
 Toutefois, vous pouvez utiliser le paramètre FORCED_REGRESSOR pour faire en sorte que l'algorithme utilise un régresseur particulier. Ce paramètre peut être utilisé uniquement avec les [!INCLUDE[msCoName](../../includes/msconame-md.md)] algorithmes MDT et [!INCLUDE[msCoName](../../includes/msconame-md.md)] MLR. Lorsque vous définissez l'indicateur de modélisation, l'algorithme essaie de rechercher des équations de régression de type `a*C1 + b*C2 + ...` pour faire tenir les séquences dans les nœuds de l'arbre. La somme des résiduels est calculée et, si l'écart est trop grand, l'arbre est fractionné.  
  
 Par exemple, si vous prédisez le comportement d'achat de vos clients en utilisant **Income** comme attribut et que vous définissez l'indicateur de modélisation REGRESSOR sur la colonne, l'algorithme essaie tout d'abord de faire tenir les valeurs **Income** en utilisant une formule de régression standard. Si l'écart est trop grand, la formule de régression est abandonnée et l'arbre est fractionné sur un autre attribut. L'algorithme MDT essaie ensuite de faire tenir un régresseur pour le revenu dans chacune des branches après le fractionnement.  
  
## <a name="requirements"></a>Spécifications  
 Un modèle d'arbre de décision doit contenir une colonne clé, des colonnes d'entrée et au moins une colonne prédictible.  
  
### <a name="input-and-predictable-columns"></a>Colonnes d'entrée et prédictibles  
 L'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) prend en charge les colonnes d'entrée et les colonnes prédictibles répertoriées dans le tableau suivant. Pour plus d’informations sur la signification des types de contenu utilisés dans un modèle d’exploration de données, consultez [Types de contenu &#40;exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md).  
  
|Colonne|Types de contenu|  
|------------|-------------------|  
|Attribut d'entrée|Continu, cyclique, discret, discrétisé, clé, trié, table|  
|Attribut prédictible|Continu, cyclique, discret, discrétisé, trié, table|  
  
> [!NOTE]  
>  Les types de contenu Cyclique et Trié sont pris en charge, mais l'algorithme les traite comme des valeurs discrètes et n'effectue pas de traitement spécial.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme MDT (Microsoft Decision Trees)](../../analysis-services/data-mining/microsoft-decision-trees-algorithm.md)   
 [Exemples de requête de modèle d’arborescences de décision](../../analysis-services/data-mining/decision-trees-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour les modèles d’arbre de décision & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-decision-tree-models-analysis-services-data-mining.md)  
  
  
