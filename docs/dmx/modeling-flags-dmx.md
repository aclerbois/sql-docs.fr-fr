---
title: Indicateurs (DMX) de modélisation | Documents Microsoft
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
dev_langs:
- DMX
helpviewer_keywords:
- REGRESSOR flag
- DMX [Analysis Services], modeling flags
- MODEL_EXISTENCE_ONLY flag
- modeling flags [DMX]
- Data Mining Extensions [Analysis Services], modeling flags
- flags [DMX]
- NOT NULL flag
ms.assetid: 498d25f7-9597-47ae-8717-61ddd1d2fd15
caps.latest.revision: 35
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: cb12be7ce222260cb74384ffe937acde9225af7b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modeling-flags-dmx"></a>Indicateurs de modélisation (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Vous pouvez utiliser des indicateurs de modélisation dans [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] pour fournir à un algorithme d'exploration de données des informations supplémentaires portant sur les données définies dans une table de cas. Ces informations permettent à l'algorithme de construire un modèle d'exploration de données plus précis. Les indicateurs de modélisation peuvent se définir sur des colonnes de structure d'exploration de données comme sur des colonnes de modèle d'exploration de données.  
  
 [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] prend en charge les indicateurs de modélisation suivants :  
  
 **NON NULL**  
 Les valeurs de la colonne d'attribut ne doivent jamais contenir de valeur NULL. Une erreur est générée si [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] rencontre une valeur NULL pour la colonne d’attribut au cours du processus d’apprentissage du modèle. Cet indicateur est défini sur une colonne de structure d'exploration de données.  
  
 **RÉGRESSEUR**  
 Indique que l'algorithme peut utiliser la colonne spécifiée dans la formule de régression des algorithmes de régression. Cet indicateur est pris en charge par les algorithmes [!INCLUDE[msCoName](../includes/msconame-md.md)] Linear Regression et [!INCLUDE[msCoName](../includes/msconame-md.md)] Decision Trees. Il est défini sur une colonne de modèle d'exploration de données.  
  
 **MODEL_EXISTENCE_ONLY**  
 Les valeurs de la colonne d'attribut sont moins importantes que la présence de l'attribut. Cet indicateur est défini sur une colonne de modèle d'exploration de données.  
  
 Des algorithmes tiers peuvent prendre en charge des indicateurs de modélisation supplémentaires. Pour déterminer les indicateurs de modélisation un algorithme prend en charge, utilisez le **SUPPORTED_MODELING_FLAGS** de lignes du schéma. Vous pouvez également interroger les services d'exploration de données sur le serveur afin de déterminer les indicateurs de modélisation pris en charge pour un algorithme spécifique. Par exemple, la requête suivante retourne les indicateurs de modélisation pris en charge pour l'algorithme MLR (Microsoft Linear Regression) sur le serveur actuel :  
  
```  
SELECT SUPPORTED_MODELING_FLAGS  
FROM $SYSTEM.DMSCHEMA_MINING_SERVICES  
WHERE SERVICE_NAME = 'Microsoft_Linear_Regression'  
```  
  
 Résultats attendus :  
  
 NOT NULL,REGRESSOR  
  
## <a name="specifying-modeling-flags-on-a-mining-model"></a>Spécification d'indicateurs de modélisation sur un modèle d'exploration de données  
 Pour obtenir des exemples de syntaxe qui [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] prend en charge pour la spécification d’un indicateur sur une colonne de structure d’exploration de données, consultez [CREATE MINING STRUCTURE &#40;DMX&#41;](../dmx/create-mining-structure-dmx.md).  
  
 Pour obtenir un exemple de syntaxe pour la spécification d’un indicateur de modélisation sur une colonne de modèle d’exploration de données, consultez [ALTER MINING STRUCTURE &#40;DMX&#41;](../dmx/alter-mining-structure-dmx.md).  
  
 Pour plus d’informations sur l’utilisation des colonnes du modèle d’exploration de données, consultez [colonnes du modèle d’exploration de données](../analysis-services/data-mining/mining-model-columns.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Algorithmes d’exploration de données &#40; Analysis Services - Exploration de données &#41;](../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; éléments de syntaxe](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence des opérateurs](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Structure et utilisation des requêtes de prédiction DMX](../dmx/structure-and-usage-of-dmx-prediction-queries.md)   
 [Présentation de l’instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
