---
title: Instruction CASE (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c57276a75e8dd25f13a51c7bd0ea6c75a119a861
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34577251"
---
# <a name="case-statement-mdx"></a>Instruction CASE (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Permet de retourner des valeurs spécifiques à partir de plusieurs comparaisons sous certaines conditions. Il existe deux types d'instructions CASE :  
  
-   Une instruction CASE simple qui compare une expression à un jeu d'expressions simples pour retourner des valeurs spécifiques.  
  
-   Une instruction CASE recherchée qui évalue un jeu d'expressions booléennes pour retourner des valeurs spécifiques.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Simple Case Statement  
CASE [input_expression]  
WHEN when_expression THEN when_true_result_expression  
[...n]  
[ELSE else_result_expression]  
END  
  
Search Case Statement  
CASE   
WHEN Boolean_expression THEN when_true_result_expression  
[...n]  
[ELSE else_result_expression]  
END  
```  
  
## <a name="arguments"></a>Arguments  
 *input_expression*  
 Expression MDX (Multidimensional Expressions) résolue en valeur scalaire.  
  
 *Il peut s’agir*  
 A spécifié une valeur scalaire par rapport à laquelle le *il peut s’agir* est évaluée, lorsqu’elle prend la valeur est true, retourne la valeur scalaire de la *else_result_expression*.  
  
 *when_true_result_expression*  
 Valeur scalaire retournée lorsque la clause WHEN prend la valeur True.  
  
 *else_result_expression*  
 Valeur scalaire retournée si aucune des clauses WHEN ne prend la valeur True.  
  
 *Boolean_expression*  
 Expression MDX qui prend une valeur scalaire.  
  
## <a name="remarks"></a>Notes  
 S'il n'existe aucune clause ELSE et si toutes les clauses WHEN prennent la valeur false, le résultat est une cellule vide.  
  
## <a name="simple-case-expression"></a>Expression CASE simple  
 La syntaxe MDX évalue une expression case simple en résolvant le *il peut s’agir* à une valeur scalaire. Cette valeur scalaire est ensuite comparée à la valeur scalaire de la *il peut s’agir*. Si les deux valeurs scalaires correspondent, l’instruction CASE retourne la valeur de la *when_true_expression*. Si aucune correspondance n'est relevée, la clause WHEN suivante est évaluée. Si toutes les clauses WHEN prennent la valeur false, la valeur de *else_result_expression* à partir de la clause ELSE, le cas échéant, est retourné.  
  
 Dans l'exemple ci-dessous, la mesure Reseller Order Count (nombre de commandes du revendeur) est évaluée par rapport à plusieurs clauses WHEN et retourne un résultat fondé sur la valeur de la mesure Reseller Order Count pour chaque année. Pour les valeurs de mesure Reseller Order Count qui ne correspondent pas à une valeur scalaire spécifiée dans un *il peut s’agir* dans la clause WHEN, la valeur scalaire de la *else_result_expression* est retourné.  
  
```  
WITH MEMBER [Measures].x AS   
CASE [Measures].[Reseller Order Count]  
   WHEN 0 THEN 'NONE'  
   WHEN 1 THEN 'SMALL'  
   WHEN 2 THEN 'SMALL'  
   WHEN 3 THEN 'MEDIUM'  
   WHEN 4 THEN 'MEDIUM'  
   WHEN 5 THEN 'LARGE'  
   WHEN 6 THEN 'LARGE'  
      ELSE 'VERY LARGE'  
END  
SELECT Calendar.[Calendar Year] on 0  
, NON EMPTY [Geography].[Postal Code].Members ON 1  
FROM [Adventure Works]  
WHERE [Measures].x  
```  
  
## <a name="searched-case-expression"></a>Expression CASE recherchée  
 Pour procéder à des évaluations plus complexes à l'aide de l'expression CASE, optez pour l'expression CASE recherchée. Cette variante de l'expression de recherche vous permet d'évaluer si une expression d'entrée s'inscrit dans une plage de valeurs. MDX évalue les clauses WHEN dans l'ordre de leur apparition dans l'instruction CASE.  
  
 Dans l’exemple suivant, la mesure Reseller Order Count est évaluée par rapport à l’élément spécifié *Boolean_expression* pour chacune des nombreuses clauses WHEN. Un résultat basé sur la valeur de la mesure Reseller Order Count pour chaque année est retourné. Du fait que les clauses WHEN sont évaluées dans l'ordre dans lequel elles apparaissent, toutes les valeurs supérieures à 6 peuvent aisément être affectées à la valeur « VERY LARGE » sans avoir à spécifier explicitement chaque valeur. Pour les valeurs de mesure Reseller Order Count qui ne sont pas spécifiés dans la clause WHEN, la valeur scalaire de la *else_result_expression* est retourné.  
  
```  
WITH MEMBER [Measures].x AS   
CASE   
   WHEN [Measures].[Reseller Order Count] > 6 THEN 'VERY LARGE'  
   WHEN [Measures].[Reseller Order Count] > 4 THEN 'LARGE'  
   WHEN [Measures].[Reseller Order Count] > 2 THEN 'MEDIUM'  
   WHEN [Measures].[Reseller Order Count] > 0 THEN 'SMALL'  
   ELSE "NONE"  
END  
SELECT Calendar.[Calendar Year] on 0,  
NON EMPTY [Geography].[Postal Code].Members on 1  
FROM [Adventure Works]  
WHERE [Measures].x  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Instructions de script MDX &#40;MDX&#41;](../mdx/mdx-scripting-statements-mdx.md)  
  
  
