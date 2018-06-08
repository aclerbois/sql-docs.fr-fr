---
title: Lag (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c5e019312eacc2cf3f842721054ad5e39ea75d4b
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578861"
---
# <a name="lag-mdx"></a>Lag (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le membre qui est un nombre spécifié de positions avant un membre spécifié au niveau du membre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Member_Expression.Lag(Index)   
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Index*  
 Expression numérique valide qui spécifie le nombre de positions de membres à décaler.  
  
## <a name="remarks"></a>Notes  
 Les positions des membres dans un niveau sont déterminées en fonction de l'ordre naturel de la hiérarchie d'attribut. La numérotation des positions commence à zéro.  
  
 Si le décalage spécifié est égal à zéro, le **Lag** fonction retourne le membre spécifié lui-même.  
  
 Si le décalage spécifié est négatif, le **Lag** fonction retourne un membre suivant.  
  
 `Lag(1)` équivaut à la [PrevMember](../mdx/prevmember-mdx.md) (fonction). `Lag(-1)` équivaut à la [NextMember](../mdx/nextmember-mdx.md) (fonction).  
  
 Le **Lag** fonction est similaire à la [entraîner](../mdx/lead-mdx.md) de fonction, à ceci près que le **entraîner** fonction recherche dans la direction opposée à la **Lag** (fonction). Ce qui signifie que `Lag(n)` est équivalent à `Lead(-n)`.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-après retourne la valeur du mois de décembre 2001 :  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lag(2) ON 0  
FROM [Adventure Works]  
  
```  
  
 L'exemple ci-après retourne la valeur du mois de mars 2002 :  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lag(-1) ON 0  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
