---
title: Existe (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 98f07985b83ae32f02bd3da68d382c8f3fceb0cf
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34578251"
---
# <a name="exists-mdx"></a>Exists (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne l'ensemble des tuples du premier jeu spécifié qui coexistent avec un ou plusieurs tuples du deuxième jeu spécifié. Cette fonction effectue manuellement ce que l'auto-existence réalise de manière automatique. Pour plus d’informations sur automatique existe, consultez [Concepts clés dans MDX &#40;Analysis Services&#41;](../analysis-services/multidimensional-models/mdx/key-concepts-in-mdx-analysis-services.md).  
  
 Si le paramètre facultatif \<nom de groupe de mesures > n’est fourni, la fonction retourne les tuples qui coexistent avec un ou plusieurs tuples à partir du deuxième jeu et les tuples qui sont associés à des lignes dans la table de faits du groupe de mesures spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Exists( Set_Expression1 , Set_Expression2 [, MeasureGroupName] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression1*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Set_Expression2*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *MeasureGroupName*  
 Expression de chaîne valide qui précise le nom d'un groupe de mesures.  
  
## <a name="remarks"></a>Notes  
  
1.  Lignes de groupe de mesures avec des mesures contenant des valeurs null contribuent à **Exists** lorsque l’argument MeasureGroupName est spécifié. C'est la différence entre cette forme d'Exists et la fonction Nonempty : si la propriété NullProcessing de ces mesures est configurée sur Conserver, cela signifie que les mesures afficheront des valeurs NULL lorsque les requêtes sont exécutées contre cette partie du cube ; NonEmpty supprimera toujours des tuples d'un jeu qui a des valeurs de la mesure Null, alors qu'Exists avec l'argument MeasureGroupName ne filtrera pas les tuples qui ont des lignes de groupe de mesures associées, même si les valeurs de mesure sont Null.  
  
2.  Si *MeasureGroupName* paramètre est utilisé, s’il existe des mesures visibles dans le groupe de mesures référencé ; s’il en existe aucune mesure visible dans le groupe de mesures référencé puis EXISTS retourne toujours un jeu vide, indépendamment des valeurs de résultats dépend *Set_Expression1* et *Set_Expression2*.  
  
## <a name="examples"></a>Exemples  
 Clients résidant en Californie :  
  
```  
SELECT [Measures].[Internet Sales Amount] ON 0,  
EXISTS(  
[Customer].[Customer].[Customer].MEMBERS  
, {[Customer].[State-Province].&[CA]&[US]}  
) ON 1   
FROM [Adventure Works]  
  
```  
  
 Clients résidant en Californie avec ventes :  
  
```  
SELECT [Measures].[Internet Sales Amount] ON 0,  
EXISTS(  
[Customer].[Customer].[Customer].MEMBERS  
, {[Customer].[State-Province].&[CA]&[US]}  
, "Internet Sales") ON 1   
FROM [Adventure Works]  
  
```  
  
 Clients avec ventes :  
  
```  
SELECT [Measures].[Internet Sales Amount] ON 0,  
EXISTS(  
[Customer].[Customer].[Customer].MEMBERS  
, , "Internet Sales") ON 1   
FROM [Adventure Works]  
  
```  
  
 Clients ayant acheté des vélos :  
  
```  
SELECT [Measures].[Internet Sales Amount] ON 0,  
EXISTS(  
[Customer].[Customer].[Customer].MEMBERS  
, {[Product].[Product Categories].[Category].&[1]}  
, "Internet Sales") ON 1   
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)   
 [Crossjoin &#40;MDX&#41;](../mdx/crossjoin-mdx.md)   
 [NonEmptyCrossjoin &#40;MDX&#41;](../mdx/nonemptycrossjoin-mdx.md)   
 [NonEmpty &#40;MDX&#41;](../mdx/nonempty-mdx.md)   
 [IsEmpty &#40;MDX&#41;](../mdx/isempty-mdx.md)  
  
  
