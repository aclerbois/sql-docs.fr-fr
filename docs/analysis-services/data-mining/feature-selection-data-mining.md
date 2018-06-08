---
title: Fonctionnalité de sélection (exploration de données) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a93e503978779e56250ddf190c61b1b2411050b9
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="feature-selection-data-mining"></a>Sélection des fonctionnalités (exploration de données)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La *sélection des fonctionnalités* est une partie importante de l’apprentissage automatique. Elle désigne le processus visant à réduire les entrées à traiter et à analyser, ou à identifier les entrées les plus pertinentes. Le terme connexe *ingénierie des fonctionnalités* (ou *extraction des fonctionnalités*) désigne le processus visant à extraire des informations ou des fonctionnalités utiles à partir de données existantes.  
  
## <a name="why-do-feature-selection"></a>Pourquoi utiliser la sélection des fonctionnalités ?  
 La sélection des fonctionnalités est essentielle à la création d’un modèle performant pour plusieurs raisons. La première raison est que la sélection des fonctionnalités implique une certaine *réduction de cardinalité*pour imposer une limite du nombre d’attributs pouvant être pris en compte lors de la création d’un modèle. Les données contiennent presque toujours plus d’informations que nécessaire pour générer le modèle ou elles contiennent un type d’informations inapproprié. Par exemple, vous pouvez avoir un dataset de 500 colonnes qui décrivent les caractéristiques des clients. Toutefois, si certaines colonnes contiennent des données éparses, cela n’est pas très utile de les ajouter au modèle et, si certaines colonnes sont en double, leur utilisation peut rendre le modèle inexact.  
  
 La sélection des fonctionnalités améliore la qualité du modèle, tout en optimisant le processus de modélisation. Si vous incluez des colonnes inutiles pour générer un modèle, davantage d’UC et de mémoire sont consommées pendant le processus d’apprentissage, et il faut plus d’espace de stockage pour le modèle terminé. Indépendamment du problème des ressources, vous avez intérêt à effectuer la sélection des fonctionnalités et à identifier les colonnes les plus pertinentes, car l’utilisation de colonnes inutiles peut diminuer la qualité du modèle de plusieurs façons :  
  
-   Des données parasites ou redondantes rendent plus difficile la découverte de séquences significatives.  
  
-   En présence d’un jeu de données multidimensionnel, la plupart des algorithmes d’exploration des données nécessitent un jeu de données d’apprentissage beaucoup plus important.  
  
 Pendant le processus de sélection des fonctionnalités, l’analyste ou l’outil de modélisation ou l’algorithme sélectionne ou ignore de façon active les attributs en fonction de leur utilité pour l’analyse.  L’analyste peut effectuer l’ingénierie des fonctionnalités pour ajouter des fonctionnalités et supprimer ou modifier des données existantes, tandis que l’algorithme d’apprentissage automatique calcule généralement les scores des colonnes pour valider leur utilité dans le modèle.  
  
 ![Fonctionnalité de sélection et des processus d’ingénierie](../../analysis-services/data-mining/media/ssdm-featureselectionprocess.png "sélection et des processus d’ingénierie de fonctionnalité")  
  
 Pour résumer, la sélection des fonctionnalités permet à la fois d’éviter d’avoir trop de données qui présentent peu d’intérêt ou de n’avoir pas assez de données utiles. Votre objectif, en utilisant la sélection des fonctionnalités, est d’identifier le nombre minimum de colonnes de la source de données qui sont importantes pour le modèle à créer.  
  
## <a name="how-feature-selection-works-in-sql-server-data-mining"></a>Fonctionnement de la sélection des fonctionnalités dans SQL Server Data Mining  
 La sélection des fonctionnalités est toujours effectuée avant l’apprentissage du modèle. Avec certains algorithmes, les techniques de sélection des fonctionnalités sont « intégrées » pour exclure les colonnes non pertinentes et détecter automatiquement les meilleures fonctionnalités. Chaque algorithme a son propre ensemble de techniques par défaut pour appliquer intelligemment la réduction des fonctionnalités.  Toutefois, vous pouvez également définir manuellement des paramètres pour influencer le comportement de la sélection des fonctionnalités.  
  
 Pendant la sélection automatique des fonctionnalités, un score est calculé pour chaque attribut, et seuls les attributs qui ont les meilleurs scores sont sélectionnés pour le modèle. Vous pouvez également ajuster le seuil pour les scores les plus élevés. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Mining fournit plusieurs méthodes pour calculer les scores ; la méthode exacte appliquée dans un modèle dépend de ces facteurs :  
  
-   Algorithme utilisé dans votre modèle  
  
-   Type de données de l'attribut  
  
-   Tous les paramètres que vous pouvez avoir définis sur votre modèle  
  
 La sélection des fonctionnalités est appliquée aux entrées, aux attributs prévisibles ou aux états d'une colonne. Lorsque le calcul du score de sélection des fonctionnalités est terminé, seuls les attributs et les états que l'algorithme sélectionne sont inclus dans le processus de génération de modèle et peuvent être utilisés pour des prédictions. Si vous choisissez un attribut prédictible qui ne correspond pas au seuil pour la sélection des fonctionnalités, l'attribut peut toujours être utilisé pour la prédiction, mais les prédictions seront uniquement basées sur les statistiques globales qui existent dans le modèle.  
  
> [!NOTE]  
>  La sélection des fonctionnalités affecte uniquement les colonnes utilisées dans le modèle et n'a aucun effet sur le stockage de la structure d'exploration de données. Les colonnes que vous omettez dans le modèle d'exploration de données restent disponibles dans la structure, et les données contenues dans les colonnes de structure d'exploration de données sont mises en cache.  
  
### <a name="feature-selection-scores"></a>Scores de sélection des fonctionnalités  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Mining prend en charge ces méthodes courantes et bien établies pour les attributs de score. La méthode qui est utilisée dans un algorithme ou un jeu de données dépend des types de données et de l’utilisation des colonnes.  
  
-   Le score *d’intérêt et de pertinence* est utilisé pour classer par ordre de priorité et trier les attributs dans des colonnes qui contiennent des données numériques continues non binaires.  
  
-   *L’entropie de Shannon* et deux scores *bayésiens* sont disponibles pour les colonnes qui contiennent des données discrètes et discrétisées. Toutefois, si le modèle contient des colonnes continues, le score d'intérêt et de pertinence est utilisé pour évaluer toutes les colonnes d'entrée, afin de garantir la cohérence.  
  
#### <a name="interestingness-score"></a>Score d'intérêt et de pertinence  
 Une fonctionnalité présente un intérêt si elle fournit des informations utiles. Toutefois, le score *d’intérêt et de pertinence* peut être calculé de plusieurs façons.  La*nouveauté* peut être utile dans la détection de valeurs hors norme, mais la capacité de faire la distinction entre des éléments étroitement liés connexes, ou *poids discriminant*, peut être plus intéressante pour la classification.  
  
 La mesure d’intérêt et de pertinence utilisée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Mining est *basée sur l’entropie*: les attributs avec des distributions aléatoires ont une entropie plus élevée, mais ils fournissent moins d’informations et sont donc moins intéressants. L'entropie pour un attribut particulier est comparée à l'entropie de tous les autres attributs, comme suit :  
  
 Intérêt/Pertinence(Attribut) = - (m - Entropie(Attribut)) * (m - Entropie(Attribut))  
  
 L’entropie centrale, ou m, représente l’entropie de l’ensemble du jeu de fonctionnalités. En soustrayant l'entropie de l'attribut cible de l'entropie centrale, vous pouvez évaluer la quantité d'informations fournies par l'attribut.  
  
 Ce score est utilisé par défaut chaque fois que la colonne contient des données numériques continues non binaires.  
  
#### <a name="shannons-entropy"></a>L’entropie de Shannon  
 L'entropie de Shannon mesure l'incertitude d'une variable aléatoire pour un résultat particulier. Par exemple, l'entropie d'une partie de pile ou face peut être représentée comme une fonction de la probabilité de la pièce tombant côté pile.  
  
 Analysis Services utilise la formule suivante pour calculer l'entropie de Shannon :  
  
 H(X) = -∑ P(xi) log(P(xi))  
  
 Cette méthode de calcul de score est disponible pour les attributs discrets et discrétisés.  
  
#### <a name="bayesian-with-k2-prior"></a>Bayésien avec a priori K2  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Data Mining fournit deux scores de sélection des fonctionnalités qui sont basés sur les réseaux bayésiens. Un réseau bayésien est un graphique *dirigé* ou *acyclique* d’états et de transitions entre états. Cela signifie que certains états sont toujours antérieurs à l’état actuel, que certains états sont postérieurs et que le graphique ne se répète pas ou ne fait pas de boucle. Par définition, les réseaux bayésiens autorisent l'utilisation de connaissances antérieures. Toutefois, la question de savoir quels états antérieurs utiliser pour le calcul des probabilités des états ultérieurs est importante pour la conception, les performances et la précision de l'algorithme.  
  
 Développé par Cooper et Herskovits, l'algorithme K2 pour l'apprentissage à partir d'un réseau bayésien est couramment employé dans le cadre de l'exploration de données. Cet algorithme est évolutif et peut analyser plusieurs variables, mais il requiert le tri des variables utilisées comme entrée. Pour plus d’informations, consultez la documentation [Learning Bayesian Networks](http://research.microsoft.com/en-us/um/people/heckerman/hgc94uai.pdf) de Chickering, Geiger et Heckerman.  
  
 Cette méthode de calcul de score est disponible pour les attributs discrets et discrétisés.  
  
#### <a name="bayesian-dirichlet-equivalent-with-uniform-prior"></a>Équivalent bayésien de Dirichlet avec a priori uniforme  
 Le score d'équivalent bayésien de Dirichlet (BDE) utilise également l'analyse bayésienne pour évaluer un réseau pour un dataset donné. La méthode de calcul de score BDE a été développée par Heckerman et est basée sur la métrique BD développée par Cooper et Herskovits. La distribution de Dirichlet est une distribution multinomiale qui décrit la probabilité conditionnelle de chaque variable dans le réseau et qui possède de nombreuses propriétés utiles pour l'apprentissage.  
  
 L'équivalent bayésien de Dirichlet avec a priori uniforme (BDEU) suppose un cas spécial de la distribution de Dirichlet dans laquelle une constante mathématique est utilisée pour créer une distribution fixe ou uniforme d'états antérieurs. Le score BDE suppose aussi l'équivalence de vraisemblance, ce qui signifie que les données ne sont pas censées faire la distinction entre des structures équivalentes. En d’autres termes, si le score pour If A Then B est le même que le score pour If B Then A, les structures ne peuvent pas être différenciées sur la base des données, et la causalité ne peut pas être déduite.  
  
 Pour plus d’informations sur les réseaux bayésiens et l’implémentation de ces méthodes de calcul de scores, consultez la documentation [Learning Bayesian Networks](http://research.microsoft.com/en-us/um/people/heckerman/hgc94uai.pdf).  
  
### <a name="feature-selection-methods-per-algorithm"></a>Méthodes de sélection des fonctionnalités par algorithme  
 Le tableau suivant répertorie les algorithmes qui prennent en charge la sélection des fonctionnalités, les méthodes de sélection des fonctionnalités utilisées par l'algorithme et les paramètres à définir pour contrôler le comportement de la sélection des fonctionnalités :  
  
|Algorithme|Méthode d'analyse|Commentaires|  
|---------------|------------------------|--------------|  
|Naive Bayes|L’entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|L'algorithme Microsoft Naïve Bayes accepte uniquement les attributs discrets ou discrétisés ; par conséquent, il ne peut pas utiliser le score d'intérêt et de pertinence.<br /><br /> Pour plus d’informations sur cet algorithme, consultez [Informations techniques de référence relatives à l’algorithme MNB (Microsoft Naive Bayes)](../../analysis-services/data-mining/microsoft-naive-bayes-algorithm-technical-reference.md).|  
|MDT (Microsoft Decision Trees)|Score d'intérêt et de pertinence<br /><br /> Entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|Si des colonnes contiennent des valeurs continues non binaires, le score d'intérêt et de pertinence est utilisé pour toutes les colonnes afin de garantir la cohérence. Sinon, la méthode de sélection des fonctionnalités par défaut ou la méthode que vous avez spécifiée lors de la création du modèle est utilisée.<br /><br /> Pour plus d’informations sur cet algorithme, consultez [Références techniques relatives à l’algorithme MDT (Microsoft Decision Trees)](../../analysis-services/data-mining/microsoft-decision-trees-algorithm-technical-reference.md).|  
|Réseau neuronal|Score d'intérêt et de pertinence<br /><br /> Entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|L'algorithme MNN (Microsoft Neural Network, réseau neuronal de Microsoft) peut utiliser les deux méthodes de type bayésien et entropie tant que les données contiennent des colonnes continues.<br /><br /> Pour plus d’informations sur cet algorithme, consultez [Informations techniques de référence relatives à l’algorithme MDT (Microsoft Decision Trees)](../../analysis-services/data-mining/microsoft-neural-network-algorithm-technical-reference.md).|  
|MLR (Microsoft Logistic Regression)|Score d'intérêt et de pertinence<br /><br /> Entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|Bien que l'algorithme MLR (Microsoft Logistic Regression) soit basé sur l'algorithme MNN (Microsoft Neural Network), vous ne pouvez pas personnaliser les modèles de régression logistique de façon à contrôler le comportement de la sélection des fonctionnalités ; par conséquent, la valeur par défaut de la sélection des fonctionnalités est toujours la méthode la plus appropriée pour l'attribut.<br /><br /> Si tous les attributs sont discrets ou discrétisés, la valeur par défaut est BDEU.<br /><br /> Pour plus d’informations sur cet algorithme, consultez [Informations techniques de référence relatives à l’algorithme MLR (Microsoft Logistic Regression)](../../analysis-services/data-mining/microsoft-logistic-regression-algorithm-technical-reference.md).|  
|Clustering|Score d'intérêt et de pertinence|L'algorithme de gestion de clusters Microsoft peut utiliser des données discrètes ou discrétisées. Toutefois, le score de chaque attribut étant calculé en tant que distance et étant représenté sous la forme d'un nombre continu, le score d'intérêt et de pertinence doit être utilisé.<br /><br /> Pour plus d’informations sur cet algorithme, consultez [Informations techniques de référence relatives à l’algorithme de gestion de clusters Microsoft](../../analysis-services/data-mining/microsoft-clustering-algorithm-technical-reference.md).|  
|Régression linéaire|Score d'intérêt et de pertinence|L'algorithme MLR (Microsoft Linear Regression) Microsoft peut utiliser uniquement le score d'intérêt et de pertinence, car il prend en charge uniquement les colonnes continues.<br /><br /> Pour plus d’informations sur cet algorithme, consultez [Informations techniques de référence relatives à l’algorithme MLR (Microsoft Linear Regression)](../../analysis-services/data-mining/microsoft-linear-regression-algorithm-technical-reference.md).|  
|MAR (Microsoft Association Rules)<br /><br /> Sequence clustering|Non utilisée|La sélection des fonctionnalités n'est pas appelée avec ces algorithmes.<br /><br /> Toutefois, vous pouvez contrôler le comportement de l'algorithme et, si nécessaire, réduire la taille des données d'entrée en définissant la valeur des paramètres MINIMUM_SUPPORT et MINIMUM_PROBABILITY.<br /><br /> Pour plus d’informations, consultez [Informations techniques de référence relatives à l’algorithme Microsoft Association](../../analysis-services/data-mining/microsoft-association-algorithm-technical-reference.md) et [Informations techniques de référence relatives à l’algorithme MSC (Microsoft Sequence Clustering)](../../analysis-services/data-mining/microsoft-sequence-clustering-algorithm-technical-reference.md).|  
|Série chronologique|Non utilisée|La sélection des fonctionnalités ne s'applique pas aux modèles de série chronologique.<br /><br /> Pour plus d’informations sur cet algorithme, consultez [Informations techniques de référence relatives à l’algorithme MTS (Microsoft Time Series)](../../analysis-services/data-mining/microsoft-time-series-algorithm-technical-reference.md).|  
  
## <a name="feature-selection-parameters"></a>Paramètres de sélection des fonctionnalités  
 Dans les algorithmes qui prennent en charge la sélection des fonctionnalités, vous pouvez contrôler à quel moment la sélection des fonctionnalités est activée au moyen des paramètres suivants. Chaque algorithme a une valeur par défaut pour le nombre des entrées autorisées, mais vous pouvez remplacer cette valeur par défaut et spécifier le nombre d'attributs. Cette section répertorie les paramètres qui sont fournis pour gérer la sélection des fonctionnalités.  
  
#### <a name="maximuminputattributes"></a>MAXIMUM_INPUT_ATTRIBUTES  
 Si un modèle contient plus de colonnes que le nombre spécifié par le paramètre *MAXIMUM_INPUT_ATTRIBUTES* , l’algorithme ignore toutes les colonnes qu’il évalue comme inintéressantes.  
  
#### <a name="maximumoutputattributes"></a>MAXIMUM_OUTPUT_ATTRIBUTES  
 De manière similaire, si un modèle contient plus de colonnes prédictibles que le nombre spécifié par le paramètre *MAXIMUM_OUTPUT_ATTRIBUTES* , l’algorithme ignore toutes les colonnes qu’il évalue comme inintéressantes.  
  
#### <a name="maximumstates"></a>MAXIMUM_STATES  
 Si un modèle contient plus de cas que le nombre spécifié par le paramètre *MAXIMUM_STATES* , les états les moins utilisés sont regroupés et traités comme étant manquants. Si l'un de ces paramètres a la valeur 0, la sélection des fonctionnalités est désactivée, ce qui affecte le temps de traitement et les performances.  
  
 En plus de ces méthodes de sélection des fonctionnalités, vous pouvez améliorer la capacité de l’algorithme à identifier ou promouvoir des attributs explicites en définissant des *indicateurs de modélisation* sur le modèle ou en définissant des *indicateurs de distribution* sur la structure. Pour plus d’informations sur ces concepts, consultez [Indicateurs de modélisation &#40;exploration de données&#41;](../../analysis-services/data-mining/modeling-flags-data-mining.md) et [Distributions de colonnes &#40;exploration de données&#41;](../../analysis-services/data-mining/column-distributions-data-mining.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Personnaliser les modèles et les structures d'exploration de données](../../analysis-services/data-mining/customize-mining-models-and-structure.md)  
  
  
