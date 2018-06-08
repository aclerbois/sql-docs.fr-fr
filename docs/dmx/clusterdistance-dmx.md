---
title: ClusterDistance (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ClusterDistance
dev_langs:
- DMX
helpviewer_keywords:
- ClusterDistance function
ms.assetid: a13152b3-4cd1-4c79-8a3e-207624198330
caps.latest.revision: 11
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 03e85862a5fc8a1a9daae56282294d9addad53f0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="clusterdistance-dmx"></a>ClusterDistance (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Le **ClusterDistance** fonction retourne la distance séparant le cas d’entrée du cluster spécifié ou si aucun cluster n’est spécifié, la distance séparant le cas d’entrée du cluster plus probable.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
ClusterDistance([<ClusterID expression>])  
```  
  
## <a name="applies-to"></a>S'applique à  
 Cette fonction ne peut être utilisée que si le modèle d'exploration de données sous-jacent prend en charge le clustering. La fonction peut être utilisée avec n'importe quel type de modèle de clustering (EM, K-Means, etc.), mais les résultats diffèrent selon l'algorithme.  
  
## <a name="return-type"></a>Type de retour  
 Une valeur scalaire.  
  
## <a name="remarks"></a>Notes  
 Le **ClusterDistance** fonction retourne la distance entre le cas d’entrée et le cluster ayant la probabilité la plus élevée pour qu’une entrée de cas.  
  
 En cas de clustering K-Means, puisque les cas ne peuvent appartenir qu'à un seul cluster, avec un poids d'appartenance de 1.0, la distance de cluster est toujours 0. Toutefois, dans le cas de K-Means, chaque cluster est supposé avoir un centroïde. Vous pouvez obtenir la valeur du centroïde en interrogeant ou en parcourant la table imbriquée NODE_DISTRIBUTION dans le contenu du modèle d'exploration de données. Pour plus d’informations, consultez [Mining Model Content for Clustering Models &#40;Analysis Services - Data Mining&#41;](../analysis-services/data-mining/mining-model-content-for-clustering-models-analysis-services-data-mining.md).  
  
 Dans le cas de la méthode de clustering EM, tous les points à l'intérieur du cluster sont considérés comme ayant la même probabilité ; il n'ya donc pas, par définition, de centroïde pour le cluster par conception. La valeur de **ClusterDistance** entre un cas particulier et un cluster particulier *N* est calculé comme suit :  
  
 ClusterDistance(N) =1–(membershipWeight(N))  
  
 Ou:  
  
 ClusterDistance(N) = 1 – ClusterProbability (N))  
  
## <a name="related-prediction-functions"></a>Fonctions de prédiction connexes  
 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] fournit les fonctions supplémentaires suivantes permettant d'interroger des modèles de clustering :  
  
-   Utilisez le [Cluster &#40;DMX&#41; ](../dmx/cluster-dmx.md) fonction pour retourner le cluster le plus probable.  
  
-   Utilisez le [ClusterProbability &#40;DMX&#41; ](../dmx/clusterprobability-dmx.md) fonction permettant d’obtenir la probabilité qu’un cas appartienne à un cluster particulier. Cette valeur est l'inverse de la distance de cluster.  
  
-   Utilisez le [PredictHistogram &#40;DMX&#41; ](../dmx/predicthistogram-dmx.md) fonction pour retourner un histogramme de la probabilité du cas d’entrée dans chacun des clusters du modèle.  
  
-   Utilisez le [PredictCaseLikelihood &#40;DMX&#41; ](../dmx/predictcaselikelihood-dmx.md) fonction pour retourner une mesure comprise entre 0 et 1 qui indique la probabilité un cas d’entrée existe, compte tenu du modèle appris par l’algorithme.  
  
## <a name="example1-obtaining-cluster-distance-to-the-most-likely-cluster"></a>Exemple 1 : obtention de la distance de cluster au cluster le plus probable  
 L'exemple suivant retourne la distance séparant le cas spécifié du cluster auquel il est le plus susceptible d'appartenir.  
  
```  
SELECT  
    ClusterDistance()  
FROM  
    [TM Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
    '2-5 Miles' AS [Commute Distance],  
    'Graduate Degree' AS [Education],  
    0 AS [Number Cars Owned],  
    0 AS [Number Children At Home]) AS t  
```  
  
 Résultats de l'exemple :  
  
|Expression|  
|----------------|  
|0.0477390930705145|  
  
 Pour déterminer de quel cluster il s'agit, vous pouvez substituer `Cluster` à `ClusterDistance` dans l'exemple précédent.  
  
 Résultats de l'exemple :  
  
|$CLUSTER|  
|--------------|  
|Cluster 6|  
  
## <a name="example2-obtaining-distance-to-a-specified-cluster"></a>Exemple 2 : obtention de la distance à un cluster spécifié  
 La syntaxe suivante utilise l'ensemble de lignes du schéma Content du modèle d'exploration de données pour retourner la liste des ID et légendes de nœud des clusters du modèle. Vous pouvez ensuite utiliser la légende du nœud en tant qu’argument d’identificateur de cluster dans le **ClusterDistance** (fonction).  
  
```  
SELECT NODE_UNIQUE_NAME, NODE_CAPTION   
FROM <model>.CONTENT   
WHERE NODE_TYPE = 5  
```  
  
 Résultats de l'exemple :  
  
|NODE_UNIQUE_NAME|NODE_CAPTION|  
|------------------------|-------------------|  
|001|Cluster 1|  
|002|Cluster 2|  
  
 L'exemple de syntaxe suivant retourne la distance séparant le cas spécifié du cluster appelé Cluster 2.  
  
```  
SELECT  
    ClusterDistance('Cluster 2')  
AS [Cluster 2 Distance]  
FROM [TM Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
    '2-5 Miles' AS [Commute Distance],  
    'Graduate Degree' AS [Education],  
    0 AS [Number Cars Owned],  
    0 AS [Number Children At Home]) AS t  
```  
  
 Résultats de l'exemple :  
  
|Cluster 2 Distance|  
|------------------------|  
|0.97008209236394|  
  
## <a name="see-also"></a>Voir aussi  
 [Cluster &#40;DMX&#41;](../dmx/cluster-dmx.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;DMX&#41;](../dmx/functions-dmx.md)   
 [Contenu du modèle d’exploration de données pour le Clustering des modèles &#40; Analysis Services - Exploration de données &#41;](../analysis-services/data-mining/mining-model-content-for-clustering-models-analysis-services-data-mining.md)  
  
  
