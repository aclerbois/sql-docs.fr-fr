---
title: Opérateurs unaires | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8d90a8380c29fd6d77126cd6ca8a0ec93634db39
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581401"
---
# <a name="unary-operators"></a>Opérateurs unaires
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Dans la syntaxe MDX (Multidimensional Expressions), les opérateurs unaires exécutent une opération sur un opérande unique, telle que le retour de la valeur négative ou positive d'une expression numérique.  
  
 MDX prend en charge les opérateurs unaires répertoriés dans le tableau suivant.  
  
|Opérateur|Description|  
|--------------|-----------------|  
|[- (Négatif)](../mdx/negative-mdx.md)|Retourne la valeur négative d'une expression numérique.|  
|[+ (Positif)](../mdx/positive-mdx.md)|Retourne la valeur positive d'une expression numérique.|  
  
 L'exemple suivant illustre l'utilisation d'un opérateur unaire pour retourner la valeur négative d'une mesure :  
  
```  
WITH   
   MEMBER [Measures].[NegDiscountAmount] AS  
   -[Measures].[Discount Amount]  
SELECT   
   {[Measures].[Discount Amount],[Measures].[NegDiscountAmount]} on COLUMNS,  
   NON EMPTY [Product].[Product].MEMBERS  ON Rows  
FROM [Adventure Works]  
WHERE [Product].[Category].[Bikes]  
```  
  
 En outre, MDX utilise des opérateurs unaires spéciaux pour déterminer l’opération d’agrégation exécutée par le [RollupChildren](../mdx/rollupchildren-mdx.md) (fonction). Pour plus d’informations sur ces opérateurs unaires particuliers, consultez [ajouter une agrégation personnalisée à une Dimension](../analysis-services/multidimensional-models/bi-wizard-add-a-custom-aggregation-to-a-dimension.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Opérateurs &#40;syntaxe MDX&#41;](../mdx/operators-mdx-syntax.md)  
  
  
