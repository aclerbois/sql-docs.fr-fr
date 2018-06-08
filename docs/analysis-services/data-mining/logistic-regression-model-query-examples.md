---
title: Exemples de requêtes de modèle de régression logistique | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 78a89577c35f1effe00fe35685640c229811860d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="logistic-regression-model-query-examples"></a>Exemples de requêtes de modèle de régression logistique
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez créer une requête de contenu, qui fournit des détails sur les modèles (ou séquences) découverts au cours de l'analyse, ou créer une requête de prédiction, qui utilise les séquences du modèle pour effectuer des prédictions à l'aide de nouvelles données.  
  
 Cette section explique comment créer des requêtes pour les modèles basés sur l'algorithme MLR (Microsoft Logistic Regression).  
  
 **Content Queries**  
  
 [Récupération des paramètres du modèle à l'aide de l'ensemble de lignes de schéma d'exploration de données](#bkmk_Query1)  
  
 [Recherche d'informations supplémentaires relatives au modèle à l'aide de DMX](#bkmk_Query2)  
  
 **Requêtes de prédiction**  
  
 [Élaboration de prédictions pour une valeur continue](#bkmk_Query3)  
  
 [Élaboration de prédictions pour une valeur discrète](#bkmk_Query4)  
  
##  <a name="bkmk_top"></a> Obtention d'informations sur le modèle de régression logistique  
 Les modèles de régression logistique sont créés en utilisant l'algorithme MNR (Microsoft Neural Network) avec un ensemble spécial de paramètres ; par conséquent, un modèle de régression logistique possède certaines informations identiques à un modèle de réseau neuronal, mais est moins complexe. Pour comprendre la structure du contenu du modèle et les types d’informations stockés dans les différents types de nœuds, consultez [Mining Model Content for Logistic Regression Models &#40;Analysis Services - Data Mining&#41;](../../analysis-services/data-mining/mining-model-content-for-logistic-regression-models.md).  
  
 Pour suivre les scénarios de requête, vous pouvez créer un modèle de régression logistique comme décrit dans la section suivante du Didacticiel intermédiaire sur l’exploration de données : [Lesson 5: Building Neural Network and Logistic Regression Models &#40;Intermediate Data Mining Tutorial&#41;](http://msdn.microsoft.com/library/42c3701a-1fd2-44ff-b7de-377345bbbd6b).  
  
 Vous pouvez aussi utiliser la structure d’exploration de données (publipostage ciblé) du [Didacticiel sur l’exploration de données de base](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c).  
  
```  
ALTER MINING STRUCTURE [Targeted Mailing]  
ADD MINING MODEL [TM_Logistic Regression]  
([Customer Key],  
[Age],  
[Bike Buyer] PREDICT,  
[Yearly Income] PREDICT,  
[Commute Distance],  
[English Education],  
Gender,  
[House Owner Flag],  
[Marital Status],  
[Number Cars Owned],  
[Number Children At Home],  
[Region],  
[Total Children]  
)  
USING Microsoft_Logistic_Regression  
```  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : récupération des paramètres du modèle à l'aide de l'ensemble de lignes de schéma d'exploration de données  
 En interrogeant l'ensemble de lignes de schéma d'exploration de données, vous pouvez rechercher les métadonnées relatives au modèle, par exemple sa date de création, le moment où il a été traité pour la dernière fois, le nom de la structure d'exploration de données sur laquelle il est basé et le nom de la colonne utilisée comme attribut prédictible. L'exemple suivant retourne les paramètres utilisés lorsque le modèle a été créé, ainsi que le nom et le type du modèle et sa date de création.  
  
```  
SELECT MODEL_NAME, SERVICE_NAME, DATE_CREATED, MINING_PARAMETERS   
FROM $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = 'Call Center_LR'  
```  
  
 Exemples de résultats :  
  
|MODEL_NAME|SERVICE_NAME|DATE_CREATED|MINING_PARAMETERS|  
|-----------------|-------------------|-------------------|------------------------|  
|Call Center_LR|Microsoft_Logistic_Regression|04/07/2009 20:38:33|HOLDOUT_PERCENTAGE=30, HOLDOUT_SEED=1, MAXIMUM_INPUT_ATTRIBUTES=255, MAXIMUM_OUTPUT_ATTRIBUTES=255, MAXIMUM_STATES=100, SAMPLE_SIZE=10000|  
  
###  <a name="bkmk_Query2"></a> Exemple de requête 2 : recherche d'informations supplémentaires relatives au modèle à l'aide de DMX  
 La requête suivante retourne des informations de base relatives au modèle de régression logistique. Un modèle de régression logistique est à bien des égards semblable à un modèle de réseau neuronal, y compris en ce qui concerne la présence d'un nœud statistique marginal (NODE_TYPE = 24), qui décrit les valeurs utilisées comme entrées. Cet exemple de requête utilise le modèle de publipostage ciblé et obtient les valeurs de toutes les entrées en les récupérant de la table imbriquée, NODE_DISTRIBUTION.  
  
```  
SELECT FLATTENED NODE_DISTRIBUTION AS t  
FROM [TM_Logistic Regression].CONTENT   
```  
  
 Résultats partiels :  
  
|t.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|t.SUPPORT|t.PROBABILITY|t.VARIANCE|t.VALUETYPE|  
|-----------------------|------------------------|---------------|-------------------|----------------|-----------------|  
|Age|Manquant|0|0|0|1|  
|Age|45.43491192|17484|1|126.9544114|3|  
|Bike Buyer|Manquant|0|0|0|1|  
|Bike Buyer|0|8869|0.507263784|0|4|  
|Bike Buyer|1|8615|0.492736216|0|4|  
|Commute Distance|Manquant|0|0|0|1|  
|Commute Distance|5-10 Miles|3033|0.173472889|0|4|  
  
 La requête réelle retourne beaucoup plus de lignes. Toutefois, cet exemple illustre le type d'informations fournies à propos des entrées. Pour les entrées discrètes, chaque valeur possible est répertoriée dans la table. Pour les entrées de valeur continue telles que l’attribut Age, une liste complète est impossible, donc l’entrée est discrétisée en tant que moyenne. Pour plus d’informations sur l’utilisation des informations du nœud de statistiques marginales, consultez [Mining Model Content for Logistic Regression Models &#40;Analysis Services - Data Mining&#41;](../../analysis-services/data-mining/mining-model-content-for-logistic-regression-models.md).  
  
> [!NOTE]  
>  Les résultats ont été aplatis pour un affichage plus aisé, mais vous pouvez retourner la table imbriquée dans une colonne unique si votre fournisseur prend en charge des ensembles de lignes hiérarchiques.  
  
## <a name="prediction-queries-on-a-logistic-regression-model"></a>Requêtes de prédiction sur un modèle de régression logistique  
 Vous pouvez utiliser la fonction [Predict &#40;DMX&#41;](../../dmx/predict-dmx.md) avec n’importe quel type de modèle d’exploration de données pour fournir de nouvelles données au modèle et élaborer des prédictions basées sur les nouvelles valeurs. Vous pouvez aussi utiliser des fonctions pour retourner des informations supplémentaires sur la prédiction, comme la probabilité que la prédiction soit correcte. Cette section fournit des exemples de requêtes de prédiction sur un modèle de régression logistique.  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3 : élaboration de prédictions pour une valeur continue  
 Il est facile de créer des modèles qui mettent en corrélation différents facteurs dans vos données, car la régression logistique prend en charge l'utilisation d'attributs continus à la fois pour les entrées et la prédiction. Vous pouvez utiliser des requêtes de prédiction pour explorer la relation entre ces facteurs.  
  
 L'exemple de requête suivant est basé sur le modèle Call Center (centre d'appels), du Didacticiel intermédiaire, et crée une requête singleton qui prédit le niveau de service de l'équipe du vendredi matin. La fonction [PredictHistogram (DMX)](../../dmx/predicthistogram-dmx.md) retourne une table imbriquée qui fournit des statistiques pertinentes pour comprendre la validité de la valeur prédite.  
  
```  
SELECT  
  Predict([Call Center_LR].[Service Grade]) as Predicted ServiceGrade,  
  PredictHistogram([Call Center_LR].[Service Grade]) as [Results],  
FROM  
  [Call Center_LR]  
NATURAL PREDICTION JOIN  
(SELECT 'Friday' AS [Day Of Week],  
  'AM' AS [Shift]) AS t  
```  
  
 Exemples de résultats :  
  
|Niveau de service (Service Grade) prédit|Service Grade (Niveau de service)|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|$VARIANCE|$STDEV|  
|-----------------------------|-------------------|--------------|------------------|--------------------------|---------------|------------|  
|0.102601830123659|0.102601830123659|83.0232558139535|0.988372093023256|0|0.00120552660600087|0.034720694203902|  
|||0.976744186046512|0.0116279069767442|0.0116279069767442|0|0|  
  
 Pour plus d’informations sur les valeurs de probabilité, de prise en charge et d’écart type dans la table NODE_DISTRIBUTION imbriquée, consultez [Mining Model Content for Logistic Regression Models &#40;Analysis Services - Data Mining&#41;](../../analysis-services/data-mining/mining-model-content-for-logistic-regression-models.md).  
  
###  <a name="bkmk_Query4"></a> Exemple de requête 4 : élaboration de prédictions pour une valeur discrète  
 La régression logistique est généralement utilisée dans les scénarios où vous souhaitez analyser les facteurs qui contribuent à un résultat binaire. Bien que le modèle utilisé dans le didacticiel prédise une valeur continue, **ServiceGrade**, dans la réalité, vous préfèrerez peut-être le configurer pour qu’il prédise si le niveau de service atteindra une valeur cible discrétisée. Vous pouvez aussi sortir les prédictions en utilisant une valeur continue pour par la suite regrouper les résultats prédits sous les libellés **Bon**, **Correct**ou **Médiocre**.  
  
 L'exemple suivant illustre comment modifier la manière dont l'attribut prédictible est groupé. Pour cela, vous créez une copie de la structure d'exploration de données, puis modifiez la méthode de discrétisation de la colonne cible afin que les valeurs soient groupées plutôt que continues.  
  
 La procédure suivante montre comment modifier le regroupement des valeurs Service Grade dans les données du centre d’appels.  
  
##### <a name="to-create-a-discretized-version-of-the-call-center-mining-structure-and-models"></a>Pour créer une version discrétisée de la structure d'exploration de données et des modèles du centre d'appels  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], dans l’Explorateur de solutions, développez **Structures d’exploration de données**.  
  
2.  Cliquez avec le bouton droit sur Call Center.dmm, puis sélectionnez **Copier**.  
  
3.  Cliquez avec le bouton droit sur **Structures d’exploration de données** , puis sélectionnez **Coller**. Une nouvelle structure d’exploration de données est ajoutée, nommée Call Center 1.  
  
4.  Cliquez avec le bouton droit sur la nouvelle structure d’exploration de données et sélectionnez **Renommer**. Tapez le nouveau nom, **Centre d’appels discrétisé**.  
  
5.  Double-cliquez sur la nouvelle structure d'exploration de données pour l'ouvrir dans le concepteur. Remarquez que tous les modèles d'exploration de données ont été également copiés et que tous ont l'extension 1. Laissez les noms tels quels pour le moment.  
  
6.  Sous l’onglet **Structure d’exploration de données** , cliquez avec le bouton droit sur la colonne de Service Grade, puis sélectionnez **Propriétés**.  
  
7.  Modifiez le paramétrage de la propriété **Content** en le faisant passer de **Continuous** à **Discretized**. Modifiez la propriété **DiscretizationMethod** en lui attribuant la valeur **Clusters**. Pour DiscretizationBucketCount, tapez **3**.  
  
    > [!NOTE]  
    >  Ces paramètres sont utilisés juste pour illustrer le processus et ne produisent pas nécessairement de modèle valide.  
  
8.  Dans le menu **Modèle d’exploration de données** , sélectionnez **Traiter la structure d’exploration de données et tous les modèles**.  
  
 L'exemple de requête suivant est basé sur ce modèle discrétisé et prédit le niveau de service pour le jour spécifié de la semaine, ainsi que les probabilités pour chaque résultat prédit.  
  
```  
SELECT  
  (PredictHistogram([Call Center_LR 1].[Service Grade])) as [Predictions]  
FROM  
  [Call Center_LR 1]  
NATURAL PREDICTION JOIN  
(SELECT 'Saturday' AS [Day Of Week]) AS t    
```  
  
 Résultats attendus :  
  
 **Prédictions :**  
  
|Service Grade (Niveau de service)|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|$VARIANCE|$STDEV|  
|-------------------|--------------|------------------|--------------------------|---------------|------------|  
|0.10872718383125|35.7246504770641|0.425293458060287|0.0170168360030293|0|0|  
|0.05855769230625|31.7098880800703|0.377498667619885|0.020882020060454|0|0|  
|0.170169491525|15.6109159883202|0.185844237956192|0.0661386571386049|0|0|  
||0.954545454545455|0.0113636363636364|0.0113636363636364|0|0|  
  
 Notez que les résultats prédits ont été groupés en trois catégories, comme spécifié ; toutefois, ces regroupements sont basés sur le clustering de valeurs réelles dans les données, et non sur des valeurs arbitraires que vous pouvez définir comme objectifs stratégiques.  
  
## <a name="list-of-prediction-functions"></a>Liste des fonctions de prédiction  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Toutefois, l'algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Logistic Regression) prend en charge les fonctions supplémentaires qui sont décrites dans le tableau suivant.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[IsDescendant & #40 ; DMX & #41 ;](../../dmx/isdescendant-dmx.md)|Détermine si un nœud est un enfant d'un autre nœud dans le modèle.|  
|[PredictAdjustedProbability & #40 ; DMX & #41 ;](../../dmx/predictadjustedprobability-dmx.md)|Retourne la probabilité ajustée d'un état spécifié.|  
|[PredictHistogram & #40 ; DMX & #41 ;](../../dmx/predicthistogram-dmx.md)|Retourne une valeur ou un ensemble de valeurs prédites pour une colonne spécifiée.|  
|[PredictProbability & #40 ; DMX & #41 ;](../../dmx/predictprobability-dmx.md)|Retourne la probabilité pour un état spécifié.|  
|[PredictStdev & #40 ; DMX & #41 ;](../../dmx/predictstdev-dmx.md)|Retourne l'écart-type pour la valeur prédite.|  
|[PredictSupport & #40 ; DMX & #41 ;](../../dmx/predictsupport-dmx.md)|Retourne la valeur de support pour un état spécifié.|  
|[PredictVariance & #40 ; DMX & #41 ;](../../dmx/predictvariance-dmx.md)|Retourne la variance d'une colonne spécifiée.|  
  
 Pour obtenir la liste des fonctions communes à tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)], consultez [Fonctions de prédiction générales &#40;DMX&#41;](../../dmx/general-prediction-functions-dmx.md). Pour en savoir plus sur la syntaxe de fonctions spécifiques, consultez [Fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
> [!NOTE]  
>  Pour les modèles de réseau neuronal et de régression logistique, la fonction [PredictSupport &#40;DMX&#41;](../../dmx/predictsupport-dmx.md) retourne une valeur unique qui représente la taille du jeu d’apprentissage pour le modèle entier.  
  
## <a name="see-also"></a>Voir aussi  
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)   
 [Algorithme Microsoft Logistic Regression](../../analysis-services/data-mining/microsoft-logistic-regression-algorithm.md)   
 [Référence technique de Microsoft Logistic Regression algorithme](../../analysis-services/data-mining/microsoft-logistic-regression-algorithm-technical-reference.md)   
 [Contenu du modèle d’exploration de données pour les modèles de régression logistique & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-logistic-regression-models.md)   
 [Leçon 5 : Génération neuronal réseau et les modèles de régression logistique & #40 ; didacticiel d’exploration de données intermédiaires & #41 ;](http://msdn.microsoft.com/library/42c3701a-1fd2-44ff-b7de-377345bbbd6b)  
  
  
