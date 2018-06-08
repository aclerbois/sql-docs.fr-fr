---
title: Élément (Tuple) (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 18217c0798155c912baa6afa30b33f5533a0e198
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34579461"
---
# <a name="item-tuple-mdx"></a>Item (Tuple) (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne un tuple d'un jeu.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Index syntax  
Set_Expression.Item(Index)  
  
String expression syntax  
Set_Expression.Item(String_Expression1 [ ,String_Expression2,...n])  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *String_Expression1*  
 Expression de chaîne valide qui correspond généralement à un tuple exprimé dans une chaîne.  
  
 *String_Expression2*  
 Expression de chaîne valide qui correspond généralement à un tuple exprimé dans une chaîne.  
  
 *Index*  
 Expression numérique valide qui précise le tuple spécifique par position dans le jeu à retourner.  
  
## <a name="remarks"></a>Notes  
 Le **élément** fonction retourne un tuple du jeu spécifié. Il existe trois façons d’appeler le **élément** fonction :  
  
-   Si une expression de chaîne unique est spécifiée, le **élément** fonction retourne le tuple spécifié. Par exemple, « ([2005].Q3, [Store05]) ».  
  
-   Si plus d’une expression de chaîne est spécifiée, le **élément** fonction retourne le tuple défini par les coordonnées spécifiées. Le nombre de chaînes doit correspondre au nombre d'axes, et chaque chaîne doit identifier une hiérarchie unique. Par exemple, « [2005].Q3 », « [Store05] ».  
  
-   Si un entier est spécifié, le **élément** fonction retourne le tuple qui se trouve dans la position de base zéro spécifiée par *Index*.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous retourne ([1996],Sales) :  
  
 `{([1996],Sales), ([1997],Sales), ([1998],Sales)}.Item(0)`  
  
 L'exemple suivant utilise une expression de niveau et retourne la mesure Internet Sales Amount (volume de vente Internet) pour chaque State-Province (état-Province) en Australie ; il dévoile également le pourcentage de volume de vente Internet total pour l'Australie. Cet exemple utilise la fonction Item pour extraire le premier (et uniquement le tuple) dans le jeu retourné par le **ancêtres** (fonction).  
  
```  
WITH MEMBER Measures.x AS [Measures].[Internet Sales Amount] /   
   ( [Measures].[Internet Sales Amount],    
      Ancestors   
      ( [Customer].[Customer Geography].CurrentMember,  
        [Customer].[Customer Geography].[Country]  
      ).Item (0)  
   ), FORMAT_STRING = '0%'  
SELECT {[Measures].[Internet Sales Amount], Measures.x} ON 0,  
{ Descendants   
   ( [Customer].[Customer Geography].[Country].&[Australia],  
     [Customer].[Customer Geography].[State-Province], SELF   
   )   
} ON 1  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
