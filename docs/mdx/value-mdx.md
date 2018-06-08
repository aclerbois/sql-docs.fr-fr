---
title: Valeur (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f1d30c54661602fc1f21d37c92480202533928c2
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34582091"
---
# <a name="value-mdx"></a>Value (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la valeur du membre actuel de la dimension de mesures située à l'intersection du membre actuel des hiérarchies d'attribut dans le contexte de la requête.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Member_Expression[.Value]   
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
## <a name="remarks"></a>Notes  
 Le **valeur** fonction retourne la valeur du membre spécifié sous forme de chaîne. Le **valeur** argument est facultatif, car la valeur d’un membre est la propriété par défaut d’un membre et est la valeur retournée pour un membre si aucune autre valeur n’est spécifiée. Pour plus d’informations sur les propriétés des membres, consultez [des propriétés de membre intrinsèques &#40;MDX&#41; ](../analysis-services/multidimensional-models/mdx/mdx-member-properties-intrinsic-member-properties.md) et [propriétés de membre définies par l’utilisateur &#40;MDX&#41;](../analysis-services/multidimensional-models/mdx/mdx-member-properties-user-defined-member-properties.md).  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous retourne la valeur d'un membre ainsi que le nom du membre de manière explicite.  
  
```  
WITH MEMBER [Date].[Calendar].NumericValue as [Date].[Calendar].[July 1, 2001].Value  
MEMBER [Date].[Calendar].MemberName AS [Date].[Calendar].[July 1, 2001].Name  
  
SELECT {[Date].[Calendar].NumericValue, [Date].[Calendar].MemberName} ON 0  
from [Adventure Works]  
```  
  
 L'exemple ci-dessous retourne la valeur d'un membre en tant que valeur par défaut retournée pour un membre sur un axe.  
  
```  
SELECT {[Date].[Calendar].[July 1, 2001]} ON 0  
from [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [MemberValue &#40;MDX&#41;](../mdx/membervalue-mdx.md)   
 [Propriétés &#40;MDX&#41;](../mdx/properties-mdx.md)   
 [Nom &#40;MDX&#41;](../mdx/name-mdx.md)   
 [UniqueName &#40;MDX&#41;](../mdx/uniquename-mdx.md)   
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
