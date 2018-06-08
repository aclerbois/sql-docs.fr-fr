---
title: Techniques de l’algorithme de gestion de clusters Microsoft | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: eb86c8271599b56deb27fd3143fd205c11f52f35
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-clustering-algorithm-technical-reference"></a>Références techniques relatives à l'algorithme de gestion de clusters Microsoft
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Cette section explique l'implémentation de l'algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)], en particulier les paramètres que vous pouvez utiliser pour contrôler le comportement des modèles de clustering. Vous y trouverez également des informations sur la façon d'améliorer les performances lors de la création et du traitement des modèles de clustering.  
  
 Pour plus d'informations sur l'utilisation des modèles de clustering, consultez les rubriques suivantes :  
  
-   [Contenu du modèle d’exploration de données pour le Clustering des modèles & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-clustering-models-analysis-services-data-mining.md)  
  
-   [Exemples de requêtes modèle de clustering](../../analysis-services/data-mining/clustering-model-query-examples.md)  
  
## <a name="implementation-of-the-microsoft-clustering-algorithm"></a>Implémentation de l'algorithme de gestion de clusters Microsoft  
 L'algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] fournit deux méthodes pour la création de clusters et l'attribution de points de données aux clusters. La première méthode, l’algorithme *K-means* , est une méthode de type hard clustering. Cela signifie qu'un point de données peut appartenir à un seul cluster et qu'une probabilité unique est calculée pour l'appartenance de chaque point de données à ce cluster. La deuxième méthode, *EM* (Expectation Maximization), est une méthode de type *soft clustering* . Cela signifie qu'un point de données appartient toujours à plusieurs clusters et qu'une probabilité est calculée pour chaque combinaison point de données/cluster.  
  
 Vous pouvez choisir l’algorithme à utiliser en définissant le paramètre *CLUSTERING_METHOD* . La méthode de clustering par défaut est EM évolutif.  
  
### <a name="em-clustering"></a>Clustering EM  
 Dans le cadre du clustering EM, l'algorithme affine de manière itérative un modèle de cluster initial pour l'adapter aux données et détermine la probabilité qu'un point de données existe dans un cluster. L'algorithme termine le processus lorsque le modèle probabiliste est adapté aux données. La fonction utilisée pour déterminer le degré d'adaptation est le logarithme du rapport de vraisemblance des données pour le modèle donné.  
  
 Si des clusters vides sont générés lors du processus ou si l'appartenance d'un ou plusieurs des clusters tombe en dessous d'un seuil donné, les clusters avec des populations faibles sont réensemencés à de nouveaux points et l'algorithme EM est réexécuté.  
  
 Les résultats de la méthode de clustering EM sont probabilistes. Cela signifie que chaque point de données appartient à tous les clusters, mais que chaque affectation d'un point de données à un cluster a une probabilité différente. Étant donné que la méthode autorise le chevauchement des clusters, la somme des éléments dans tous les clusters peut dépasser le nombre total d'éléments dans le jeu d'apprentissage. Dans les résultats du modèle d'exploration de données, les scores qui indiquent la prise en charge sont ajustés pour tenir compte de cela.  
  
 L'algorithme EM est l'algorithme par défaut utilisé dans les modèles de clustering Microsoft. Cela s'explique par le fait qu'il offre plusieurs avantages par rapport au clustering K-means :  
  
-   nécessité d'une analyse de base de données au plus ;  
  
-   possibilité de fonctionner en dépit d'une mémoire RAM limitée ;  
  
-   possibilité d'utiliser un curseur avant uniquement ;  
  
-   meilleures performances que les approches d'échantillonnage.  
  
 L'implémentation Microsoft propose deux options : EM évolutif et EM non évolutif. Par défaut, dans la méthode EM évolutif, les 50 000 premiers enregistrements sont utilisés pour ensemencer l'analyse initiale. Si l'opération réussit, le modèle se limite à ces données. Si l'adaptation du modèle échoue avec 50 000 enregistrements, 50 000 autres enregistrements sont lus. Dans la méthode EM non évolutif, le dataset entier est lu quelle que soit sa taille. Cette méthode peut créer des clusters plus précis, mais les besoins en mémoire peuvent être importants. La méthode EM évolutif fonctionnant sur une mémoire tampon locale, le parcours des données s'effectue beaucoup plus rapidement, et l'algorithme utilise beaucoup mieux le cache mémoire du processeur que la méthode EM non évolutif. De plus, la méthode EM évolutif est trois fois plus rapide que la méthode EM non évolutif, même si toutes les données peuvent tenir dans la mémoire principale. Dans la majorité de cas, l'amélioration des performances n'entraîne pas de baisse de la qualité du modèle complet.  
  
 Pour obtenir un rapport technique qui décrit l’implémentation de la méthode EM dans l’algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] , consultez [Scaling EM (Expectation Maximization) Clustering to Large Databases](http://go.microsoft.com/fwlink/?LinkId=45964).  
  
### <a name="k-means-clustering"></a>Clustering K-means  
 Le clustering K-means est une méthode bien connue d'attribution de l'appartenance au cluster qui repose sur la minimisation des différences entre les éléments d'un cluster et la maximisation de la distance entre les clusters. Le terme « means » dans K-means fait référence au *centroïde* du cluster, c’est-à-dire un point de données choisi arbitrairement puis affiné de manière itérative jusqu’à ce qu’il représente la moyenne vraie de tous les points de données dans le cluster. La lettre « k » fait référence au nombre arbitraire de points qui sont utilisés pour ensemencer le processus de clustering. L'algorithme K-means calcule les distances euclidiennes au carré entre les enregistrements de données dans un cluster et le vecteur qui représente la moyenne du cluster, puis converge vers un jeu final de k clusters lorsque cette somme atteint sa valeur minimale.  
  
 L'algorithme K-means attribue chaque point de données à un seul cluster et n'accepte pas l'incertitude de l'appartenance. L'appartenance dans un cluster est exprimée sous forme d'une distance par rapport au centroïde.  
  
 L'algorithme K-means est généralement utilisé pour créer des clusters d'attributs continus du fait de la simplicité du calcul de la distance à une moyenne. Toutefois, l'implémentation [!INCLUDE[msCoName](../../includes/msconame-md.md)] adapte la méthode K-means aux attributs discrets de cluster au moyen de probabilités.  Pour les attributs discrets, la distance d'un cluster particulier à un point de données est calculée comme suit :  
  
 1 - P(point de données, cluster)  
  
> [!NOTE]  
>  L'algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] n'expose pas la fonction de distance utilisée pour calculer K-means, et les mesures de distance ne sont pas disponibles dans le modèle terminé. Toutefois, vous pouvez utiliser une fonction de prédiction pour retourner une valeur qui correspond à la distance, celle-ci étant calculée comme la probabilité d'appartenance d'un point de données au cluster. Pour plus d’informations, consultez [ClusterProbability &#40;DMX&#41;](../../dmx/clusterprobability-dmx.md).  
  
 L'algorithme K-means offre deux méthodes d'échantillonnage du jeu de données : K-means non évolutif et K-means évolutif. La première charge le jeu de données entier et fait un passage de clustering. Dans la seconde, l'algorithme utilise les 50 000 premiers cas et ne lit davantage de cas que s'il a besoin de données supplémentaires pour trouver un modèle adapté aux données.  
  
### <a name="updates-to-the-microsoft-clustering-algorithm-in-sql-server-2008"></a>Mises à jour apportées à l'algorithme de gestion de clusters Microsoft dans SQL Server 2008  
 Dans SQL Server 2008, la configuration par défaut de l’algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] a été modifiée de façon à utiliser le paramètre interne, NORMALIZATION = 1. La normalisation est effectuée à l'aide de statistiques z-score et suppose une distribution normale. La finalité de cette modification du comportement par défaut est de réduire l'effet d'attributs susceptibles de présenter des grandeurs importantes et de nombreuses valeurs hors norme. Toutefois, la normalisation z-score peut modifier les résultats du clustering sur les distributions qui ne sont pas normales (loi uniforme, par exemple). Pour empêcher la normalisation et obtenir le même comportement que l’algorithme de gestion de clusters K-means de SQL Server 2005, vous pouvez utiliser la boîte de dialogue **Paramètres** pour ajouter le paramètre personnalisé, NORMALIZATION, et lui affecter la valeur 0.  
  
> [!NOTE]  
>  Le paramètre NORMALIZATION est une propriété interne de l’algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] et n’est pas pris en charge. En général, l'utilisation de la normalisation est recommandée dans les modèles de gestion de clusters afin d'améliorer les résultats des modèles.  
  
## <a name="customizing-the-microsoft-clustering-algorithm"></a>Personnalisation de l'algorithme de gestion de clusters Microsoft  
 L’algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] prend en charge plusieurs paramètres qui affectent le comportement, les performances et la précision du modèle d’exploration de données résultant.  
  
### <a name="setting-algorithm-parameters"></a>Définition des paramètres de l'algorithme  
 Le tableau suivant décrit les paramètres qui peuvent être utilisés avec l'algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Ces paramètres affectent les performances et la précision du modèle d'exploration de données résultant.  
  
 CLUSTERING_METHOD  
 Spécifie la méthode de clustering que doit utiliser l'algorithme. Les méthodes de clustering disponibles sont les suivantes :  
  
|ID|Méthode|  
|--------|------------|  
|1|EM évolutif|  
|2|EM non évolutif|  
|3|K-means évolutif|  
|4|K-means non évolutif.|  
  
 La valeur par défaut est 1 (EM évolutif).  
  
 CLUSTER_COUNT  
 Spécifie le nombre approximatif de clusters que l'algorithme doit générer. S'il est impossible de générer ce nombre approximatif de clusters à partir des données, l'algorithme génère autant de clusters que possible. Si le paramètre CLUSTER_COUNT est défini à 0, l'algorithme utilise des valeurs heuristiques pour déterminer de manière optimale le nombre de clusters à générer.  
  
 La valeur par défaut est 10.  
  
 CLUSTER_SEED  
 Spécifie la valeur de départ utilisée pour générer de façon aléatoire des clusters pour le stade initial de construction d'un modèle.  
  
 En modifiant ce nombre, vous pouvez modifier la façon dont les clusters initiaux sont créés, puis comparer des modèles qui ont été générés avec des valeurs de départ différentes. Si la valeur de départ est modifiée mais que les clusters trouvés ne changent guère, le modèle peut être considéré comme relativement stable.  
  
 La valeur par défaut est 0.  
  
 MINIMUM_SUPPORT  
 Spécifie le nombre minimal de cas requis pour générer un cluster. Si le nombre de cas dans le cluster est inférieur à ce nombre, le cluster est considéré comme vide et est ignoré.  
  
 Si vous définissez un nombre trop grand, vous pouvez laisser passer des clusters valides.  
  
> [!NOTE]  
>  Si vous utilisez la méthode EM, qui est la méthode de clustering par défaut, certains clusters peuvent avoir une valeur de prise en charge qui est inférieure à la valeur spécifiée. Cela est dû au fait que l'appartenance de chaque cas est évaluée dans tous les clusters possibles, et que pour certains clusters, la prise en charge peut être minimale uniquement.  
  
 La valeur par défaut est 1.  
  
 MODELLING_CARDINALITY  
 Spécifie le nombre d'exemples de modèles générés pendant le processus de clustering.  
  
 Le fait de réduire le nombre de modèles candidats peut améliorer les performances au risque de laisser passer quelques bons modèles candidats.  
  
 La valeur par défaut est 10.  
  
 STOPPING_TOLERANCE  
 Spécifie la valeur utilisée pour déterminer quand une convergence est atteinte et quand l'algorithme a terminé la construction du modèle. La convergence est atteinte lorsque le changement global dans les probabilités de clusters est inférieur au rapport du paramètre STOPPING_TOLERANCE divisé par la taille du modèle.  
  
 La valeur par défaut est 10.  
  
 SAMPLE_SIZE  
 Spécifie le nombre de cas que l'algorithme utilise à chaque passage si l'une des méthodes de clustering évolutif est définie pour le paramètre CLUSTERING_METHOD. Si la valeur 0 est attribuée au paramètre SAMPLE_SIZE, le jeu de données complet est organisé en clusters en un seul passage. Le chargement du dataset entier en un seul passage peut provoquer des problèmes de mémoire et de performances.  
  
 La valeur par défaut est 50000.  
  
 MAXIMUM_INPUT_ATTRIBUTES  
 Spécifie le nombre maximal d'attributs d'entrée que l'algorithme peut traiter avant d'appeler la sélection des fonctionnalités. La valeur 0 spécifie qu'il n'y a pas de nombre maximal d'attributs.  
  
 Le fait d'augmenter le nombre d'attributs peut nuire considérablement aux performances.  
  
 La valeur par défaut est 255.  
  
 MAXIMUM_STATES  
 Spécifie le nombre maximal d'états d'attribut que l'algorithme prend en charge. Si le nombre d'états d'un attribut est supérieur au nombre maximal d'états, l'algorithme utilise les états les plus fréquents et ignore le reste des états.  
  
 Le fait d'augmenter le nombre d'états peut nuire considérablement aux performances.  
  
 La valeur par défaut est 100.  
  
### <a name="modeling-flags"></a>Indicateurs de modélisation  
 L'algorithme prend en charge les indicateurs de modélisation suivants. Vous définissez des indicateurs de modélisation lorsque vous créez la structure d'exploration de données ou le modèle d'exploration de données. Les indicateurs de modélisation spécifient le mode de traitement des valeurs dans chaque colonne pendant l'analyse.  
  
|Indicateur de modélisation|Description|  
|-------------------|-----------------|  
|MODEL_EXISTENCE_ONLY|La colonne sera considérée comme ayant deux états possibles : manquante et existante. Une valeur NULL est une valeur manquante.<br /><br /> S'applique à la colonne de modèle d'exploration de données.|  
|NOT NULL|La colonne ne peut pas contenir de valeur NULL. Une erreur est générée si Analysis Services rencontre une valeur NULL au cours de l'apprentissage du modèle.<br /><br /> S'applique à la colonne de structure d'exploration de données.|  
  
## <a name="requirements"></a>Spécifications  
 Un modèle de clustering doit contenir une colonne clé et des colonnes d'entrée. Vous pouvez également définir les colonnes d'entrée comme prédictibles. Les colonnes ayant la valeur **Prédire uniquement** ne sont pas utilisées pour générer des clusters. La distribution de ces valeurs dans les clusters est calculée après la création des clusters.  
  
### <a name="input-and-predictable-columns"></a>Colonnes d'entrée et prédictibles  
 L’algorithme de gestion de clusters [!INCLUDE[msCoName](../../includes/msconame-md.md)] prend en charge les colonnes d’entrée et les colonnes prédictibles répertoriées dans le tableau suivant. Pour plus d’informations sur la signification des types de contenu utilisés dans un modèle d’exploration de données, consultez [Types de contenu &#40;exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md).  
  
|Colonne|Types de contenu|  
|------------|-------------------|  
|Attribut d'entrée|Continu, cyclique, discret, discrétisé, clé, table, trié|  
|Attribut prédictible|Continu, cyclique, discret, discrétisé, table, trié|  
  
> [!NOTE]  
>  Les types de contenu Cyclique et Trié sont pris en charge, mais l'algorithme les traite comme des valeurs discrètes et n'effectue pas de traitement spécial.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme Microsoft Clustering](../../analysis-services/data-mining/microsoft-clustering-algorithm.md)   
 [Exemples de requêtes modèle de clustering](../../analysis-services/data-mining/clustering-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour le Clustering des modèles &#40; Analysis Services - Exploration de données &#41;](../../analysis-services/data-mining/mining-model-content-for-clustering-models-analysis-services-data-mining.md)  
  
  
