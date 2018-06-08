---
title: Lag (DMX) | Documents Microsoft
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
- LAG
dev_langs:
- DMX
helpviewer_keywords:
- Lag function
ms.assetid: 2da6df1a-5506-4871-a0f0-83292f1df41e
caps.latest.revision: 31
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 929924bc7ef571a64bc4d7f6c26d20dece081821
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="lag-dmx"></a>Lag (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la tranche horaire entre la date du cas en cours et la dernière date de l'ensemble d'apprentissage.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Lag()  
```  
  
## <a name="return-type"></a>Type de retour  
 Valeur scalaire de type entier.  
  
## <a name="remarks"></a>Notes  
 Si le **Lag** fonction est utilisée sur un modèle où se trouve la colonne KEY TIME au sein d’une table imbriquée, la fonction doit se trouve dans l’expression Sub-SELECT de l’instruction.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne les cas compris dans les données des 12 derniers mois qui ont servi à l'apprentissage du modèle.  
  
```  
SELECT * FROM [Forecasting].CASES  
WHERE Lag() < 12  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Data Mining Extensions &#40;DMX&#41; référence de fonction](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [Fonctions &#40;DMX&#41;](../dmx/functions-dmx.md)   
 [Fonctions de prédiction générales &#40;DMX&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  
