---
title: Référence technique de Microsoft Neural Network algorithme | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 76e71ae9c0ceb236c49df8e7fc8ec67713ef3e76
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-neural-network-algorithm-technical-reference"></a>Microsoft Neural Network Algorithm Technical Reference
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L’algorithme MNN ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) utilise un réseau *perceptron multicouche*, également appelé *réseau à règle delta à rétropropagation*, qui peut comporter jusqu’à trois couches de neurones, ou *perceptrons*. une couche d'entrée, une couche masquée facultative et une couche de sortie.  
  
 Les réseaux neuronaux de type perceptron multicouche ne sont pas décrits en détails dans la présente documentation. Cette rubrique explique l'implémentation de base de l'algorithme, dont la méthode utilisée pour normaliser les valeurs d'entrée et de sortie, ainsi que les méthodes de sélection de fonctionnalités utilisées pour réduire la cardinalité de l'attribut. Cette rubrique décrit les paramètres et autres valeurs qui peuvent être utilisés pour personnaliser le comportement de l'algorithme, et fournit des liens vers des informations supplémentaires sur l'interrogation du modèle.  
  
## <a name="implementation-of-the-microsoft-neural-network-algorithm"></a>Implémentation de l'algorithme MNN (Microsoft Neural Network)  
 Dans un réseau neuronal de type perceptron multicouche, chaque neurone reçoit une ou plusieurs entrées et génère une ou plusieurs sorties identiques. Chaque sortie est une fonction non linéaire simple de la somme des entrées dans le neurone. Les entrées sont propagées vers l'avant entre les nœuds de la couche d'entrée et ceux de la couche masquée, puis entre la couche masquée et celle de sortie ; il n'y a pas de connexion entre les neurones au sein d'une même couche. S'il n'y a pas de couche masquée, comme c'est le cas dans un modèle de régression logistique, les entrées sont propagées vers l'avant entre les nœuds de la couche d'entrée et ceux de la couche de sortie.  
  
 Un réseau neuronal créé avec l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) comporte trois types de neurones :  
  
**Neurones d’entrée**  
  
 Les neurones d'entrée fournissent les valeurs des attributs d'entrée du modèle d'exploration de données. Pour les attributs d'entrée discrets, un neurone d'entrée représente généralement un état unique de l'attribut d'entrée. Cela inclut des valeurs manquantes, si les données d'apprentissage contiennent des valeurs Null pour cet attribut. Un attribut d'entrée discret qui a plus de deux états génère un neurone d'entrée pour chaque état et un neurone d'entrée pour un état manquant, si les données d'apprentissage contiennent des valeurs Null. Un attribut d'entrée continu génère deux neurones d'entrée : un pour un état manquant et un pour la valeur de l'attribut continu lui-même. Les neurones d'entrée fournissent des entrées à un ou plusieurs neurones cachés.  
  
**Hidden neurons**  
  
 Les neurones cachés reçoivent des entrées des neurones d'entrée et fournissent des sorties aux neurones de sortie.  
  
**Neurones de sortie**  
  
 Les neurones de sortie représentent les valeurs des attributs prédictibles du modèle d'exploration de données. Pour les attributs d'entrée discrets, un neurone de sortie représente généralement un état prédit unique d'un attribut prédictible, y compris un état manquant. Par exemple, un attribut prédictible binaire produit un nœud de sortie qui décrit un état existant ou manquant pour indiquer si une valeur existe ou non pour cet attribut. Une colonne booléenne utilisée comme attribut prédictible génère trois neurones de sortie : un pour une valeur true, un pour une valeur false et un pour un état manquant ou existant. Un attribut prédictible discret qui a plus de deux états génère un neurone de sortie pour chaque état et un neurone de sortie pour un état manquant ou existant. Les colonnes prédictibles continues génèrent deux neurones de sortie : un pour un état manquant ou existant et un pour la valeur de la colonne continue elle-même. Si plus de 500 neurones de sortie sont générés par l'analyse du jeu de colonnes prédictibles, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère un nouveau réseau dans le modèle d'exploration de données pour représenter les neurones de sortie supplémentaires.  
  
 Un neurone reçoit des entrées provenant d'autres neurones ou données, selon la couche du réseau sur laquelle il se trouve. Un neurone d'entrée reçoit des entrées provenant des données d'origine. Les neurones cachés et de sortie reçoivent des entrées provenant de la sortie d'autres neurones du réseau. Les entrées établissent des relations entre les neurones et ces relations servent de chemin d'analyse pour un ensemble de cas spécifique.  
  
 Chaque entrée est dotée d'une valeur appelée *poids*, qui décrit la pertinence ou l'importance de cette entrée spécifique par rapport au neurone caché ou à celui de sortie. Plus le poids affecté à une entrée est élevé, plus la valeur de celle-ci est pertinente ou importante. Le poids peut être négatif, ce qui implique que l'entrée peut désactiver un neurone spécifique au lieu de l'activer. La valeur de chaque entrée est multipliée par le poids pour accentuer son importance pour un neurone spécifique. Si le poids est négatif, la multiplication de la valeur par celui-ci a pour effet de désaccentuer son importance.  
  
 Chaque neurone est doté d’une fonction non linéaire simple appelée *fonction d’activation*qui décrit la pertinence ou l’importance d’un neurone spécifique par rapport à cette couche d’un réseau neuronal. Les neurones cachés utilisent une fonction *tangente hyperbolique* (tanh) pour leur fonction d’activation, tandis que les neurones de sortie utilisent une fonction *sigmoïde* . Il s'agit de deux fonctions continues non linéaires qui permettent au réseau neuronal de modéliser les relations non linéaires entre les neurones d'entrée et de sortie.  
  
### <a name="training-neural-networks"></a>Apprentissage des réseaux neuronaux  
 L'apprentissage d'un modèle d'exploration de données utilisant l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) comporte plusieurs étapes. Ces étapes sont fortement influencées par les valeurs que vous spécifiez pour les paramètres d'algorithme.  
  
 L'algorithme commence par évaluer et par extraire les données d'apprentissage de la source de données. Un pourcentage des données d'apprentissage, appelé *données d'exclusion*, est réservé à l'évaluation de la précision du réseau. Pendant le processus d'apprentissage, le réseau est évalué immédiatement après chaque itération des données d'apprentissage. Lorsque la précision n'augmente plus, le processus d'apprentissage est interrompu.  
  
 Les valeurs des paramètres *SAMPLE_SIZE* et *HOLDOUT_PERCENTAGE* permettent de déterminer le nombre de cas à échantillonner dans les données d’apprentissage et le nombre de cas à mettre de côté pour les données d’exclusion. La valeur du paramètre *HOLDOUT_SEED* permet de déterminer de façon aléatoire les cas individuels à mettre de côté pour les données d’exclusion.  
  
> [!NOTE]  
>  Ces paramètres d'algorithme sont différents des propriétés HOLDOUT_SIZE et HOLDOUT_SEED qui sont appliquées à une structure d'exploration de données pour définir in jeu de données de test.  
  
 L'algorithme détermine ensuite le nombre et la complexité des réseaux pris en charge par le modèle d'exploration de données. Si le modèle d'exploration de données contient un ou plusieurs attributs qui sont utilisés uniquement pour la prédiction, l'algorithme crée un réseau unique qui représente tous ces attributs. Si le modèle d'exploration de données contient un ou plusieurs attributs qui sont utilisés à la fois pour l'entrée et pour la prédiction, le fournisseur de l'algorithme construit un réseau pour chaque attribut.  
  
 Pour les attributs d'entrée ou les attributs prédictibles dotés de valeurs discrètes, chaque neurone d'entrée ou de sortie représente respectivement un état unique. Pour les attributs d'entrée ou les attributs prédictibles dotés de valeurs continues, chaque neurone d'entrée ou de sortie représente respectivement la plage et la distribution de valeurs pour l'attribut. Le nombre maximal d’états pris en charge dans les deux cas dépend de la valeur du paramètre *MAXIMUM_STATES* de l’algorithme. Si le nombre d’états d’un attribut spécifique dépasse la valeur du paramètre d’algorithme *MAXIMUM_STATES* , les états les plus fréquents ou les plus pertinents pour cet attribut sont sélectionnés, à hauteur du nombre maximal d’états autorisé, et les états restants sont considérés comme des valeurs manquantes dans le cadre de l’analyse.  
  
 L’algorithme utilise ensuite la valeur du paramètre *HIDDEN_NODE_RATIO* pour déterminer le nombre initial de neurones à créer pour la couche cachée. Vous pouvez attribuer la valeur 0 à *HIDDEN_NODE_RATIO* pour empêcher la création d’une couche cachée dans les réseaux générés par l’algorithme pour le modèle d’exploration de données. Le réseau neuronal est alors traité en tant que régression logistique.  
  
 Le fournisseur d'algorithme évalue de façon itérative le poids de toutes les entrées se trouvant sur le réseau au même moment, en prenant l'ensemble de données d'apprentissage qui a été auparavant mis de côté et en comparant la valeur réelle connue de chacun des cas figurant dans les données d'exclusion à la prédiction du réseau : ce processus s'appelle l' *apprentissage par lots*. Une fois que l'algorithme a évalué l'intégralité des données d'apprentissage, l'algorithme passe en revue la valeur prédite et la valeur réelle de chaque neurone. L'algorithme calcule, le cas échéant, le taux d'erreur et ajuste les poids attribués aux entrées de ce neurone en propageant les informations vers l'arrière, c'est-à-dire en partant des neurones de sortie et en remontant aux neurones d'entrée : ce processus s'appelle la *rétropropagation*. L'algorithme répète ensuite le processus sur l'intégralité des données d'apprentissage. Étant donné que l'algorithme peut prendre en charge de nombreux poids et neurones de sortie, l'algorithme du gradient conjugué est utilisé afin de guider le processus d'apprentissage pour l'attribution et l'évaluation des poids des entrées. L'algorithme du gradient conjugué n'est pas traité dans la présente documentation.  
  
### <a name="feature-selection"></a>Sélection des fonctionnalités  
 Si le nombre d’attributs d’entrée ou d’attributs prédictibles est supérieur respectivement à la valeur du paramètre *MAXIMUM_INPUT_ATTRIBUTES* ou du paramètre *MAXIMUM_OUTPUT_ATTRIBUTES* , un algorithme de sélection des fonctionnalités est utilisé pour réduire la complexité des réseaux inclus dans le modèle d’exploration de données. La sélection des fonctionnalités réduit le nombre d'attributs d'entrée ou prévisibles à ceux qui sont statistiquement les plus pertinents pour le modèle.  
  
 La sélection des fonctionnalités est automatiquement utilisée par tous les algorithmes d'exploration de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour améliorer l'analyse et réduire la charge de traitement. La méthode utilisée pour la sélection des fonctionnalités dans les modèles de réseau neuronal dépend du type de données de l'attribut. Pour référence, le tableau suivant présente les méthodes de sélection de fonctionnalités utilisées pour les modèles de réseau neuronal et pour l'algorithme Logistic Regression qui est basé sur l'algorithme Neural Network.  
  
|Algorithm|Méthode d'analyse|Commentaires|  
|---------------|------------------------|--------------|  
|Neural Network|Score d'intérêt et de pertinence<br /><br /> Entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|L'algorithme MNN (Microsoft Neural Network, réseau neuronal de Microsoft) peut utiliser les deux méthodes de calcul de score (entropie et bayésien), tant que les données contiennent des colonnes continues.<br /><br /> Valeur par défaut.|  
|Logistic Regression|Score d'intérêt et de pertinence<br /><br /> Entropie de Shannon<br /><br /> Bayésien avec a priori K2<br /><br /> Équivalent bayésien de Dirichlet avec a priori uniforme (par défaut)|Étant donné que vous ne pouvez pas passer de paramètre à cet algorithme pour contrôler le comportement de la sélection des fonctionnalités, les valeurs par défaut sont utilisées. Par conséquent, si tous les attributs sont discrets ou discrétisés, la valeur par défaut est BDEU.|  
  
 Les paramètres d'algorithme qui contrôlent la sélection des fonctionnalités d'un modèle de réseau neuronal sont MAXIMUM_INPUT_ATTRIBUTES, MAXIMUM_OUTPUT_ATTRIBUTES et MAXIMUM_STATES. Vous pouvez également contrôler le nombre de couches masquées en définissant le paramètre HIDDEN_NODE_RATIO.  
  
### <a name="scoring-methods"></a>Méthodes de calcul de score  
 Le*score* est une sorte de normalisation qui, dans le contexte de l'apprentissage d'un modèle de réseau neuronal, désigne le processus de conversion d'une valeur, telle qu'une étiquette de texte discrète, en une valeur qui peut être comparée avec d'autres types d'entrées et être pondérée dans le réseau. Par exemple, si un attribut d'entrée est Gender et que les valeurs possibles sont Male et Female, et qu'un autre attribut d'entrée est Income, avec une plage de valeurs variable, les valeurs pour chaque attribut ne sont pas directement comparables, et doivent donc être encodées à une échelle commune afin que les poids puissent être calculés. Le score est le processus qui consiste à normaliser ces entrées en valeurs numériques, plus précisément en une plage de probabilités. Les fonctionnalités utilisées pour la normalisation permettent également de répartir la valeur d'entrée plus équitablement sur une échelle uniforme afin que les valeurs extrêmes ne déforment pas les résultats d'analyse.  
  
 Les sorties du réseau neuronal sont également encodées. Lorsqu'il y a une cible unique pour la sortie (c'est-à-dire, la prédiction), ou plusieurs cibles utilisées uniquement pour la prédiction et non pas pour l'entrée, le modèle crée un réseau unique et il peut ne pas s'avérer nécessaire de normaliser les valeurs. Toutefois, si plusieurs attributs sont utilisés pour l'entrée et la prédiction, le modèle doit créer plusieurs réseaux ; par conséquent, toutes les valeurs doivent être normalisées, et les sorties doivent également être encodées lorsqu'elles quittent le réseau.  
  
 L'encodage des entrées consiste à additionner chaque valeur discrète des cas d'apprentissage, et à la multiplier par son poids. On parle alors de *somme pondérée*, laquelle est transmise à la fonction d'activation dans la couche masquée. Un z-score est utilisé pour l'encodage :  
  
 **Valeurs discrètes**  
  
 `μ = p` – probabilité antérieure d’un état  
  
 `StdDev  = sqrt(p(1-p))`  
  
 **Valeurs continues**  
  
 Valeur présente = `1 - μ/σ`  
  
 Aucune valeur existante = `-μ/σ`  
  
 Une fois les valeurs encodées, les entrées font l'objet d'une addition pondérée, avec les contours du réseau comme poids.  
  
 L'encodage des sorties utilise la fonction sigmoïde, qui est dotée de propriétés qui la rendent très utile pour la prédiction. L'une d'entre elles est que, indépendamment de la méthode utilisée pour mettre les valeurs d'origine à l'échelle et du fait que les valeurs sont négatives ou positives, la sortie de cette fonction est toujours une valeur comprise entre 0 et 1, ce qui convient parfaitement pour l'estimation des probabilités. Une autre propriété utile est que la fonction sigmoïde a un effet de lissage, de sorte que lorsque les valeurs s'éloignent du point d'inflexion, la probabilité pour la valeur se rapproche de 0 ou 1, mais lentement.  
  
## <a name="customizing-the-neural-network-algorithm"></a>Personnalisation de l'algorithme Neural Network  
 L'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) prend en charge plusieurs paramètres qui affectent le comportement, les performances et la précision du modèle d'exploration de données résultant. Vous pouvez également modifier la manière dont le modèle traite les données en définissant des indicateurs de modélisation sur les colonnes ou des indicateurs de distribution afin de spécifier le mode de traitement des valeurs dans la colonne.  
  
### <a name="setting-algorithm-parameters"></a>Définition des paramètres de l'algorithme  
 Le tableau suivant décrit les paramètres qui peuvent être utilisés avec l'algorithme MNN (Microsoft Neural Network).  
  
 HIDDEN_NODE_RATIO  
 Spécifie le taux de neurones cachés par rapport aux neurones d'entrée et de sortie. La formule suivante détermine le nombre initial de neurones dans la couche masquée :  
  
 HIDDEN_NODE_RATIO * SQRT (total neurones d’entrée \* total neurones de sortie)  
  
 La valeur par défaut est 4,0.  
  
 HOLDOUT_PERCENTAGE  
 Spécifie le pourcentage de cas extraits des données d'apprentissage pour calculer l'erreur d'exclusion, qui constitue l'un des critères d'arrêt pendant l'apprentissage du modèle d'exploration de données.  
  
 La valeur par défaut est 30.  
  
 HOLDOUT_SEED  
 Spécifie un nombre qui est utilisé en tant que valeur de départ du générateur de nombres pseudo-aléatoires lorsque l'algorithme détermine de façon aléatoire les données d'exclusion. Si ce paramètre a la valeur 0, l'algorithme génère la valeur de départ en fonction du nom du modèle d'exploration de données, afin de garantir que le contenu du modèle reste inchangé pendant le retraitement.  
  
 La valeur par défaut est 0 :  
  
 MAXIMUM_INPUT_ATTRIBUTES  
 Détermine le nombre maximal d'attributs d'entrée qui peuvent être fournis à l'algorithme et au-delà duquel la sélection des fonctionnalités est utilisée. La valeur 0 désactive la sélection des fonctionnalités pour les attributs d'entrée.  
  
 La valeur par défaut est 255.  
  
 MAXIMUM_OUTPUT_ATTRIBUTES  
 Détermine le nombre maximal d'attributs de sortie qui peuvent être fournis à l'algorithme et au-delà duquel la sélection des fonctionnalités est utilisée. La valeur 0 désactive la sélection des fonctionnalités pour les attributs de sortie.  
  
 La valeur par défaut est 255.  
  
 MAXIMUM_STATES  
 Spécifie le nombre maximal d'états discrets par attribut qui est pris en charge par l'algorithme. Si le nombre d'états d'un attribut spécifique est supérieur au nombre spécifié pour ce paramètre, l'algorithme sélectionne les états les plus fréquents pour cet attribut et traite les autres comme étant absents.  
  
 La valeur par défaut est 100.  
  
 SAMPLE_SIZE  
 Spécifie le nombre de cas à utiliser pour effectuer l'apprentissage du modèle. L'algorithme utilise soit ce nombre, soit le pourcentage du nombre total de cas non inclus dans les données d'exclusion conformément au paramètre HOLDOUT_PERCENTAGE, la plus petite valeur étant retenue.  
  
 En d'autres termes, si HOLDOUT_PERCENTAGE a la valeur 30, l'algorithme utilisera soit la valeur de ce paramètre, soit une valeur égale à 70 % du nombre total de cas, la plus petite valeur étant retenue.  
  
 La valeur par défaut est 10 000.  
  
### <a name="modeling-flags"></a>Indicateurs de modélisation  
 Les indicateurs de modélisation suivants sont pris en charge pour une utilisation avec l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network).  
  
 NOT NULL  
 Indique que la colonne ne peut pas contenir de valeur Null. Une erreur est générée si Analysis Services rencontre une valeur NULL au cours de l'apprentissage du modèle.  
  
 S'applique aux colonnes de structure d'exploration de données.  
  
 MODEL_EXISTENCE_ONLY  
 Indique que le modèle doit uniquement déterminer si une valeur existe ou non pour l'attribut ou si une valeur manque. La valeur exacte n'a pas d'importance.  
  
 S'applique aux colonnes de modèle d'exploration de données.  
  
### <a name="distribution-flags"></a>Indicateurs de distribution  
 Les indicateurs de distribution suivants sont pris en charge pour une utilisation avec l'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network). Les indicateurs sont uniquement utilisés comme indications pour le modèle ; si l'algorithme détecte une autre distribution, il utilisera celle-ci et non pas celle spécifiée dans l'indication.  
  
 Normale  
 Indique que les valeurs de la colonne doivent être considérées comme étant la normale, ou gaussiennes.  
  
 Uniforme  
 Indique que les valeurs de la colonne doivent être considérées comme étant distribuées uniformément ; autrement dit, la probabilité de toute valeur est à peu près constante et est une fonction du nombre total de valeurs.  
  
 Log-normale  
 Indique que les valeurs de la colonne doivent être considérées comme étant distribuées selon la courbe *log-normale* , ce qui signifie que le logarithme des valeurs est distribué normalement.  
  
## <a name="requirements"></a>Spécifications  
 Un modèle de réseau neuronal doit contenir au moins une colonne d'entrée et une colonne de sortie.  
  
### <a name="input-and-predictable-columns"></a>Colonnes d'entrée et prédictibles  
 L'algorithme MNN ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Neural Network) prend en charge les colonnes d'entrée spécifiques et les colonnes prédictibles répertoriées dans le tableau suivant.  
  
|Colonne|Types de contenu|  
|------------|-------------------|  
|Attribut d'entrée|Continu, Cyclique, Discret, Discrétisé, Clé, Table et Trié|  
|Attribut prédictible|Continu, Cyclique, Discret, Discrétisé et Trié|  
  
> [!NOTE]  
>  Les types de contenu Cyclique et Trié sont pris en charge, mais l'algorithme les traite comme des valeurs discrètes et n'effectue pas de traitement spécial.  
  
## <a name="see-also"></a>Voir aussi  
 [MNN (Microsoft Neural Network)](../../analysis-services/data-mining/microsoft-neural-network-algorithm.md)   
 [Contenu du modèle d’exploration de données pour les modèles de réseau neuronal & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-neural-network-models-analysis-services-data-mining.md)   
 [Exemples de requête de modèle de réseau neuronal](../../analysis-services/data-mining/neural-network-model-query-examples.md)  
  
  
