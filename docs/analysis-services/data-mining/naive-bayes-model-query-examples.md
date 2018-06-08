---
title: Exemples de requêtes de modèle Naive Bayes | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4f4ea8c07865980caa6f817e2920599d1a9003d0
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="naive-bayes-model-query-examples"></a>Exemples de requêtes de modèle Naive Bayes
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez soit créer une requête de contenu, qui fournit des détails sur les modèles (ou séquences) découverts au cours de l'analyse, soit créer une requête de prédiction, qui utilise les séquences du modèle pour effectuer des prédictions pour les nouvelles données. Vous pouvez également récupérer les métadonnées relatives au modèle en utilisant une requête sur l'ensemble de lignes de schéma d'exploration de données. Cette section explique comment créer ces requêtes pour les modèles basés sur l'algorithme MNB (Microsoft Naive Bayes).  
  
 **Requêtes de contenu**  
  
 [Obtention de métadonnées de modèle avec DMX](#bkmk_Query1)  
  
 [Récupération d'un résumé de données d'apprentissage](#bkmk_Query2)  
  
 [Recherche d'informations sur les attributs](#bkmk_Query3)  
  
 [Utilisation des procédures stockées système](#bkmk_Query4)  
  
 **Requêtes de prédiction**  
  
 [Prédiction des résultats à l'aide d'une requête singleton](#bkmk_Query5)  
  
 [Obtention de prédictions avec une probabilité et des valeurs de prise en charge](#bkmk_Query6)  
  
 [Prédiction d'associations](#bkmk_Query7)  
  
## <a name="finding-information-about-a-naive-bayes-model"></a>Recherche d'informations relatives à un modèle Naive Bayes  
 Le contenu d'un modèle Naive Bayes fournit des informations d'agrégat sur la distribution de valeurs dans les données d'apprentissage. Vous pouvez également récupérer des informations sur les métadonnées du modèle en créant des requêtes sur les ensembles de lignes de schéma d'exploration de données.  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : obtention des métadonnées du modèle à l'aide de DMX  
 En interrogeant l'ensemble de lignes de schéma d'exploration de données, vous pouvez obtenir les métadonnées du modèle. Celles-ci peuvent inclure la date de création du modèle, celle de son dernier traitement, le nom de la structure d'exploration de données sur laquelle le modèle est basé, ainsi que le nom des colonnes utilisées comme attribut prédictible. Vous pouvez également retourner les paramètres qui ont été utilisés lors de la création du modèle.  
  
```  
SELECT MODEL_CATALOG, MODEL_NAME, DATE_CREATED, LAST_PROCESSED,  
SERVICE_NAME, PREDICTION_ENTITY, FILTER  
FROM $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = 'TM_NaiveBayes_Filtered'  
```  
  
 Exemples de résultats :  
  
|||  
|-|-|  
|MODEL_CATALOG|AdventureWorks|  
|MODEL_NAME|TM_NaiveBayes_Filtered|  
|DATE_CREATED|3/1/2008 19:15|  
|LAST_PROCESSED|3/2/2008 20:00|  
|SERVICE_NAME|Microsoft_Naive_Bayes|  
|PREDICTION_ENTITY|Bike Buyer,Yearly Income|  
|FILTER|[Region] = 'Europe' OR [Region] = 'North America'|  
  
 Le modèle utilisé pour cet exemple est basé sur le modèle Naive Bayes créé dans le [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c), mais il a été modifié en ajoutant un deuxième attribut prédictible et en appliquant un filtre aux données d'apprentissage.  
  
###  <a name="bkmk_Query2"></a> Exemple de requête 2 : extraction d'un résumé des données d'apprentissage  
 Dans un modèle Naive Bayes, le nœud des statistiques marginales stocke des informations d'agrégat sur la distribution de valeurs dans les données d'apprentissage. Ce résumé est pratique et vous évite d'avoir à créer des requêtes SQL sur les données d'apprentissage pour obtenir les mêmes informations.  
  
 L'exemple suivant utilise une requête de contenu DMX pour récupérer les données du nœud (NODE_TYPE = 24). Les statistiques étant stockées dans une table imbriquée, le mot clé FLATTENED est utilisé pour simplifier l'affichage des résultats.  
  
```  
SELECT FLATTENED MODEL_NAME,  
(SELECT ATTRIBUTE_NAME, ATTRIBUTE_VALUE, [SUPPORT], [PROBABILITY], VALUETYPE FROM NODE_DISTRIBUTION) AS t  
FROM TM_NaiveBayes.CONTENT  
WHERE NODE_TYPE = 26  
```  
  
> [!NOTE]  
>  Vous devez placer le nom des colonnes SUPPORT et PROBABILITY entre crochet afin de les distinguer des mots clés réservés MDX (Multidimensional Expressions) qui portent le même nom.  
  
 Résultats partiels :  
  
|MODEL_NAME|t.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|t.SUPPORT|t.PROBABILITY|t.VALUETYPE|  
|-----------------|-----------------------|------------------------|---------------|-------------------|-----------------|  
|TM_NaiveBayes|Bike Buyer|Manquant|0|0|1|  
|TM_NaiveBayes|Bike Buyer|0|8869|0.507263784|4|  
|TM_NaiveBayes|Bike Buyer|1|8615|0.492736216|4|  
|TM_NaiveBayes|Gender|Manquant|0|0|1|  
|TM_NaiveBayes|Gender|F|8656|0.495081217|4|  
|TM_NaiveBayes|Gender|M|8828|0.504918783|4|  
  
 Par exemple, ces résultats vous indiquent le nombre de cas d'apprentissage pour chaque valeur discrète (VALUETYPE = 4), ainsi que la probabilité calculée, ajustée pour les valeurs manquantes (VALUETYPE = 1).  
  
 Pour obtenir la définition des valeurs indiquées dans la table NODE_DISTRIBUTION d’un modèle Naive Bayes, consultez [Contenu du modèle d’exploration de données pour les modèles Naive Bayes &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-naive-bayes-models-analysis-services-data-mining.md). Pour plus d’informations sur la façon dont les valeurs manquantes affectent les calculs de prise en charge et de probabilité, consultez [Valeurs manquantes &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/missing-values-analysis-services-data-mining.md).  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3 : obtention d'informations complémentaires sur les attributs.  
 Un modèle Naive Bayes contenant souvent des informations complexes sur les relations entre les différents attributs, la méthode la plus facile pour consulter ces relations consiste à utiliser la [Visionneuse de l'algorithme MNB (Microsoft Naive Bayes)](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-naive-bayes-viewer.md). Vous pouvez cependant créer des requêtes DMX pour retourner les données.  
  
 L'exemple suivant montre comment retourner des informations à partir du modèle d'un attribut spécifique, `Region`.  
  
```  
SELECT NODE_TYPE, NODE_CAPTION,   
NODE_PROBABILITY, NODE_SUPPORT, MSOLAP_NODE_SCORE  
FROM TM_NaiveBayes.CONTENT  
WHERE ATTRIBUTE_NAME = 'Region'  
```  
  
 Cette requête retourne deux types de nœuds : le nœud qui représente l'attribut d'entrée (NODE_TYPE = 10) et ceux pour chaque valeur de l'attribut (NODE_TYPE = 11). La légende du nœud est utilisée pour l'identifier, plutôt que son nom, car elle affiche à la fois le nom d'attribut et la valeur d'attribut.  
  
|NODE_TYPE|NODE_CAPTION|NODE_PROBABILITY|NODE_SUPPORT|MSOLAP_NODE_SCORE|NODE_TYPE|  
|----------------|-------------------|-----------------------|-------------------|-------------------------|----------------|  
|10|Bike Buyer -> Region|1|17484|84.51555875|10|  
|11|Bike Buyer -> Region = Missing|0|0|0|11|  
|11|Bike Buyer -> Region = North America|0.508236102|8886|0|11|  
|11|Bike Buyer -> Region = Pacific|0.193891558|3390|0|11|  
|11|Bike Buyer -> Region = Europe|0.29787234|5208|0|11|  
  
 Certaines des colonnes stockées dans les nœuds sont identiques à celles obtenues à partir des nœuds des statistiques marginales, telles que le score de probabilité ou les valeurs de support. Toutefois, MSOLAP_NODE_SCORE est une valeur spéciale indiquée uniquement pour les nœuds de l'attribut d'entrée et précise l'importance relative de cet attribut dans le modèle. Vous pouvez consulter pratiquement les mêmes informations dans le volet Réseau de dépendances de la visionneuse, celle-ci n'indiquant cependant pas de score.  
  
 La requête suivante retourne les scores d'importance de tous les attributs du modèle.  
  
```  
SELECT NODE_CAPTION, MSOLAP_NODE_SCORE  
FROM TM_NaiveBayes.CONTENT  
WHERE NODE_TYPE = 10  
ORDER BY MSOLAP_NODE_SCORE DESC  
```  
  
 Exemples de résultats :  
  
|NODE_CAPTION|MSOLAP_NODE_SCORE|  
|-------------------|-------------------------|  
|Bike Buyer -> Total Children|181.3654836|  
|Bike Buyer -> Commute Distance|179.8419482|  
|Bike Buyer -> English Education|156.9841928|  
|Bike Buyer -> Number Children At Home|111.8122599|  
|Bike Buyer -> Region|84.51555875|  
|Bike Buyer -> Marital Status|23.13297354|  
|Bike Buyer -> English Occupation|2.832069191|  
  
 En parcourant le contenu du modèle dans la [Visionneuse de l'arborescence de contenu générique Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-generic-content-tree-viewer.md), vous obtiendrez une meilleure idée des statistiques qui peuvent s'avérer intéressantes. Les exemples présentés dans la présente documentation sont simples, mais la plupart du temps, il peut s'avérer nécessaire d'exécuter plusieurs requêtes ou de stocker les résultats et de les traiter sur le client.  
  
###  <a name="bkmk_Query4"></a> Exemple de requête 4 : utilisation des procédures stockées système  
 Outre l'écriture de vos propres requêtes de contenu, vous pouvez utiliser des procédures stockées système Analysis Services pour explorer les résultats. Pour utiliser une procédure stockée système, préfixez son nom avec le mot clé CALL :  
  
```  
CALL GetPredictableAttributes ('TM_NaiveBayes')  
```  
  
 Résultats partiels :  
  
|ATTRIBUTE_NAME|NODE_UNIQUE_NAME|  
|---------------------|------------------------|  
|Bike Buyer|100000001|  
  
> [!NOTE]  
>  Ces procédures stockées système sont réservées à la communication interne entre le serveur Analysis Services et le client, et doivent uniquement être utilisées pour des raisons pratiques lors du développement et du test des modèles d'exploration de données. Lorsque vous créez des requêtes pour un système de production, vous devez toujours les écrire en utilisant DMX.  
  
 Pour plus d’informations sur les procédures stockées système Analysis Services, consultez [Procédures stockées d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/data-mining-stored-procedures-analysis-services-data-mining.md).  
  
## <a name="using-a-naive-bayes-model-to-make-predictions"></a>Utilisation d'un modèle Naive Bayes pour élaborer des prédictions  
 L'algorithme MNB (Microsoft Naive Bayes) est en général moins utilisé pour la prédiction qu'il ne l'est pour l'exploration des relations entre les attributs d'entrée et prévisibles. Toutefois, le modèle prend en charge l'utilisation de fonctions de prédiction à la fois pour la prédiction et l'association.  
  
###  <a name="bkmk_Query5"></a> Exemple de requête 5 : prédiction des résultats à l'aide d'une requête singleton  
 La requête suivante utilise une requête singleton pour fournir une nouvelle valeur et prédire, selon le modèle, si un client présentant ces caractéristiques est susceptible d'acheter un vélo. La méthode la plus facile pour créer une requête singleton sur un modèle de régression consiste à utiliser la boîte de dialogue **Entrée de requête singleton** . Par exemple, pour créer la requête DMX suivante, sélectionnez le modèle `TM_NaiveBayes` , choisissez **Requête Singleton**, puis sélectionnez des valeurs pour `[Commute Distance]` et `Gender`dans les listes déroulantes.  
  
```  
SELECT  
  Predict([TM_NaiveBayes].[Bike Buyer])  
FROM  
  [TM_NaiveBayes]  
NATURAL PREDICTION JOIN  
(SELECT '5-10 Miles' AS [Commute Distance],  
  'F' AS [Gender]) AS t  
```  
  
 Résultats de l'exemple :  
  
|Expression|  
|----------------|  
|0|  
  
 La fonction de prédiction retourne la valeur la plus probable, dans ce cas 0, qui signifie qu'il est peu probable que ce type de client achète un vélo.  
  
###  <a name="bkmk_Query6"></a> Exemple de requête 6 : obtention de prédictions avec une probabilité et des valeurs de prise en charge  
 Outre la prédiction d'un résultat, vous souhaitez souvent connaître sa pertinence. La requête suivante utilise la même requête singleton que l’exemple précédent, mais ajoute la fonction de prédiction [PredictHistogram &#40;DMX&#41;](../../dmx/predicthistogram-dmx.md) pour retourner une table imbriquée qui contient des statistiques pour la prédiction.  
  
```  
SELECT  
  Predict([TM_NaiveBayes].[Bike Buyer]),  
  PredictHistogram([TM_NaiveBayes].[Bike Buyer])  
FROM  
  [TM_NaiveBayes]  
NATURAL PREDICTION JOIN  
(SELECT '5-10 Miles' AS [Commute Distance],  
  'F' AS [Gender]) AS t  
```  
  
 Résultats de l'exemple :  
  
|Bike Buyer|$SUPPORT|$PROBABILITY|$ADJUSTEDPROBABILITY|$VARIANCE|$STDEV|  
|----------------|--------------|------------------|--------------------------|---------------|------------|  
|0|10161.5714|0.581192599|0.010530981|0|0|  
|1|7321.428768|0.418750215|0.008945684|0|0|  
||0.999828444|5.72E-05|5.72E-05|0|0|  
  
 La ligne finale de la table indique les ajustements apportés aux paramètres de prise en charge et de probabilité pour la valeur manquante. Les valeurs de variance et d'écart type sont toujours 0, car les modèles Naive Bayes ne peuvent pas modéliser de valeurs continues.  
  
###  <a name="bkmk_Query7"></a> Exemple de requête 7 : prédiction d'associations  
 L'algorithme MNB (Microsoft Naive Bayes) permet d'analyser les associations, si la structure d'exploration de données contient une table imbriquée avec l'attribut prédictible comme clé. Par exemple, pour créer un modèle Naive Bayes, utilisez la structure d’exploration de données créée dans [Leçon 3 : Génération d’un scénario de panier d’achat &#40;Didacticiel sur l’exploration de données intermédiaire&#41;](http://msdn.microsoft.com/library/651eef38-772e-4d97-af51-075b1b27fc5a) du didacticiel d’exploration de données. Le modèle utilisé dans cet exemple a été modifié pour ajouter des informations sur le revenu et la région du client dans la table de cas.  
  
 L'exemple de requête suivant présente une requête singleton qui prédit les produits associés aux achats, `'Road Tire Tube'`. Vous pouvez utiliser ces informations pour recommander des produits à un type de client spécifique.  
  
```  
SELECT   PredictAssociation([Association].[v Assoc Seq Line Items])  
FROM [Association_NB]  
NATURAL PREDICTION JOIN  
(SELECT 'High' AS [Income Group],  
  'Europe' AS [Region],  
  (SELECT 'Road Tire Tube' AS [Model])   
AS [v Assoc Seq Line Items])   
AS t  
```  
  
 Résultats partiels :  
  
|Modèle|  
|-----------|  
|Women's Mountain Shorts|  
|Water Bottle|  
|Touring-3000|  
|Touring-2000|  
|Touring-1000|  
  
## <a name="function-list"></a>Liste de fonctions  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Toutefois, l'algorithme MNB ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Naive Bayes) prend en charge les fonctions supplémentaires répertoriées dans le tableau suivant.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[IsDescendant & #40 ; DMX & #41 ;](../../dmx/isdescendant-dmx.md)|Détermine si un nœud est un enfant d'un autre nœud dans le modèle.|  
|[Prédire & #40 ; DMX & #41 ;](../../dmx/predict-dmx.md)|Retourne une valeur ou un ensemble de valeurs prédites pour une colonne spécifiée.|  
|[PredictAdjustedProbability & #40 ; DMX & #41 ;](../../dmx/predictadjustedprobability-dmx.md)|Retourne la probabilité pondérée.|  
|[PredictAssociation & #40 ; DMX & #41 ;](../../dmx/predictassociation-dmx.md)|Prédit l'appartenance à un dataset associatif.|  
|[PredictNodeId & #40 ; DMX & #41 ;](../../dmx/predictnodeid-dmx.md)|Retourne Node_ID pour chaque cas.|  
|[PredictProbability & #40 ; DMX & #41 ;](../../dmx/predictprobability-dmx.md)|Retourne la probabilité pour la valeur prédite.|  
|[PredictSupport & #40 ; DMX & #41 ;](../../dmx/predictsupport-dmx.md)|Retourne la valeur de support pour un état spécifié.|  
  
 Pour connaître la syntaxe de fonctions spécifiques, consultez [Informations de référence sur les fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Référence technique de Microsoft Naive Bayes algorithme](../../analysis-services/data-mining/microsoft-naive-bayes-algorithm-technical-reference.md)   
 [Algorithme Microsoft Naive Bayes](../../analysis-services/data-mining/microsoft-naive-bayes-algorithm.md)   
 [Contenu du modèle d’exploration de données pour les modèles Naive Bayes & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-naive-bayes-models-analysis-services-data-mining.md)  
  
  
