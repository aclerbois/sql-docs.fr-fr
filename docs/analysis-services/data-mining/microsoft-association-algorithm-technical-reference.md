---
title: Référence technique d’algorithme Microsoft Association | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d9cba8f282e1f355b7b4265298890eccb0a4d613
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="microsoft-association-algorithm-technical-reference"></a>Références techniques relatives à l'algorithme Microsoft Association
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  L'algorithme MAR ([!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) est une implémentation simple de l'algorithme Apriori bien connu.  
  
 Vous pouvez utiliser les algorithmes MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) et MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) pour analyser des associations, mais les règles identifiées par chaque algorithme peuvent différer. Dans un modèle d'arbres de décision, les divisions qui aboutissent à des règles spécifiques sont basées sur un gain d'informations, tandis que dans un modèle d'association, les règles sont entièrement basées sur la confiance. Par conséquent, dans un modèle d'association, une règle forte ou avec un niveau de confiance élevé n'est pas forcément intéressante car elle ne fournit pas de nouvelles informations.  
  
## <a name="implementation-of-the-microsoft-association-algorithm"></a>Implémentation de l'algorithme Microsoft Association  
 L'algorithme Apriori n'analyse pas les séquences. Au lieu de cela, il génère puis compte les *jeux d'éléments candidats*. Un élément peut représenter un événement, un produit ou la valeur d'un attribut en fonction du type de données analysées.  
  
 Dans le type de modèle d'association le plus courant, des variables booléennes, représentant une valeur Oui/Non ou Manquant/Existant, sont assignées à chaque attribut, tel que le nom d'un produit ou d'un événement. Une analyse du panier d'achat est un exemple de modèle de règles d'association qui utilise des variables booléennes pour représenter la présence ou l'absence de produits particuliers dans le panier d'achat d'un client.  
  
 Pour chaque jeu d'éléments, l'algorithme crée alors des scores qui représentent la prise en charge et la confiance. Ces scores peuvent être utilisés pour classer par ordre de priorité et dériver des règles intéressantes à partir des jeux d'éléments.  
  
 Des modèles d'association peuvent également être créés pour des attributs numériques. Si les attributs sont continus, les nombres peuvent être *discrétisés* , ou regroupés dans des compartiments. Les valeurs discrétisées peuvent ensuite être traitées comme des valeurs booléennes ou comme des paires attribut-valeur.  
  
### <a name="support-probability-and-importance"></a>Prise en charge, probabilité et importance  
 La*prise en charge*, parfois appelée *fréquence*, représente le nombre de cas contenant l’élément ciblé ou une combinaison d’éléments ciblés. Seuls les éléments ayant au moins le niveau de prise en charge spécifié peuvent être inclus dans le modèle.  
  
 Un *jeu d’éléments fréquent* fait référence à une collection d’éléments dans laquelle la prise en charge de la combinaison d’éléments est aussi supérieure au seuil défini par le paramètre MINIMUM_SUPPORT. Par exemple, si le jeu d'éléments est {A,B,C} et que MINIMUM_SUPPORT a la valeur 10, chaque élément individuel A, B et C doit être présent dans au moins 10 cas pour être inclus dans le modèle et la combinaison d'éléments {A,B,C} doit également être présente dans au moins 10 cas.  
  
 **Remarque** Vous pouvez également contrôler le nombre de jeux d'éléments dans un modèle d'exploration de données en spécifiant la longueur maximale d'un jeu d'éléments, la longueur représentant le nombre d'éléments.  
  
 Par défaut, la prise en charge pour un élément ou un jeu d'éléments particulier représente le nombre de cas contenant cet élément ou ces éléments. Toutefois, vous pouvez également exprimer MINIMUM_SUPPORT sous la forme d'un pourcentage du nombre total de cas dans le jeu de données en tapant une valeur décimale inférieure à 1. Par exemple, si vous spécifiez une valeur MINIMUM_SUPPORT de 0,03, cela signifie qu'au moins 3 % du nombre total de cas dans le jeu de données doivent contenir cet élément ou ce jeu d'éléments pour l'inclusion dans le modèle. Faites des essais avec votre modèle pour déterminer s'il vaut mieux utiliser un nombre ou un pourcentage.  
  
 Par opposition, le seuil pour les règles n'est pas exprimé sous forme de nombre ou de pourcentage, mais sous forme d'une probabilité qui est parfois appelée *confiance*. Par exemple, si le jeu d'éléments {A,B,C} se produit dans 50 cas, mais que le jeu d'éléments {A,B,D} se produit également dans 50 cas et que le jeu d'éléments {A,B} se produit dans 50 autres cas, il est évident que {A,B} n'est pas un prédicteur fort de {C}. Par conséquent, pour comparer un résultat particulier à tous les résultats connus, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] calcule la probabilité de la règle individuelle (comme If {A,B} Then {C}) en divisant la prise en charge du jeu d’éléments {A,B,C} par la prise en charge de tous les jeux d’éléments connexes.  
  
 Vous pouvez restreindre le nombre de règles qu'un modèle produit en définissant une valeur pour MINIMUM_PROBABILITY.  
  
 Pour chaque règle créée, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère en sortie un score qui indique son *importance*, aussi appelée *finesse*. La finesse est calculée différemment pour les jeux d'éléments et les règles.  
  
 L'importance d'un jeu d'éléments est calculée comme la probabilité du jeu d'éléments divisée par la probabilité composée des éléments individuels dans le jeu d'éléments. Par exemple, si un jeu d'éléments contient {A,B}, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] compte en premier tous les cas qui contiennent cette combinaison A et B, divise ce nombre par le nombre total de cas, puis normalise la probabilité.  
  
 L'importance d'une règle est calculée par le logarithme du rapport de vraisemblance de la partie droite de la règle, étant donné la partie gauche de la règle. Par exemple, dans la règle `If {A} Then {B}`, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] calcule le ratio des cas avec A et B sur les cas avec B sans A, puis normalise ce quotient en utilisant une échelle logarithmique.  
  
### <a name="feature-selection"></a>Sélection des fonctionnalités  
 L'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) n'effectue aucune sélection automatique de fonctionnalités. En revanche, l'algorithme fournit des paramètres qui contrôlent les données utilisées par l'algorithme. Il peut s'agir d'application de limites sur la taille de chaque jeu d'éléments ou de la définition des prises en charge maximale et minimale requises pour ajouter un jeu d'éléments au modèle.  
  
-   Pour exclure par filtrage les éléments et les événements qui sont trop courants et donc inintéressants, diminuez la valeur de MAXIMUM_SUPPORT pour supprimer les jeux d'éléments très fréquents du modèle.  
  
-   Pour exclure par filtrage les éléments et les jeux d'éléments qui sont rares, augmentez la valeur de MINIMUM_SUPPORT.  
  
-   Pour exclure par filtrage des règles, augmentez la valeur de MINIMUM_PROBABILITY.  
  
## <a name="customizing-the-microsoft-association-rules-algorithm"></a>Personnalisation de l'algorithme MAR  
 L'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) prend en charge plusieurs paramètres qui affectent le comportement, les performances et la précision du modèle d'exploration de données résultant.  
  
### <a name="setting-algorithm-parameters"></a>Définition des paramètres de l'algorithme  
 Vous pouvez modifier à tout moment les paramètres d'un modèle d'exploration de données à l'aide du Concepteur d'exploration de données dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Vous pouvez également modifier les paramètres par programmation à l’aide de la <xref:Microsoft.AnalysisServices.MiningModel.AlgorithmParameters%2A> collection dans AMO, ou à l’aide de la [MiningModels élément &#40;ASSL&#41; ](../../analysis-services/scripting/collections/miningmodels-element-assl.md) dans XMLA. La table ci-dessous décrit chaque paramètre.  
  
> [!NOTE]  
>  Vous ne pouvez pas modifier les paramètres dans un modèle existant à l’aide d’une instruction DMX ; vous devez spécifier les paramètres dans DMX CREATE MODEL ou ALTER STRUCTURE… ADD MODEL quand vous créez le modèle.  
  
 *MAXIMUM_ITEMSET_COUNT*  
 Spécifie le nombre maximal de jeux d'éléments à produire. Si aucun nombre n'est spécifié, la valeur par défaut est utilisée.  
  
 La valeur par défaut est 200000.  
  
> [!NOTE]  
>  Les jeux d'éléments sont classés par prise en charge. Parmi les jeux d'éléments qui ont la même prise en charge, le classement est arbitraire.  
  
 *MAXIMUM_ITEMSET_SIZE*  
 Spécifie le nombre maximal d'éléments autorisés dans un jeu d'éléments. Une valeur de 0 spécifie qu'il n'y a pas de limite quant à la taille du jeu d'éléments.  
  
 La valeur par défaut est 3.  
  
> [!NOTE]  
>  Le fait de réduire cette valeur peut potentiellement accélérer la création du modèle, son traitement étant arrêté une fois la limite atteinte.  
  
 *MAXIMUM_SUPPORT*  
 Spécifie le nombre maximal de cas qu'un jeu d'éléments prend en charge. Ce paramètre peut être utilisé pour éliminer des éléments qui apparaissent fréquemment et qui par conséquent ont une signification éventuellement limitée.  
  
 Une valeur inférieure à 1 représente un pourcentage du nombre total de cas. Une valeur supérieure à 1 représente le nombre absolu de cas qui peuvent contenir le jeu d'éléments.  
  
 La valeur par défaut est 1.  
  
 *MINIMUM_ITEMSET_SIZE*  
 Spécifie le nombre minimal d'éléments autorisés dans un jeu d'éléments. Si vous augmentez ce nombre, le modèle peut contenir moins de jeux d'éléments. Ceci peut être utile si vous souhaitez ignorer les jeux d'éléments comportant un seul élément par exemple.  
  
 La valeur par défaut est 1.  
  
> [!NOTE]  
>  Vous ne pouvez pas réduire le temps de traitement du modèle en augmentant la valeur minimale car [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] doit calculer de toute façon des probabilités pour les éléments uniques dans le cadre du traitement. Toutefois, en augmentant cette valeur, vous pouvez exclure les jeux d'éléments de plus petite taille par filtrage.  
  
 *MINIMUM_PROBABILITY*  
 Spécifie la probabilité minimale qu'une règle ait la valeur True.  
  
 Par exemple, l'attribution de la valeur 0,5 spécifie que toute règle ayant une probabilité inférieure à 50 % ne peut pas être générée.  
  
 La valeur par défaut est 0,4.  
  
 *MINIMUM_SUPPORT*  
 Spécifie le nombre minimal de cas qui doivent contenir le jeu d'éléments avant que l'algorithme génère une règle.  
  
 Si vous attribuez une valeur inférieure à 1 à ce paramètre, le nombre minimal de cas est calculé sous la forme d'un pourcentage du nombre total de cas.  
  
 Si vous attribuez une valeur entière supérieure à 1, le nombre minimal de cas est calculé comme le nombre absolu de cas devant contenir le jeu d'éléments. L'algorithme peut augmenter automatiquement la valeur de ce paramètre si la mémoire est limitée.  
  
 La valeur par défaut est 0,03. Cela signifie qu'un jeu d'éléments doit être présent dans au moins 3 % des cas pour être inclus dans le modèle.  
  
 *OPTIMIZED_PREDICTION_COUNT*  
 Définit le nombre d'éléments à mettre en cache pour l'optimisation d'une prédiction.  
  
 La valeur par défaut est 0 : Lorsque la valeur par défaut est utilisée, l'algorithme produit autant de prédictions que demandé dans la requête.  
  
 Si vous spécifiez une valeur différente de zéro pour *OPTIMIZED_PREDICTION_COUNT* , les requêtes de prédiction peuvent retourner, au plus, le nombre spécifié d’éléments, même si vous demandez des prédictions supplémentaires. Toutefois, la définition d'une valeur peut améliorer les performances de la prédiction.  
  
 Par exemple, si la valeur définie est 3, l'algorithme met en cache uniquement 3 éléments pour la prédiction. Vous ne pouvez pas afficher des prédictions supplémentaires qui peuvent être de probabilité égale aux 3 éléments retournés.  
  
### <a name="modeling-flags"></a>Indicateurs de modélisation  
 Les indicateurs de modélisation suivants sont pris en charge pour une utilisation avec l'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules).  
  
 NOT NULL  
 Indique que la colonne ne peut pas contenir de valeur Null. Une erreur est générée si [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] rencontre une valeur NULL au cours de l'apprentissage du modèle.  
  
 S'applique à la colonne de structure d'exploration de données.  
  
 MODEL_EXISTENCE_ONLY  
 Signifie que la colonne sera considérée comme ayant deux états possibles : **Missing** et **Existing**. Une valeur NULL est une valeur manquante.  
  
 S'applique à la colonne du modèle d'exploration de données.  
  
## <a name="requirements"></a>Spécifications  
 Un modèle d'association doit contenir une colonne clé, des colonnes d'entrée et une colonne prédictible unique.  
  
### <a name="input-and-predictable-columns"></a>Colonnes d'entrée et prédictibles  
 L'algorithme MAR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Association Rules) prend en charge les colonnes d'entrée et les colonnes prédictibles répertoriées dans le tableau suivant. Pour plus d’informations sur la signification des types de contenu dans un modèle d’exploration de données, consultez [Types de contenu &#40;exploration de données&#41;](../../analysis-services/data-mining/content-types-data-mining.md).  
  
|Colonne|Types de contenu|  
|------------|-------------------|  
|Attribut d'entrée|Cyclique, discret, discrétisé, clé, table et trié|  
|Attribut prédictible|Cyclique, discret, discrétisé, table et trié|  
  
> [!NOTE]  
>  Les types de contenu Cyclique et Trié sont pris en charge, mais l'algorithme les traite comme des valeurs discrètes et n'effectue pas de traitement spécial.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme Microsoft Association](../../analysis-services/data-mining/microsoft-association-algorithm.md)   
 [Exemples de requêtes de modèle association](../../analysis-services/data-mining/association-model-query-examples.md)   
 [Contenu du modèle d’exploration de données pour les modèles d’Association & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-association-models-analysis-services-data-mining.md)  
  
  
