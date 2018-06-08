---
title: PredictCaseLikelihood (DMX) | Documents Microsoft
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
- PredictCaseLikelihood
dev_langs:
- DMX
helpviewer_keywords:
- PredictCaseLikelihood function
ms.assetid: b00180e5-b2eb-49e2-891d-e39fb378f50a
caps.latest.revision: 39
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 3d9cbe8c92be301fa56c2d5a9f78e741ef5765f9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="predictcaselikelihood-dmx"></a>PredictCaseLikelihood (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Cette fonction retourne la probabilité qu'un cas d'entrée corresponde au modèle existant. Uniquement utilisée avec les modèles de clustering.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PredictCaseLikelihood([NORMALIZED|NONNORMALIZED])  
```  
  
## <a name="arguments"></a>Arguments  
 NORMALIZED  
 La valeur de retour contient la probabilité du cas au sein du modèle divisée par la probabilité du cas sans le modèle.  
  
 NONNORMALIZED  
 La valeur de retour contient la probabilité brute du cas, qui est le produit des probabilités des attributs de cas.  
  
## <a name="applies-to"></a>S'applique à  
 Modèles qui sont générés à l’aide de la [!INCLUDE[msCoName](../includes/msconame-md.md)] Clustering et [!INCLUDE[msCoName](../includes/msconame-md.md)] des algorithmes Sequence Clustering.  
  
## <a name="return-type"></a>Type de retour  
 Nombre à virgule flottante double précision compris entre 0 et 1. Un nombre plus proche de 1 indique que le cas a une probabilité plus élevée de se produire dans ce modèle. Un nombre plus proche de 0 indique qu'il est moins probable que le cas se produise dans ce modèle.  
  
## <a name="remarks"></a>Notes  
 Par défaut, le résultat de la **PredictCaseLikelihood** fonction est normalisée. Les valeurs normalisées sont généralement plus utiles lorsque le nombre d'attributs d'un cas augmente et que les différences entre les probabilités brutes de deux cas deviennent beaucoup moins importantes.  
  
 L'équation suivante est utilisée pour calculer les valeurs normalisées pour des valeurs x et y données :  
  
-   x = probabilité du cas en fonction du modèle de clustering  
  
-   y = probabilité de cas marginale, calculée comme le logarithme du rapport de vraisemblance du cas en fonction des cas d'apprentissage  
  
-   Z = Exp( log(x) – Log(Y))  
  
 Normalisé = (z / (1 + z))  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne la probabilité que le cas spécifié se produira dans le modèle de clustering, qui est basé sur la base de données [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] DW.  
  
```  
SELECT  
  PredictCaseLikelihood() AS Default_Likelihood,  
  PredictCaseLikelihood(NORMALIZED) AS Normalized_Likelihood,  
  PredictCaseLikelihood(NONNORMALIZED) AS Raw_Likelihood,  
FROM  
  [TM Clustering]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
  '2-5 Miles' AS [Commute Distance],  
  'Graduate Degree' AS [Education],  
  0 AS [Number Cars Owned],  
  0 AS [Number Children At Home]) AS t  
```  
  
 Résultats attendus :  
  
|Default_Likelihood|Normalized_Likelihood|Raw_Likelihood|  
|-------------------------|----------------------------|---------------------|  
|6.30672792729321E-08|6.30672792729321E-08|9.5824454056846E-48|  
  
 La différence entre ces résultats montre l'effet de la normalisation. La valeur brute de **CaseLikelihood** suggère que la probabilité du cas est d’environ 20 pour cent ; Toutefois, lorsque vous normalisez les résultats, il devient évidente que la probabilité du cas est très faible.  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;DMX&#41;](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  
