---
title: SUM (MDX) | Documents Microsoft
ms.custom: ''
ms.date: 03/02/2016
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.component: ''
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SUM
dev_langs:
- kbMDX
helpviewer_keywords:
- Sum function [MDX]
ms.assetid: 6c3db1e3-2c02-49f2-a0bf-cab0fb78c622
caps.latest.revision: 31
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: 21b9c43749abc81180edc009f908a8b4b4f2febd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sum-mdx"></a>Sum (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne la somme d'une expression numérique évaluée sur un jeu spécifié.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Sum( Set_Expression [ , Numeric_Expression ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Expression d'ensemble MDX (Multidimensional Expressions) valide.  
  
 *Numeric_expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
## <a name="remarks"></a>Notes  
 Si une expression numérique est spécifiée, l'expression numérique spécifiée est évaluée sur le jeu, puis totalisée. Si aucune expression numérique n'est précisée, le jeu spécifié est évalué dans le contexte actuel des membres du jeu avant d'être totalisé. Si la fonction SUM est appliquée à une expression non numérique, le résultat n'est pas défini.  
  
> [!NOTE]  
>  Analysis Services ignore les valeurs NULL lors du calcul de la somme d'un jeu de nombres.  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-dessous retourne la somme de la mesure Reseller Sales Amount (volume de vente du revendeur) de tous les membres de la hiérarchie d'attribut Product.Category pour les années civiles 2001 et 2002.  
  
```  
WITH MEMBER Measures.x AS SUM  
   ( { [Date].[Calendar Year].&[2001]  
         , [Date].[Calendar Year].&[2002] }  
      , [Measures].[Reseller Sales Amount]  
    )  
SELECT Measures.x ON 0  
,[Product].[Category].Members ON 1  
FROM [Adventure Works]  
```  
  
 L'exemple ci-dessous retourne la somme des coûts de fret concernant les ventes Internet du mois de juillet 2002 jusqu'à la date du 20 juillet.  
  
```  
WITH MEMBER Measures.x AS SUM   
   (  
      MTD([Date].[Calendar].[Date].[July 20, 2002])  
     , [Measures].[Internet Freight Cost]  
     )  
SELECT Measures.x ON 0  
FROM [Adventure Works]  
```  
  
 L’exemple suivant utilise le mot clé WITH MEMBER et **somme** afin de définir un membre calculé dans la dimension de mesures qui contient la somme de la mesure Reseller Sales Amount pour les membres Canada et United States de la hiérarchie d’attribut pays dans la dimension Geography.  
  
```  
WITH MEMBER Measures.NorthAmerica AS SUM   
      (  
         {[Geography].[Country].&[Canada]  
            , [Geography].[Country].&[United States]}  
       ,[Measures].[Reseller Sales Amount]  
      )  
SELECT {[Measures].[NorthAmerica]} ON 0,  
[Product].[Category].members ON 1  
FROM [Adventure Works]  
```  
  
 Souvent, les **somme** fonction est utilisée avec la **CURRENTMEMBER** ou plusieurs fonctions comme **YTD** qui retourne un jeu qui varie selon le membre actuel d’une hiérarchie. Par exemple, la requête suivante retourne la somme de la mesure de Montant des ventes sur Internet pour toutes les dates du début de l'année civile à la date affichée sur l'axe des lignes :  
  
 `WITH MEMBER MEASURES.YTDSUM AS`  
  
 `SUM(YTD(), [Measures].[Internet Sales Amount])`  
  
 `SELECT {[Measures].[Internet Sales Amount], MEASURES.YTDSUM} ON 0,`  
  
 `[Date].[Calendar].MEMBERS ON 1`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
