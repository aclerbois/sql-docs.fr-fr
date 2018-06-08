---
title: Head (MDX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: ''
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- HEAD
dev_langs:
- kbMDX
helpviewer_keywords:
- Head function
ms.assetid: 2a909bda-1366-4537-93b0-c089554fc11f
caps.latest.revision: 35
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: b0e25718c55dbe433bd34438b459aed31b2097f9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="head-mdx"></a>Head (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le premier nombre spécifié d'éléments dans un jeu, en conservant les doublons.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Head(Set_Expression [ ,Count ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Compter*  
 Expression numérique valide qui précise le nombre de tuples à retourner.  
  
## <a name="remarks"></a>Notes  
 Le **Head** fonction retourne le nombre spécifié de tuples à partir du début du jeu spécifié. L'ordre des éléments est conservé. La valeur par défaut de Count est 1. Si le nombre de tuples spécifié est inférieur à 1, le **Head** fonction retourne un jeu vide. Si le nombre de tuples spécifié dépasse le nombre de tuples dans le jeu, la fonction retourne le jeu d'origine.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne les cinq premières sous-catégories de vente de produits, quelle que soit la hiérarchie et conformément à la mesure Reseller Gross Profit (marge brute du revendeur). Le **Head** fonction est utilisée pour retourner uniquement les 5 premiers jeux dans le résultat, une fois que le résultat est trié à l’aide de la **ordre** (fonction).  
  
```  
SELECT   
[Measures].[Reseller Gross Profit] ON 0,  
Head  
   (Order   
      ([Product].[Product Categories].[SubCategory].members  
         ,[Measures].[Reseller Gross Profit]  
         ,BDESC  
      )  
   ,5  
   ) ON 1  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [La fin du &#40;MDX&#41;](../mdx/tail-mdx.md)   
 [Élément &#40;Tuple&#41; &#40;MDX&#41;](../mdx/item-tuple-mdx.md)   
 [Élément &#40;membre&#41; &#40;MDX&#41;](../mdx/item-member-mdx.md)   
 [Rang &#40;MDX&#41;](../mdx/rank-mdx.md)   
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
