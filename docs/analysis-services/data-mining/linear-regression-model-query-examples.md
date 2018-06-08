---
title: Exemples de requêtes de modèle de régression linéaire | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e967bcb024a20c09105447780e5d672d4dc843fa
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="linear-regression-model-query-examples"></a>Exemples de requête de modèle de régression linéaire
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Lorsque vous créez une requête sur un modèle d'exploration de données, vous pouvez créer une requête de contenu, qui fournit des détails sur les modèles (ou séquences) découverts au cours de l'analyse, ou créer une requête de prédiction, qui utilise les séquences du modèle pour effectuer des prédictions pour les nouvelles données. Par exemple, une requête de contenu peut fournir des détails supplémentaires sur la formule de régression, tandis qu'une requête de prédiction peut vous indiquer si un nouveau point de données est adapté au modèle. Vous pouvez également extraire les métadonnées relatives au modèle en utilisant une requête.  
  
 Cette section explique comment créer ces requêtes pour les modèles basés sur l'algorithme MLR (Microsoft Linear Regression).  
  
> [!NOTE]  
>  La régression linéaire étant basée sur un cas spécial de l'algorithme MDT (Microsoft Decision Trees), les similarités sont nombreuses, et certains modèles d'arbre de décision qui utilisent des attributs prédictibles continus peuvent contenir des formules de régression. Pour plus d’informations, consultez [Références techniques relatives à l’algorithme MDT (Microsoft Decision Trees)](../../analysis-services/data-mining/microsoft-decision-trees-algorithm-technical-reference.md).  
  
 **Requêtes de contenu**  
  
 [Utilisation de l'ensemble de lignes de schéma d'exploration de données pour déterminer les paramètres utilisés pour un modèle](#bkmk_Query1)  
  
 [Utilisation de DMX pour retourner la formule de régression du modèle](#bkmk_Query2)  
  
 [Retour uniquement du coefficient du modèle](#bkmk_Query3)  
  
 **Requêtes de prédiction**  
  
 [Prédiction du revenu à l'aide d'une requête singleton](#bkmk_Query4)  
  
 [Utilisation des fonctions de prédiction avec un modèle de régression](#bkmk_Query5)  
  
##  <a name="bkmk_top"></a> Recherche d'informations sur le modèle de régression linéaire  
 La structure d'un modèle de régression linéaire est extrêmement simple : le modèle d'exploration de données représente les données sous la forme d'un nœud unique qui définit la formule de régression. Pour plus d’informations, consultez [Contenu du modèle d’exploration de données pour les modèles de régression logistique &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-logistic-regression-models.md).  
  
 [Retour en haut](#bkmk_top)  
  
###  <a name="bkmk_Query1"></a> Exemple de requête 1 : utilisation de l'ensemble de lignes de schéma d'exploration de données pour déterminer les paramètres utilisés pour un modèle  
 En interrogeant l'ensemble de lignes de schéma d'exploration de données, vous pouvez obtenir les métadonnées relatives au modèle. Celles-ci peuvent inclure la date de création du modèle, celle de son dernier traitement, le nom de la structure d'exploration de données sur laquelle le modèle est basé, ainsi que le nom de la colonne désignée comme attribut prédictible. Vous pouvez également retourner les paramètres qui ont été utilisés lorsque le modèle a été créé pour la première fois.  
  
```  
SELECT MINING_PARAMETERS   
FROM $system.DMSCHEMA_MINING_MODELS  
WHERE MODEL_NAME = 'TM_PredictIncome'  
```  
  
 Exemples de résultats :  
  
|MINING_PARAMETERS|  
|------------------------|  
|COMPLEXITY_PENALTY=0.9,<br /><br /> MAXIMUM_INPUT_ATTRIBUTES=255,<br /><br /> MAXIMUM_OUTPUT_ATTRIBUTES=255,<br /><br /> MINIMUM_SUPPORT=10,<br /><br /> SCORE_METHOD=4,<br /><br /> SPLIT_METHOD=3,<br /><br /> FORCE_REGRESSOR=|  
  
> [!NOTE]  
>  Le paramètre`FORCE_REGRESSOR =` indique que la valeur actuelle du paramètre FORCE_REGRESSOR est Null.  
  
 [Retour en haut](#bkmk_top)  
  
###  <a name="bkmk_Query2"></a> Exemple de requête 2 : récupération de la formule de régression du modèle  
 La requête suivante retourne le contenu du modèle d'exploration de données pour un modèle de régression linéaire qui a été construit en utilisant la même source de données de publipostage ciblé que celle utilisée dans le [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). Ce modèle prédit le revenu des clients en fonction de leur âge.  
  
 La requête retourne le contenu du nœud qui contient la formule de régression. Chaque variable et chaque coefficient est stocké dans une ligne distincte de la table imbriquée NODE_DISTRIBUTION. Pour consulter la formule de régression complète, dans la [Visionneuse d’arborescences Microsoft](../../analysis-services/data-mining/browse-a-model-using-the-microsoft-tree-viewer.md), cliquez sur le nœud **(Tout)** , puis ouvrez **Légende d’exploration de données**.  
  
```  
SELECT FLATTENED NODE_DISTRIBUTION as t  
FROM LR_PredictIncome.CONTENT  
```  
  
> [!NOTE]  
>  Si vous faites référence à des colonnes individuelles de la table imbriquée en utilisant une requête comme `SELECT <column name> from NODE_DISTRIBUTION`, certaines colonnes, comme **SUPPORT** ou **PROBABILITY**, doivent être placées entre crochets afin de les distinguer des mots clés réservés qui portent le même nom.  
  
 Résultats attendus :  
  
|t.ATTRIBUTE_NAME|t.ATTRIBUTE_VALUE|t.SUPPORT|t.PROBABILITY|t.VARIANCE|t.VALUETYPE|  
|-----------------------|------------------------|---------------|-------------------|----------------|-----------------|  
|Yearly Income|Manquant|0|0.000457142857142857|0|1|  
|Yearly Income|57220.8876687257|17484|0.999542857142857|1041275619.52776|3|  
|Age|471.687717702463|0|0|126.969442359327|7|  
|Age|234.680904692439|0|0|0|8|  
|Age|45.4269617936399|0|0|126.969442359327|9|  
||35793.5477381267|0|0|1012968919.28372|11|  
  
 En comparaison, dans **Légende d’exploration de données**, la formule de régression apparaît comme suit :  
  
 `Yearly Income = 57,220.919 + 471.688 * (Age - 45.427)`  
  
 Vous pouvez constater que dans **Légende d’exploration de données**, certains nombres sont arrondis ; toutefois, la table NODE_DISTRIBUTION et **Légende d’exploration de données** contiennent essentiellement les mêmes valeurs.  
  
 Les valeurs indiquées dans la colonne VALUETYPE précisent le type des informations contenues dans chaque ligne, ce qui est utile si vous traitez les résultats par programme. Le tableau suivant affiche les types de valeur qui sont générés pour une formule de régression linéaire.  
  
|VALUETYPE|  
|---------------|  
|1 (Manquante)|  
|3 (Continue)|  
|7 (Coefficient)|  
|8 (Gain du score)|  
|9 (Statistiques)|  
|7 (Coefficient)|  
|8 (Gain du score)|  
|9 (Statistiques)|  
|11 (Ordonnée à l'origine)|  
  
 Pour plus d’informations sur la signification de chaque type de valeur pour les modèles de régression, consultez [Contenu du modèle d’exploration de données pour les modèles de régression linéaire &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md).  
  
 [Retour en haut](#bkmk_top)  
  
###  <a name="bkmk_Query3"></a> Exemple de requête 3 : retour uniquement du coefficient du modèle  
 En utilisant l'énumération VALUETYPE, vous pouvez retourner uniquement le coefficient de l'équation de régression, tel qu'indiqué dans la requête suivante :  
  
```  
SELECT FLATTENED MODEL_NAME,  
    (SELECT ATTRIBUTE_VALUE, VALUETYPE  
     FROM NODE_DISTRIBUTION  
     WHERE VALUETYPE = 11)   
AS t  
FROM LR_PredictIncome.CONTENT  
```  
  
 Cette requête retourne deux lignes : une provenant du contenu du modèle d'exploration de données et celle provenant de la table imbriquée qui contient le coefficient. La colonne ATTRIBUTE_NAME n'est pas incluse ici parce qu'elle est toujours vide pour le coefficient.  
  
|MODEL_NAME|t.ATTRIBUTE_VALUE|t.VALUETYPE|  
|-----------------|------------------------|-----------------|  
|LR_PredictIncome|||  
|LR_PredictIncome|35793.5477381267|11|  
  
 [Retour en haut](#bkmk_top)  
  
## <a name="making-predictions-from-a-linear-regression-model"></a>Élaborer des prédictions à partir d'un modèle de régression linéaire  
 Vous pouvez créer des requêtes de prédiction sur des modèles de régression linéaire en utilisant l'onglet Prévision de modèle d'exploration de données du Concepteur d'exploration de données. Le générateur de requêtes de prédiction est disponible à la fois dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
> [!NOTE]  
>  Vous pouvez également créer des requêtes sur les modèles de régression en utilisant les compléments d’exploration de données pour Excel [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou les compléments d’exploration de données pour Excel [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] . Même si les compléments d'exploration de données pour Excel ne créent pas de modèles de régression, vous pouvez parcourir et interroger un modèle d'exploration de données stocké sur une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 [Retour en haut](#bkmk_top)  
  
###  <a name="bkmk_Query4"></a> Exemple de requête 4 : prédiction du revenu à l'aide d'une requête singleton  
 La méthode la plus facile pour créer une requête singleton sur un modèle de régression consiste à utiliser la boîte de dialogue **Entrée de requête singleton** . Par exemple, pour créer la requête DMX suivante, sélectionnez le modèle de régression approprié, choisissez **Requête singleton**, puis tapez **20** comme valeur pour **Age**.  
  
```  
SELECT [LR_PredictIncome].[Yearly Income]  
From   [LR_PredictIncome]  
NATURAL PREDICTION JOIN  
(SELECT 20 AS [Age]) AS t  
```  
  
 Exemples de résultats :  
  
|Yearly Income|  
|-------------------|  
|45227.302092176|  
  
 [Retour en haut](#bkmk_top)  
  
###  <a name="bkmk_Query5"></a> Exemple de requête 5 : utilisation des fonctions de prédiction avec un modèle de régression  
 Vous pouvez utiliser de nombreuses fonctions de prédiction standard avec les modèles de régression linéaire. L'exemple suivant montre comment ajouter des statistiques descriptives aux résultats de requête de prédiction. D'après ces résultats, vous pouvez constater que l'écart de la moyenne est très important pour ce modèle.  
  
```  
SELECT  
  ([LR_PredictIncome].[Yearly Income]) as [PredIncome],  
  (PredictStdev([LR_PredictIncome].[Yearly Income])) as [StDev1]  
From  
  [LR_PredictIncome]  
NATURAL PREDICTION JOIN  
(SELECT 20 AS [Age]) AS t  
```  
  
 Exemples de résultats :  
  
|Yearly Income|StDev1|  
|-------------------|------------|  
|45227.302092176|31827.1726561396|  
  
 [Retour en haut](#bkmk_top)  
  
## <a name="list-of-prediction-functions"></a>Liste des fonctions de prédiction  
 Tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)] prennent en charge un ensemble commun de fonctions. Toutefois, l'algorithme MLR ( [!INCLUDE[msCoName](../../includes/msconame-md.md)] Linear Regression) prend en charge les fonctions supplémentaires répertoriées dans le tableau suivant.  
  
|||  
|-|-|  
|Fonction de prédiction|Utilisation|  
|[IsDescendant & #40 ; DMX & #41 ;](../../dmx/isdescendant-dmx.md)|Détermine si un nœud est un enfant d'un autre nœud dans le modèle.|  
|[IsInNode & #40 ; DMX & #41 ;](../../dmx/isinnode-dmx.md)|Indique si le nœud spécifié contient le cas courant.|  
|[PredictHistogram & #40 ; DMX & #41 ;](../../dmx/predicthistogram-dmx.md)|Retourne une valeur ou un ensemble de valeurs prédites pour une colonne spécifiée.|  
|[PredictNodeId & #40 ; DMX & #41 ;](../../dmx/predictnodeid-dmx.md)|Retourne Node_ID pour chaque cas.|  
|[PredictStdev & #40 ; DMX & #41 ;](../../dmx/predictstdev-dmx.md)|Retourne l'écart-type pour la valeur prédite.|  
|[PredictSupport & #40 ; DMX & #41 ;](../../dmx/predictsupport-dmx.md)|Retourne la valeur de support pour un état spécifié.|  
|[PredictVariance & #40 ; DMX & #41 ;](../../dmx/predictvariance-dmx.md)|Retourne la variance d'une colonne spécifiée.|  
  
 Pour obtenir la liste des fonctions qui sont communes à tous les algorithmes [!INCLUDE[msCoName](../../includes/msconame-md.md)], consultez [Algorithmes d’exploration de données &#40;Analysis Services – Exploration de données&#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md). Pour plus d’informations sur l’utilisation de ces fonctions, consultez [Fonctions DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-function-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithme de régression linéaire Microsoft](../../analysis-services/data-mining/microsoft-linear-regression-algorithm.md)   
 [Requêtes d’exploration de données](../../analysis-services/data-mining/data-mining-queries.md)   
 [Référence technique de Microsoft Linear Regression algorithme](../../analysis-services/data-mining/microsoft-linear-regression-algorithm-technical-reference.md)   
 [Contenu du modèle d’exploration de données pour les modèles de régression linéaire & #40 ; Analysis Services - Exploration de données & #41 ;](../../analysis-services/data-mining/mining-model-content-for-linear-regression-models-analysis-services-data-mining.md)  
  
  
