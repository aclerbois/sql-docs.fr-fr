---
title: Exemples de requêtes de modèle des arbres de décision | Documents Microsoft
ms.date: 05/01/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b2045dfa9923fb745f0f9d3936579a4e73a50564
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="decision-trees-model-query-examples"></a>Exemples de requêtes de modèle d'arbre de décision
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez créer une requête de contenu, qui fournit des détails sur les modèles (ou séquences) découverts au cours de l'analyse, ou créer une requête de prédiction, qui utilise les séquences du modèle pour effectuer des prédictions pour les nouvelles données. Par exemple, une requête de contenu pour un modèle d'arbre de décision peut fournir des statistiques sur le nombre de cas à chaque niveau de l'arbre, ou les règles qui font la différence entre des cas. En revanche, une requête de prédiction mappe le modèle à de nouvelles données pour générer des recommandations, des classifications, etc. Vous pouvez également extraire les métadonnées relatives au modèle en utilisant une requête.  
  
 Cette section explique comment créer des requêtes pour les modèles basés sur l'algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees).  
  
 **Content Queries**  
  
 [Récupération des paramètres du modèle à partir de l'ensemble de lignes de schéma d'exploration de données](#bkmk_Query1)  
  
 [Obtention de détails à propos des arborescences dans le modèle à l'aide de DMX](#bkmk_Query2)  
  
 [Récupération de sous-arborescences du modèle](#bkmk_Query3)  
  
 **Requêtes de prédiction**  
  
 [Retour de prédictions avec des probabilités](#bkmk_Query4)  
  
 [Prédiction d'associations à partir d'un modèle d'arbres de décision](#bkmk_Query5)  
  
 [Récupération d'une formule de régression d'un modèle d'arbres de décision](#bkmk_Query6)  
  
##  <a name="bkmk_top2"></a> Recherche d'informations à propos d'un modèle d'arbre de décision  
 Pour créer des requêtes explicites sur le contenu d'un modèle d'arbre de décision, vous devez comprendre la structure du contenu du modèle et les types d'informations stockés dans les différents types de nœuds. Pour plus d’informations, consultez [Mining Model Content for Decision Tree Models &#40;Analysis Services - Data Mining&#41;](../../analysis-services/data-mining/mining-model-content-for-decision-tree-models-analysis-services-data-mining.md).  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : récupération des paramètres du modèle à partir de l'ensemble de lignes de schéma d'exploration de données  
 En interrogeant l'ensemble de lignes de schéma d'exploration de données, vous pouvez rechercher les métadonnées relatives au modèle, par exemple sa date de création, le moment où il a été traité pour la dernière fois, le nom de la structure d'exploration de données sur laquelle il est basé et le nom de la colonne utilisée comme attribut prédictible. Vous pouvez également retourner les paramètres qui ont été utilisés lorsque le modèle a été créé pour la première fois.  
  
```  
select MINING_PARAMETERS   
from $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = 'TM_Decision Tree'  
```  
  
 Exemples de résultats :  
  
 MINING_PARAMETERS  
  
 COMPLEXITY_PENALTY=0.5, MAXIMUM_INPUT_ATTRIBUTES=255,MAXIMUM_OUTPUT_ATTRIBUTES=255,MINIMUM_SUPPORT=10,SCORE_METHOD=4,SPLIT_METHOD=3,FORCE_REGRESSOR=  
  
###  <a name="bkmk_Query2"></a> Exemple de requête 2 : retour de détails à propos du contenu du modèle à l'aide de DMX  
 La requête suivante retourne des informations de base sur les arbres de décision qui ont été créés lors de la génération du modèle dans le [Didacticiel sur l’exploration de données de base](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). Chaque arborescence est stockée dans son propre nœud. Étant donné que ce modèle contient un seul attribut prédictible, il n'y a qu'un seul nœud d'arbre. Toutefois, si vous créez un modèle d'association à l'aide de l'algorithme MDT, il peut y avoir des centaines d'arbres, un pour chaque produit.  
  
 Cette requête retourne tous les nœuds de type 2, lesquels sont les nœuds de niveau supérieur d'un arbre qui représente un attribut prédictible particulier.  
  
> [!NOTE]  
>  La colonne, CHILDREN_CARDINALITY, doit être placée entre parenthèses afin de la distinguer du mot clé réservé MDX du même nom.  
  
```  
SELECT MODEL_NAME, NODE_NAME, NODE_CAPTION,   
NODE_SUPPORT, [CHILDREN_CARDINALITY]  
FROM TM_DecisionTrees.CONTENT  
WHERE NODE_TYPE = 2  
```  
  
 Résultats de l'exemple :  
  
|MODEL_NAME|NODE_NAME|NODE_CAPTION|NODE_SUPPORT|CHILDREN_CARDINALITY|  
|-----------------|----------------|-------------------|-------------------|---------------------------|  
|TM_DecisionTree|000000001|Tous|12939|5|  
  
 Que vous indiquent ces résultats ? Dans un modèle d'arbre de décision, la cardinalité d'un nœud particulier vous indique le nombre d'enfants immédiats de ce nœud. La cardinalité de ce nœud est de 5, ce qui signifie que le modèle a divisé la population cible d'acheteurs de bicyclettes potentiels en 5 sous-groupes.  
  
 La requête associée suivante retourne les enfants de ces cinq sous-groupes, ainsi que la distribution d'attributs et de valeurs dans les nœuds enfants. Étant donné que des statistiques, comme la prise en charge, la probabilité et la variance, sont stockées dans la table imbriquée, NODE_DISTRIBUTION, cet exemple utilise le mot clé `FLATTENED` pour produire les colonnes de la table imbriquée.  
  
> [!NOTE]  
>  La colonne de table imbriquée, SUPPORT, doit être placée entre parenthèses afin de la distinguer du mot clé réservé du même nom.  
  
```  
SELECT FLATTENED NODE_NAME, NODE_CAPTION,  
(SELECT ATTRIBUTE_NAME, ATTRIBUTE_VALUE, [SUPPORT]  
FROM NODE_DISTRIBUTION) AS t  
FROM TM_DecisionTree.CONTENT  
WHERE [PARENT_UNIQUE_NAME] = '000000001'  
```  
  
 Résultats de l'exemple :  
  
|NODE_NAME|NODE_CAPTION|T.ATTRIBUTE_NAME|T.ATTRIBUTE_VALUE|SUPPORT|  
|----------------|-------------------|-----------------------|------------------------|-------------|  
|00000000100|Number Cars Owned = 0|Bike Buyer|Manquant|0|  
|00000000100|Number Cars Owned = 0|Bike Buyer|0|1067|  
|00000000100|Number Cars Owned = 0|Bike Buyer|1|1875|  
|00000000101|Number Cars Owned = 3|Bike Buyer|Manquant|0|  
|00000000101|Number Cars Owned = 3|Bike Buyer|0|678|  
|00000000101|Number Cars Owned = 3|Bike Buyer|1|473|  
  
 D’après ces résultats, vous pouvez déterminer que parmi les clients qui ont acheté un vélo ([Bike Buyer] = 1), 1 067 clients n’avaient pas de voitures et 473 clients avaient 3 voitures.  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3 : récupération de sous-arborescences à partir du modèle  
 Imaginons que vous souhaitiez en savoir plus sur les clients qui ont acheté un vélo. Vous pouvez afficher des détails supplémentaires pour chacun des sous-arbres en utilisant la fonction [IsDescendant &#40;DMX&#41;](../../dmx/isdescendant-dmx.md) dans la requête, comme le montre l’exemple suivant. La requête retourne le nombre d'acheteurs de vélos en extrayant les nœuds terminaux (NODE_TYPE = 4) de l'arbre qui contient les clients âgés de plus de 42 ans. La requête limite les lignes de la table imbriquée à ceux pour lesquels Bike Buyer = 1.  
  
```  
SELECT FLATTENED NODE_NAME, NODE_CAPTION,NODE_TYPE,  
(  
SELECT [SUPPORT] FROM NODE_DISTRIBUTION WHERE ATTRIBUTE_NAME = 'Bike Buyer' AND ATTRIBUTE_VALUE = '1'  
) AS t  
FROM TM_DecisionTree.CONTENT  
WHERE ISDESCENDANT('0000000010001')  
AND NODE_TYPE = 4  
```  
  
 Résultats de l'exemple :  
  
|NODE_NAME|NODE_CAPTION|t.SUPPORT|  
|----------------|-------------------|---------------|  
|000000001000100|Yearly Income >= 26000 et < 42000|266|  
|00000000100010100|Total Children = 3|75|  
|0000000010001010100|Number Children At Home = 1|75|  
  
## <a name="making-predictions-using-a-decision-trees-model"></a>Réaliser des prédictions à l'aide d'un modèle d'arbres de décision  
 Étant donné que les arbres de décision peuvent être utilisés pour différentes tâches, notamment la classification, la régression et même l'association, lorsque vous créez une requête de prédiction sur un modèle d'arbre de décision, vous disposez de nombreuses options. Pour comprendre les résultats de la prédiction, vous devez comprendre le but pour lequel le modèle a été créé. Les exemples de requêtes suivants illustrent trois scénarios différents :  
  
-   retour d'une prédiction pour un modèle de classification, ainsi que de la probabilité d'exactitude de la prédiction, puis filtrage des résultats en fonction de la probabilité ;  
  
-   création d'une requête singleton pour prédire des associations ;  
  
-   extraction de la formule de régression pour une partie d'un arbre de décision où la relation entre l'entrée et la sortie est linéaire.  
  
###  <a name="bkmk_Query4"></a> Exemple de requête 4 : retour de prédictions avec des probabilités  
 L’exemple de requête suivant utilise le modèle d’arbre de décision qui a été créé dans le [Didacticiel sur l’exploration de données de base](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). La requête passe dans un nouveau jeu d’exemples de données, de la tabledbo.ProspectiveBuyers dans [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] DW, pour prédire quels clients du nouveau jeu de données achèteront un vélo.  
  
 La requête utilise la fonction de prédiction [PredictHistogram &#40;DMX&#41;](../../dmx/predicthistogram-dmx.md), qui retourne une table imbriquée qui contient des informations utiles sur les probabilités découvertes par le modèle. La clause WHERE finale de la requête filtre les résultats afin de retourner uniquement les clients prédits comme acheteurs de vélos potentiels, avec une probabilité supérieure à 0 %.  
  
```  
SELECT  
  [TM_DecisionTree].[Bike Buyer],  
  PredictHistogram([Bike Buyer]) as Results  
From  
  [TM_DecisionTree]  
PREDICTION JOIN  
  OPENQUERY([Adventure Works DW Multidimensional 2012],  
    'SELECT  
      [FirstName],  
      [LastName],  
      [MaritalStatus],  
      [Gender],  
      [YearlyIncome],  
      [TotalChildren],  
      [NumberChildrenAtHome],  
      [HouseOwnerFlag],  
      [NumberCarsOwned]  
    FROM  
      [dbo].[ProspectiveBuyer]  
    ') AS t  
ON  
  [TM_DecisionTree].[First Name] = t.[FirstName] AND  
  [TM_DecisionTree].[Last Name] = t.[LastName] AND  
  [TM_DecisionTree].[Marital Status] = t.[MaritalStatus] AND  
  [TM_DecisionTree].[Gender] = t.[Gender] AND  
  [TM_DecisionTree].[Yearly Income] = t.[YearlyIncome] AND  
  [TM_DecisionTree].[Total Children] = t.[TotalChildren] AND  
  [TM_DecisionTree].[Number Children At Home] = t.[NumberChildrenAtHome] AND  
  [TM_DecisionTree].[House Owner Flag] = t.[HouseOwnerFlag] AND  
  [TM_DecisionTree].[Number Cars Owned] = t.[NumberCarsOwned]  
WHERE [Bike Buyer] = 1  
AND PredictProbability([Bike Buyer]) >'.05'  
```  
  
 Par défaut, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne des tables imbriquées avec l’étiquette de colonne, **Expression**. Vous pouvez modifier cette étiquette en utilisant un alias pour la colonne qui est retournée. En procédant ainsi, l’alias (dans le cas présent, **Results**) est utilisé à la fois comme en-tête de colonne et comme valeur dans la table imbriquée. Vous devez développer la table imbriquée pour afficher les résultats.  
  
 Exemple de résultats avec **Bike Buyer** = 1 :  
  
|Bike Buyer|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|$VARIANCE|$STDEV|  
|----------------|--------------|------------------|--------------------------|---------------|------------|  
|1|2540|0.634849242045644|0.013562168281562|0|0|  
|0|1460|0.364984174579377|0.00661336932550915|0|0|  
||0|0.000166583374979177|0.000166583374979177|0|0|  
  
 Si votre fournisseur ne prend pas en charge les ensembles de lignes hiérarchiques tels que ceux présentés ici, vous pouvez utiliser le mot clé FLATTENED dans la requête pour retourner, sous forme de table, les résultats qui contiennent des valeurs Null à la place des valeurs de colonne répétées. Pour plus d’informations, consultez [Tables imbriquées &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/nested-tables-analysis-services-data-mining.md) ou [Présentation de l’instruction DMX Select ](../../dmx/understanding-the-dmx-select-statement.md).  
  
###  <a name="bkmk_Query5"></a> Exemple de requête 5 : prédiction d'associations à partir d'un modèle d'arbres de décision  
 L’exemple de requête suivant est basé sur la structure d’exploration de données Association. Pour pouvoir suivre cet exemple, vous pouvez ajouter un nouveau modèle à cette structure d'exploration de données et sélectionner l'algorithme MDT (Microsoft Decision Trees). Pour plus d’informations sur la création de la structure d’exploration de données Association, consultez [Leçon 3 : Génération d’un scénario de panier d’achat &#40;Didacticiel d’exploration de données intermédiaire&#41;](http://msdn.microsoft.com/library/651eef38-772e-4d97-af51-075b1b27fc5a).  
  
 L'exemple de requête suivant est une requête singleton que vous pouvez facilement créer dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] en choisissant des champs, puis en sélectionnant des valeurs pour ces champs dans une liste déroulante.  
  
```  
SELECT PredictAssociation([DT_Association].[v Assoc Seq Line Items],3)  
FROM  
  [DT_Association]  
NATURAL PREDICTION JOIN  
(SELECT (SELECT 'Patch kit' AS [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
 Résultats attendus :  
  
|Modèle|  
|-----------|  
|Mountain-200|  
|Mountain Tire Tube|  
|Touring Tire Tube (Pneu pour vélo de tourisme)|  
  
 Les résultats vous indiquent les trois meilleurs produits à recommander aux clients qui ont acheté le produit Patch Kit. Vous pouvez également fournir plusieurs produits comme entrée quand vous faites des recommandations, soit en entrant des valeurs, soit en ajoutant ou en supprimant des valeurs à l’aide de la boîte de dialogue **Entrée de requête singleton** . L'exemple de requête suivant montre comment les valeurs multiples sur lesquelles faire une prédiction sont fournies. Les valeurs sont connectées par une clause UNION dans l'instruction SELECT qui définit les valeurs d'entrée.  
  
```  
SELECT PredictAssociation([DT_Association].[v Assoc Seq Line Items],3)  
From  
  [DT_Association]  
NATURAL PREDICTION JOIN  
(SELECT (SELECT 'Racing Socks' AS [Model]  
  UNION SELECT 'Women''s Mountain Shorts' AS [Model]) AS [v Assoc Seq Line Items]) AS t  
```  
  
 Résultats attendus :  
  
|Modèle|  
|-----------|  
|Long-Sleeve Logo Jersey (Pull-over à manches longues)|  
|Mountain-400-W|  
|Classic Vest|  
  
###  <a name="bkmk_Query6"></a> Exemple de requête 6 : récupération d'une formule de régression à partir d'un modèle d'arbres de décision  
 Lorsque vous créez un modèle d'arbre de décision qui contient une régression sur un attribut continu, vous pouvez utiliser la formule de régression pour effectuer des prédictions, ou extraire des informations relatives à la formule de régression. Pour plus d’informations sur les requêtes sur les modèles de régression, consultez [Exemples de requête de modèle de régression linéaire](../../analysis-services/data-mining/linear-regression-model-query-examples.md).  
  
 Si le modèle d'arbre de décision contient un mélange de nœuds de régression et de nœuds qui se divisent sur les plages ou les attributs discrets, vous pouvez créer une requête qui retourne uniquement le nœud de régression. La table NODE_DISTRIBUTION contient les détails de la formule de régression. Dans cet exemple, les colonnes sont aplaties et la table NODE_DISTRIBUTION a un alias afin de faciliter sa lecture. Cependant, dans ce modèle, aucun régresseur n’a été trouvé pour lier le revenu (Income) à d’autres attributs continus. Dans ce cas-là, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne la valeur moyenne de l'attribut et la variance totale dans le modèle pour cet attribut.  
  
```  
SELECT FLATTENED NODE_DISTRIBUTION AS t  
FROM DT_Predict. CONTENT  
WHERE NODE_TYPE = 25  
```  
  
 Résultats de l'exemple :  
  
|T.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|t.SUPPORT|t.PROBABILITY|t.VARIANCE|t.VALUETYPE|  
|-----------------------|------------------------|---------------|-------------------|----------------|-----------------|  
|Yearly Income|Manquant|0|0.000457142857142857|0|1|  
|Yearly Income|57220.8876687257|17484|0.999542857142857|1041275619.52776|3|  
||57220.8876687257|0|0|1041216662.54387|11|  
  
 Pour plus d’informations sur types de valeurs et les statistiques utilisés dans les nœuds de régression, consultez [Contenu du modèle d’exploration de données pour les modèles de régression linéaire &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md).  
  
## <a name="list-of-prediction-functions"></a>Liste des fonctions de prédiction  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Cependant, l’algorithme MDT ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Decision Trees) prend en charge les fonctions supplémentaires répertoriées dans le tableau ci-dessous.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[IsDescendant & #40 ; DMX & #41 ;](../../dmx/isdescendant-dmx.md)|Détermine si un nœud est un enfant d'un autre nœud dans le modèle.|  
|[IsInNode & #40 ; DMX & #41 ;](../../dmx/isinnode-dmx.md)|Indique si le nœud spécifié contient le cas courant.|  
|[PredictAdjustedProbability & #40 ; DMX & #41 ;](../../dmx/predictadjustedprobability-dmx.md)|Retourne la probabilité pondérée.|  
|[PredictAssociation & #40 ; DMX & #41 ;](../../dmx/predictassociation-dmx.md)|Prédit l'appartenance à un dataset associatif.|  
|[PredictHistogram & #40 ; DMX & #41 ;](../../dmx/predicthistogram-dmx.md)|Retourne une table des valeurs associées à la valeur prédite actuelle.|  
|[PredictNodeId & #40 ; DMX & #41 ;](../../dmx/predictnodeid-dmx.md)|Retourne Node_ID pour chaque cas.|  
|[PredictProbability & #40 ; DMX & #41 ;](../../dmx/predictprobability-dmx.md)|Retourne la probabilité pour la valeur prédite.|  
|[PredictStdev & #40 ; DMX & #41 ;](../../dmx/predictstdev-dmx.md)|Retourne l'écart-type prévu pour la colonne spécifiée.|  
|[PredictSupport & #40 ; DMX & #41 ;](../../dmx/predictsupport-dmx.md)|Retourne la valeur de support pour un état spécifié.|  
|[PredictVariance & #40 ; DMX & #41 ;](../../dmx/predictvariance-dmx.md)|Retourne la variance d'une colonne spécifiée.|  
  
 Pour obtenir la liste des fonctions communes à tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)], consultez [Fonctions de prédiction générales &#40;DMX&#41;](../../dmx/general-prediction-functions-dmx.md). Pour la syntaxe de fonctions spécifiques, consultez [Fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)   
 [Algorithme d’arbres de décision Microsoft](../../analysis-services/data-mining/microsoft-decision-trees-algorithm.md)   
 [Techniques de l’algorithme d’arbres de décision Microsoft](../../analysis-services/data-mining/microsoft-decision-trees-algorithm-technical-reference.md)   
 [Contenu du modèle d’exploration de données pour les modèles d’arbre de décision & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-decision-tree-models-analysis-services-data-mining.md)  
  
  
