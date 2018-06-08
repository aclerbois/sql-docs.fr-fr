---
title: PredictAssociation (DMX) | Documents Microsoft
ms.custom: ''
ms.date: 09/14/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: data-mining
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- PredictAssociation
dev_langs:
- DMX
helpviewer_keywords:
- PredictAssociation function
ms.assetid: 33eb66b5-84c6-449f-aaae-316345bc4ad5
caps.latest.revision: 33
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: e1d65c529dd268560b34d25a1cb767aefe4575db
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="predictassociation-dmx"></a>PredictAssociation (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Prévoit une appartenance associative.  
  
Par exemple, vous pouvez utiliser la fonction PredictAssociation pour obtenir l’ensemble des recommandations, étant donné l’état actuel du panier d’achat d’un client. 
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
PredictAssociation(<table column reference>, option1, option2, n ...)  
```  
  
## <a name="applies-to"></a>S'applique à  
 Algorithmes qui contiennent des tables imbriquées prévisibles, y compris l’association et certains algorithmes de classification. Les algorithmes de classification qui prennent en charge des tables imbriquées sont le [!INCLUDE[msCoName](../includes/msconame-md.md)] arbres de décision, [!INCLUDE[msCoName](../includes/msconame-md.md)] Naive Bayes, et [!INCLUDE[msCoName](../includes/msconame-md.md)] algorithmes de réseau neuronal.  
  
## <a name="return-type"></a>Type de retour  
 \<Expression de table >  
  
## <a name="remarks"></a>Notes  
 Les options pour le **PredictAssociation** fonction incluent EXCLUDE_NULL, INCLUDE_NULL, INCLUSIVE, EXCLUSIVE (par défaut), INPUT_ONLY, INCLUDE_STATISTICS et INCLUDE_NODE_ID.  
  
> [!NOTE]  
>  INCLUSIVE, EXCLUSIVE, INPUT_ONLY et INCLUDE_STATISTICS s'appliquent uniquement à une référence de colonne de table, et EXCLUDE_NULL et INCLUDE_NULL s'appliquent uniquement à une référence de colonne scalaire.  
  
 INCLUDE_STATISTICS retourne uniquement **$Probability** et **$AdjustedProbability**.  
  
 Si le paramètre numérique *n* est spécifié, le **PredictAssociation** fonction retourne les n premières valeurs probablement selon la probabilité :  
  
```  
PredictAssociation(colref, [$AdjustedProbability], n)  
```  
  
 Si vous incluez **$AdjustedProbability**, l’instruction retourne la partie supérieure *n* valeurs basées sur les **$AdjustedProbability**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant utilise le **PredictAssociation** fonction pour retourner les quatre produits de la société Adventure Works de base de données qui sont susceptibles d’être vendus ensemble.  
  
```  
SELECT  
  PredictAssociation([Association].[v Assoc Seq Line Items],4)  
From  
  [Association]  
```  
L’exemple suivant montre comment vous pouvez utiliser une table imbriquée comme entrée pour la fonction de prédiction, à l’aide de la clause SHAPE. La requête SHAPE crée un ensemble de lignes avec customerId comme une seule colonne et une table imbriquée en tant que deuxième colonne, qui contient la liste des produits de qu'un client a déjà. 

~~~~
SELECT T.[CustomerId], PredictAssociation(MyNestedTable, 5) // returns top 5 associated items
FROM My Model
PREDICTION JOIN
SHAPE {
    OPENQUERY([Adventure Works DW],'SELECT CustomerID, OrderNumber
    FROM vAssocSeqOrders ORDER BY OrderNumber')
} APPEND (
    {OPENQUERY([Adventure Works DW],'SELECT OrderNumber, model FROM 
    dbo.vAssocSeqLineItems ORDER BY OrderNumber, Model')}
  RELATE OrderNumber to OrderNumber) AS T
~~~~  

  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;DMX&#41;](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  
