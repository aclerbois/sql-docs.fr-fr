---
title: "MISE À JOUR (DMX) | Documents Microsoft"
ms.custom: 
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.service: 
ms.component: data-mining
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: language-reference
f1_keywords: UPDATE
dev_langs: DMX
helpviewer_keywords:
- NODE_CAPTION column
- mining models [Analysis Services], content changes
- modifying mining model content
- UPDATE statement [SQL Server], DMX
ms.assetid: 8a2b0942-c490-410c-b1cf-ff2e0fd8e24b
caps.latest.revision: "36"
author: Minewiskan
ms.author: owend
manager: erikre
ms.workload: Inactive
ms.openlocfilehash: d8c6c6d2779088fd10a3f4e3f63b52df110a898a
ms.sourcegitcommit: f486d12078a45c87b0fcf52270b904ca7b0c7fc8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/08/2018
---
# <a name="update-dmx"></a>UPDATE (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Modifications du **NODE_CAPTION** colonne dans le modèle d’exploration de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
UPDATE <model>.CONTENT  
SET NODE_CAPTION='new caption'  
[WHERE <condition expression>]  
```  
  
## <a name="arguments"></a>Arguments  
 *model*  
 Identificateur du modèle  
  
 *nouvelle légende*  
 Chaîne qui contient le nouveau nom pour le **NODE_CAPTION** colonne.  
  
 *expression de condition*  
 Facultatif. Condition pour restreindre les valeurs retournées de la liste des colonnes.  
  
## <a name="examples"></a>Exemples  
 Dans l’exemple suivant, la **mise à jour** instruction modifie le nom par défaut, `Cluster 1`, pour le cluster `001` à un nom plus descriptif, `Likely Customers`.  
  
```  
UPDATE [TM Clustering].CONTENT  
SET NODE_CAPTION= 'Likely Customers'  
WHERE NODE_UNIQUE_NAME = '001'  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Les Extensions d’exploration de données &#40; DMX &#41; Instructions de définition de données](../dmx/dmx-statements-data-definition.md)   
 [Les Extensions d’exploration de données &#40; DMX &#41; Instructions de Manipulation de données](../dmx/dmx-statements-data-manipulation.md)   
 [Guide de référence des instructions DMX &#40;Data Mining Extensions&#41;](../dmx/data-mining-extensions-dmx-statements.md)  
  
  
