---
title: Structure et utilisation des requêtes de prédiction DMX | Documents Microsoft
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
- prediction joins [DMX]
- empty prediction joins [DMX]
- natural prediction joins [DMX]
- DMX [Analysis Services], prediction queries
- prediction queries [DMX]
- queries [DMX], prediction queries
- singleton query predictions [DMX]
- Data Mining Extensions [Analysis Services], prediction queries
ms.assetid: 098bdaa6-9e7d-4e13-a9aa-eb17ce1750e6
caps.latest.revision: 37
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 6f4e5d0f723851776340d3435c05e382c88552ae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="structure-and-usage-of-dmx-prediction-queries"></a>Structure et utilisation des requêtes de prédiction DMX
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Dans [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)], vous pouvez utiliser la requête de prédiction dans les Extensions DMX (Data Mining) pour prédire des valeurs de colonne inconnues dans un nouveau jeu de données, selon les résultats d’un modèle d’exploration de données.  
  
 Le type de requête utilisé dépend des informations que vous souhaitez obtenir d'un modèle. Si vous voulez créer des prédictions simples en temps réel, par exemple pour savoir si un éventuel client sur un site Web correspond au personnage de l'acheteur de bicyclette, vous utilisez une requête singleton. En revanche, si vous souhaitez créer un lot de prédictions à partir d'un ensemble de cas figurant dans une source de données, vous devez utiliser une requête de prédiction standard.  
  
## <a name="prediction-types"></a>Types de prédictions  
 Dans DMX, vous pouvez créer les types de prédictions suivants :  
  
 Jointure de prédiction  
 Ce type de prédiction permet de créer des prédictions sur des données d'entrée sur la base des schémas qui figurent dans le modèle d'exploration de données. Cette instruction de requête doit être suivie par une **ON** clause qui fournit les conditions de jointure entre les colonnes du modèle d’exploration de données et les colonnes d’entrée.  
  
 Jointure de prédiction naturelle  
 Ce type de prédiction permet de créer des prédictions basées sur les noms de colonnes du modèle d'exploration de données qui correspondent exactement aux noms de colonnes de la table sur laquelle vous effectuez la requête. Cette instruction de requête ne nécessite pas une **ON** clause, car la condition de jointure est automatiquement générée en fonction de la correspondance des noms entre les colonnes du modèle d’exploration de données et les colonnes d’entrée.  
  
 Jointure de prédiction vide  
 Cette requête permet de découvrir la prédiction la plus probable, sans avoir à fournir de données d'entrée. Elle retourne une prédiction basée uniquement sur le contenu du modèle d'exploration de données.  
  
 Requête singleton  
 Ce type de prédiction permet de créer une prédiction en alimentant les données à la requête. Cette instruction est utile car elle permet de soumettre un seul cas à la requête et d'obtenir les résultats rapidement. Par exemple, vous pouvez utiliser la requête pour prévoir si une personne, du sexe féminin, âgée de 35 ans et mariée, serait susceptible d'acheter une bicyclette. Cette requête ne nécessite pas de source de données externe.  
  
## <a name="query-structure"></a>Structure de la requête  
 Pour construire une requête de prédiction en DMX, vous devez utiliser une combinaison des éléments suivants :  
  
-   **SÉLECTIONNEZ [APLATIES]**  
  
-   **TOP**  
  
-   **À partir de***\<modèle >***PREDICTION JOIN**   
  
-   **ON**  
  
-   **WHERE**  
  
-   **ORDER BY**  
  
 Le **sélectionnez** élément d’une requête de prédiction définit les colonnes et les expressions qui apparaîtront dans le résultat et peuvent inclure les données suivantes :  
  
-   **Prédire** ou **PredictOnly** les colonnes du modèle d’exploration de données.  
  
-   Toute colonne des données d'entrée utilisée pour créer les prédictions.  
  
-   Des fonctions retournant une colonne de données.  
  
 Le **FROM**  *\<modèle >* **PREDICTION JOIN** élément définit les données sources à utiliser pour créer la prédiction. Pour une requête singleton, c'est une série de valeurs affectées aux colonnes. Pour une jointure de prédiction vide, l'élément est vide.  
  
 Le **ON** élément mappe les colonnes qui sont définies dans le modèle d’exploration de données à des colonnes dans un jeu de données externe. Il est inutile d'inclure cet élément si vous créez une requête de jointure de prédiction vide ou une jointure de prédiction naturelle.  
  
 Vous pouvez utiliser la **où** clause pour filtrer les résultats d’une requête de prédiction. Vous pouvez utiliser un **haut** ou **ORDER BY** clause pour sélectionner les prédictions les plus probables. Pour plus d’informations sur l’utilisation de ces clauses, consultez [sélectionnez &#40;DMX&#41;](../dmx/select-dmx.md).  
  
 Pour plus d’informations sur la syntaxe d’une instruction de prédiction, consultez [SELECT FROM &#60;modèle&#62; PREDICTION JOIN &#40;DMX&#41; ](../dmx/select-from-model-prediction-join-dmx.md) et [SELECT FROM &#60;modèle&#62; &#40;DMX &#41;](../dmx/select-from-model-dmx.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence](../dmx/data-mining-extensions-dmx-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Data Mining Extensions &#40;DMX&#41; référence des opérateurs](../dmx/data-mining-extensions-dmx-operator-reference.md)   
 [Les Extensions d’exploration de données & #40 ; DMX & #41 ; Référence des instructions](../dmx/data-mining-extensions-dmx-statements.md)   
 [Data Mining Extensions &#40;DMX&#41; Conventions de syntaxe](../dmx/data-mining-extensions-dmx-syntax-conventions.md)   
 [Data Mining Extensions &#40;DMX&#41; éléments de syntaxe](../dmx/data-mining-extensions-dmx-syntax-elements.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)   
 [Présentation de l’instruction DMX Select](../dmx/understanding-the-dmx-select-statement.md)  
  
  
