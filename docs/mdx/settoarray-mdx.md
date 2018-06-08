---
title: SetToArray (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 2239249c7372c7132861fbcb74dbe5079a4ae4eb
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34581001"
---
# <a name="settoarray-mdx"></a>SetToArray (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Convertit un ou plusieurs jeux en tableau utilisé par une fonction définie par l'utilisateur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SetToArray(Set_Expression1 [ ,Set_Expression2,...n ][ ,Numeric_Expression ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression1*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Set_Expression2*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Numeric_expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
## <a name="remarks"></a>Notes  
 Le **SetToArray** fonction convertit un ou plusieurs jeux en tableau à utiliser dans une fonction définie par l’utilisateur. Le nombre de dimensions de ce tableau est identique au nombre de jeux précisé.  
  
 L'expression numérique facultative peut fournir les valeurs des cellules du tableau. Si aucune expression numérique n'est spécifiée, la jointure croisée des jeux est évaluée dans le contexte actuel.  
  
 Les coordonnées des cellules du tableau obtenu correspondent à la position des jeux dans la liste. Soit par exemple trois jeux, `SA`, `SB` et `SC`. Chacun de ces jeux a deux éléments. L'instruction MDX, `SetToArray(SA, SB, SC)`, crée le tableau suivant en trois dimensions :  
  
```  
(SA1, SB1, SC1) (SA2, SB1, SC1) (SA1, SB2, SC1) (SA2, SB2, SC1)   
(SA1, SB1, SC2) (SA2, SB1, SC2) (SA1, SB2, SC2) (SA2, SB2, SC2)   
```  
  
> [!NOTE]  
>  Le type de retour de la **SetToArray** fonction est le type VARIANT, VT_ARRAY. Par conséquent, la sortie de la **SetToArray** fonction doit être utilisée uniquement comme entrée à une fonction définie par l’utilisateur.  
  
## <a name="example"></a>Exemple  
 L'exemple ci-dessous retourne un tableau.  
  
```  
SetToArray([Geography].[Geography].Members, [Measures].[Internet Sales Amount])  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
