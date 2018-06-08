---
title: Existe (DMX) | Documents Microsoft
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
- Exists
dev_langs:
- DMX
helpviewer_keywords:
- Exists function
ms.assetid: 3b54dd93-f0a8-4f9a-96ae-a38bf977dda1
caps.latest.revision: 14
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: c42102653cb2da9ec85e4714bba3bedb7a99cd52
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="exists-dmx"></a>Exists (DMX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne **true** si la sous-requête spécifiée retourne au moins une ligne.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
EXISTS(<subquery>)  
```  
  
## <a name="arguments"></a>Arguments  
 *subquery*  
 Une instruction SELECT sous la forme SELECT * FROM \<nom de colonne > [où \<liste_prédicats >].  
  
## <a name="result-type"></a>Type de résultat  
 Retourne **true** si le jeu de résultats retourné par la sous-requête contient au moins une ligne ; sinon, retourne **false**.  
  
## <a name="remarks"></a>Notes  
 Vous pouvez utiliser le mot clé NOT avant EXISTS ; par exemple, `WHERE NOT EXISTS (<subquery>)`.  
  
 La liste des colonnes que vous ajoutez à l'argument de sous-requête d'EXISTS est inappropriée ; la fonction vérifie uniquement l'existence d'une ligne qui remplit la condition.  
  
## <a name="examples"></a>Exemples  
 Vous pouvez utiliser EXISTS et NOT EXISTS pour vérifier des conditions dans une table imbriquée. Cela est utile lors de la création d'un filtre qui contrôle les données utilisées pour l'apprentissage ou le test d'un modèle d'exploration de données. Pour plus d’informations, consultez [Filtres pour les modèles d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../analysis-services/data-mining/filters-for-mining-models-analysis-services-data-mining.md).  
  
 L’exemple suivant est basé sur le `[Association]` structure et un modèle d’exploration de données que vous avez créé dans le [Basic Data Mining Tutorial](http://msdn.microsoft.com/library/6602edb6-d160-43fb-83c8-9df5dddfeb9c). La requête retourne uniquement les cas où le client a acheté au moins un produit Patch Kit.  
  
```  
SELECT * FROM [Association].CASES  
WHERE EXISTS  
(  
SELECT * FROM [v Assoc Seq Line Numbers]  
WHERE [[Model] = 'Patch kit'  
)  
```  
  
 Un autre pour afficher les mêmes données que celui qui sont retournées par cette requête consiste à ouvrir le modèle dans la visionneuse d’associations, cliquez sur le jeu d’éléments **Patch kit = Existing**, sélectionnez le **extraire** option et sélectionnez **cas de modèles uniquement**.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions &#40;DMX&#41;](../dmx/functions-dmx.md)   
 [Syntaxe de filtre et des exemples de modèle &#40;Analysis Services - Exploration de données&#41;](../analysis-services/data-mining/model-filter-syntax-and-examples-analysis-services-data-mining.md)  
  
  
