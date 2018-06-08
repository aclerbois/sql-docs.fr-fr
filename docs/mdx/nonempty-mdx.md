---
title: NonEmpty (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4a718a7e75a9b31b7d35cdc33883c97739aee68c
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580281"
---
# <a name="nonempty-mdx"></a>NonEmpty (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne l'ensemble des tuples qui ne sont pas vides d'un jeu spécifié sur la base du produit croisé du jeu spécifié avec un deuxième jeu.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
NONEMPTY(set_expression1 [,set_expression2])  
```  
  
## <a name="arguments"></a>Arguments  
 *set_expression1*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *set_expression2*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
## <a name="remarks"></a>Notes  
 Cette fonction retourne dans le premier jeu spécifié les tuples qui ne sont pas vides lorsqu'ils sont évalués dans les tuples du deuxième jeu. Le **NonEmpty** fonction tient compte des calculs et conserve les tuples dupliqués. Si aucun deuxième jeu n'est fourni, l'expression est évaluée dans le contexte des coordonnées actuelles des membres des hiérarchies d'attribut et des mesures du cube.  
  
> [!NOTE]  
>  Utilisez cette fonction plutôt que déconseillées [NonEmptyCrossjoin &#40;MDX&#41; ](../mdx/nonemptycrossjoin-mdx.md) (fonction).  
  
> [!IMPORTANT]  
>  La valeur non vide est une caractéristique des cellules référencées par les tuples, et non des tuples eux-mêmes.  
  
## <a name="examples"></a>Exemples  
 La requête suivante montre un exemple simple de **NonEmpty**, renvoyer tous les clients qui avaient une valeur non null pour le montant des ventes sur Internet sur le 1er juillet 2001 :  
  
 `SELECT [Measures].[Internet Sales Amount] ON 0,`  
  
 `NONEMPTY(`  
  
 `[Customer].[Customer].[Customer].MEMBERS`  
  
 `, {([Date].[Calendar].[Date].&[20010701], [Measures].[Internet Sales Amount])}`  
  
 `)`  
  
 `ON 1`  
  
 `FROM [Adventure Works]`  
  
 L’exemple suivant retourne le jeu de tuples contenant les clients et les dates d’achat, à l’aide de la **filtre** (fonction) et le **NonEmpty** fonctions pour trouver la dernière date à laquelle chaque client a effectué un achat :  
  
 `WITH SET MYROWS AS FILTER`  
  
 `(NONEMPTY`  
  
 `([Customer].[Customer Geography].[Customer].MEMBERS`  
  
 `* [Date].[Date].[Date].MEMBERS`  
  
 `, [Measures].[Internet Sales Amount]`  
  
 `) AS MYSET`  
  
 `, NOT(MYSET.CURRENT.ITEM(0)`  
  
 `IS MYSET.ITEM(RANK(MYSET.CURRENT, MYSET)).ITEM(0))`  
  
 `)`  
  
 `SELECT [Measures].[Internet Sales Amount] ON 0,`  
  
 `MYROWS ON 1`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [DefaultMember &#40;MDX&#41;](../mdx/defaultmember-mdx.md)   
 [Filtre &#40;MDX&#41;](../mdx/filter-mdx.md)   
 [IsEmpty &#40;MDX&#41;](../mdx/isempty-mdx.md)   
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)   
 [NonEmptyCrossjoin &#40;MDX&#41;](../mdx/nonemptycrossjoin-mdx.md)  
  
  
