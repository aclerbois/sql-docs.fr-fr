---
title: Création d’étendue de Session de cellules calculées | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4ca621d103f294a88fec93dbf6f24d7402279efa
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-cell-calculations---session-scoped-calculated-cells"></a>Calculs de cellules MDX - cellules calculées au niveau de Session
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
    
> [!IMPORTANT]  
>  Cette syntaxe est déconseillée. Il est conseillé d'utiliser plutôt les assignations MDX. Pour plus d’informations sur les assignations, consultez [Script MDX de base &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/the-basic-mdx-script-mdx.md).  
  
 Pour créer des cellules calculées mises à la disposition de l'ensemble des requêtes au cours de la même session, vous pouvez utiliser l'instruction [CREATE CELL CALCULATION](../../../mdx/mdx-data-definition-create-cell-calculation.md) ou l'instruction [ALTER CUBE](../../../mdx/mdx-data-definition-alter-cube.md) . Ces deux instructions génèrent le même résultat.  
  
## <a name="create-cell-calculation-syntax"></a>Syntaxe de l'instruction CREATE CELL CALCULATION  
  
> [!IMPORTANT]  
>  Cette syntaxe est déconseillée. Il est conseillé d'utiliser plutôt les assignations MDX. Pour plus d’informations sur les assignations, consultez [Script MDX de base &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/the-basic-mdx-script-mdx.md).  
  
 Utilisez la syntaxe suivante pour définir une cellule calculée au niveau de la session à l'aide de l'instruction CREATE CELL CALCULATION :  
  
```  
CREATE CELL CALCULATION Cube_Expression.<CREATE CELL CALCULATION body clause>  
  
<CREATE CELL CALCULATION body clause> ::=CellCalc_Identifier FOR String_Expression AS 'MDX_Expression'   
   [ <CREATE CELL CALCULATION property clause> [ , <CREATE CELL CALCULATION property clause> ... ] ]  
  
<CREATE CELL CALCULATION property clause> ::=  
   ( CONDITION = 'Logical_Expression' ) |   
   ( DISABLED = { TRUE | FALSE } ) |   
   ( DESCRIPTION =String_Expression ) |   
   ( CALCULATION_PASS_NUMBER = Integer_Expression ) |   
   ( CALCULATION_PASS_DEPTH = Integer_Expression ) |   
   ( SOLVE_ORDER = Integer_Expression ) |   
   ( FORMAT_STRING = String_Expression ) |   
   ( CellProperty_Identifier = Scalar_Expression )  
```  
  
## <a name="alter-cube-syntax"></a>Syntaxe de l'instruction ALTER CUBE  
  
> [!IMPORTANT]  
>  Cette syntaxe est déconseillée. Il est conseillé d'utiliser plutôt les assignations MDX. Pour plus d’informations sur les assignations, consultez [Script MDX de base &#40;MDX&#41;](../../../analysis-services/multidimensional-models/mdx/the-basic-mdx-script-mdx.md).  
  
 Utilisez la syntaxe suivante pour définir une cellule calculée au niveau de la session à l'aide de l'instruction ALTER CUBE :  
  
```  
ALTER CUBE Cube_Identifier CREATE CELL CALCULATION  
FOR String_Expression AS 'MDX_Expression'   
   [ <CREATE CELL CALCULATION property clause> [ , <CREATE CELL CALCULATION property clause> ... ] ]  
  
<CREATE CELL CALCULATION property clause> ::=  
   ( CONDITION = 'Logical_Expression' ) |   
   ( DISABLED = { TRUE | FALSE } ) |   
   ( DESCRIPTION =String_Expression ) |   
   ( CALCULATION_PASS_NUMBER = Integer_Expression ) |   
   ( CALCULATION_PASS_DEPTH = Integer_Expression ) |   
   ( SOLVE_ORDER = Integer_Expression ) |   
   ( FORMAT_STRING = String_Expression ) |   
   ( CellProperty_Identifier = Scalar_Expression )  
```  
  
 La valeur `String_Expression` contient une liste d'expressions de jeu MDX unidimensionnelles et orthogonales dont chacune doit prendre la valeur de l'une des catégories de jeux répertoriées dans le tableau suivant.  
  
|Catégorie|Description|  
|--------------|-----------------|  
|Jeu vide|Expression de jeu MDX qui prend la valeur d'un ensemble vide. Dans ce cas, la portée de la cellule calculée est l'intégralité du cube.|  
|Jeu à un seul membre|Expression de jeu MDX qui prend la valeur d'un seul membre.|  
|Jeu de membres de niveau|Expression de jeu MDX qui prend la valeur des membres d'un même niveau. La fonction MDX *Level_Expression*.**Members** en est un exemple. Pour inclure des membres calculés, utilisez la fonction MDX *Level_Expression*.**AllMembers** .<br /><br /> Pour plus d’informations, consultez [AllMembers &#40;MDX&#41;](../../../mdx/allmembers-mdx.md).|  
|Jeu de descendants|Expression de jeu MDX qui prend la valeur des descendants d'un membre spécifié. La fonction MDX **Descendants**(*Member_Expression*, *Level_Expresion*, *Desc_Flag*) en est un exemple.<br /><br /> Pour plus d’informations, consultez [Descendants &#40;MDX&#41;](../../../mdx/descendants-mdx.md).|  
  
## <a name="see-also"></a>Voir aussi  
 [Création de calculs de cellules dans MDX & #40 ; MDX & #41 ;](../../../analysis-services/multidimensional-models/mdx/mdx-cell-calculations-build-cell-calculations.md)  
  
  
