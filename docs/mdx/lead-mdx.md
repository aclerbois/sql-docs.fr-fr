---
title: Entraîner (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 0e9faf5d0f7b549887c6aa4a12e4c502c2663ce3
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578991"
---
# <a name="lead-mdx"></a>Lead (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne un membre correspondant à un nombre spécifique de positions après un membre spécifié le long du niveau du membre.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Member_Expression.Lead( Index )  
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Index*  
 Expression numérique valide qui spécifie un nombre de positions de membres.  
  
## <a name="remarks"></a>Notes  
 Les positions des membres dans un niveau sont déterminées en fonction de l'ordre naturel de la hiérarchie d'attribut. La numérotation des positions commence à zéro.  
  
 Si le nombre placé en premier est zéro (0), la **entraîner** fonction retourne le membre spécifié.  
  
 Si le nombre placé en premier est négatif, le **entraîner** fonction retourne un membre précédent.  
  
 `Lead(1)` équivaut à la [NextMember](../mdx/nextmember-mdx.md) (fonction). `Lead(-1)` équivaut à la [PrevMember](../mdx/prevmember-mdx.md) (fonction).  
  
 Le **entraîner** fonction est similaire à la [Lag](../mdx/lag-mdx.md) de fonction, à ceci près que le **Lag** fonction recherche dans la direction opposée à la **entraîner** (fonction). Ce qui signifie que `Lead(n)` est équivalent à `Lag(-n)`.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-après retourne la valeur du mois de décembre 2001 :  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lead(-2) ON 0  
FROM [Adventure Works]  
  
```  
  
 L'exemple ci-après retourne la valeur du mois de mars 2002 :  
  
```  
SELECT [Date].[Fiscal].[Month].[February 2002].Lead(1) ON 0  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
