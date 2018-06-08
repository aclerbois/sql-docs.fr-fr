---
title: AVG (MDX) | Documents Microsoft
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
- AVG
dev_langs:
- kbMDX
helpviewer_keywords:
- Avg function [MDX]
ms.assetid: efe61272-c3eb-4a33-b231-e00c30be16aa
caps.latest.revision: 42
author: Minewiskan
ms.author: owend
manager: erikre
ms.openlocfilehash: dd508fcef5ecd378dea8ec4afe86dfd35e45d1a5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="avg-mdx"></a>Avg (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Évalue un jeu et retourne la moyenne des valeurs non vides des cellules dans le jeu par rapport aux mesures du jeu ou à une mesure spécifique.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Avg( Set_Expression [ , Numeric_Expression ] )  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Numeric_expression*  
 Expression numérique valide qui correspond généralement à une expression MDX (Multidimensional Expressions) des coordonnées des cellules qui retournent un nombre.  
  
## <a name="remarks"></a>Notes  
 Si un jeu de tuples vides ou un jeu vide est spécifié, le **Avg** fonction retourne une valeur vide.  
  
 Le **Avg** calcule la moyenne des valeurs non vides des cellules dans le jeu spécifié en calculant la somme des valeurs d’abord toutes les cellules dans le jeu spécifié, puis en divisant cette somme par le nombre de cellules non vides dans le jeu spécifié.  
  
> [!NOTE]  
>  [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] ignore les valeurs NULL lors du calcul de la valeur moyenne dans un jeu de nombres.  
  
 Si une expression numérique spécifique (en général une mesure) n’est pas spécifiée, le **Avg** fonction calcule la moyenne de chaque mesure dans le contexte de requête actuel. Si une mesure spécifique est fournie, le **Avg** fonction évalue d’abord la mesure dans le jeu, puis calcule la moyenne en fonction de la mesure spécifiée.  
  
> [!NOTE]  
>  Lorsque vous utilisez la **CurrentMember** fonction dans une instruction de membre calculé, vous devez spécifier une expression numérique, car aucune mesure par défaut n’existe pour la coordonnée actuelle dans ce contexte de requête.  
  
 Pour forcer l’inclusion de cellules vides, l’application doit utiliser le [CoalesceEmpty](../mdx/coalesceempty-mdx.md) de fonction, ou spécifiez une valide *Numeric_Expression* qui fournit une valeur de zéro (0) pour les valeurs vides. Pour plus d'informations sur les cellules vides, consultez la documentation OLE DB.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant retourne la moyenne pour une mesure sur un jeu spécifié. Remarquez que la mesure spécifiée peut être soit la mesure par défaut pour les membres du jeu spécifié ou une mesure spécifiée.  
  
 `WITH SET [NW Region] AS`  
  
 `{[Geography].[State-Province].[Washington]`  
  
 `, [Geography].[State-Province].[Oregon]`  
  
 `, [Geography].[State-Province].[Idaho]}`  
  
 `MEMBER [Geography].[Geography].[NW Region Avg] AS`  
  
 `AVG ([NW Region]`  
  
 `--Uncomment the line below to get an average by Reseller Gross Profit Margin`  
  
 `--otherwise the average will be by whatever the default measure is in the cube,`  
  
 `--or whatever measure is specified in the query`  
  
 `--, [Measures].[Reseller Gross Profit Margin]`  
  
 `)`  
  
 `SELECT [Date].[Calendar Year].[Calendar Year].Members ON 0`  
  
 `FROM [Adventure Works]`  
  
 `WHERE ([Geography].[Geography].[NW Region Avg])`  
  
 L’exemple suivant retourne la moyenne quotidienne de la `Measures.[Gross Profit Margin]` mesure, calculée sur les jours de chaque mois de l’année fiscale 2003 dans le **Adventure Works** cube. Le **Avg** calcule la moyenne de l’ensemble de jours qui sont contenus dans chaque mois de la `[Ship Date].[Fiscal Time]` hiérarchie. La première version du calcul montre le comportement par défaut d'Avg en excluant de la moyenne les jours qui n'ont pas enregistré de ventes, la deuxième version montre comment inclure les jours sans ventes dans la moyenne.  
  
 `WITH MEMBER Measures.[Avg Gross Profit Margin] AS`  
  
 `Avg(`  
  
 `Descendants(`  
  
 `[Ship Date].[Fiscal].CurrentMember,`  
  
 `[Ship Date].[Fiscal].[Date]`  
  
 `),`  
  
 `Measures.[Gross Profit Margin]`  
  
 `), format_String='percent'`  
  
 `MEMBER Measures.[Avg Gross Profit Margin Including Empty Days] AS`  
  
 `Avg(`  
  
 `Descendants(`  
  
 `[Ship Date].[Fiscal].CurrentMember,`  
  
 `[Ship Date].[Fiscal].[Date]`  
  
 `),`  
  
 `CoalesceEmpty(Measures.[Gross Profit Margin],0)`  
  
 `), Format_String='percent'`  
  
 `SELECT`  
  
 `{Measures.[Avg Gross Profit Margin],Measures.[Avg Gross Profit Margin Including Empty Days]} ON COLUMNS,`  
  
 `[Ship Date].[Fiscal].[Fiscal Year].Members ON ROWS`  
  
 `FROM`  
  
 `[Adventure Works]`  
  
 `WHERE([Product].[Product Categories].[Product].&[344])`  
  
 L’exemple suivant retourne la moyenne quotidienne de la `Measures.[Gross Profit Margin]` mesure, calculée sur les jours de chaque semestre de l’année fiscale 2003 dans le **Adventure Works** cube.  
  
```  
WITH MEMBER Measures.[Avg Gross Profit Margin] AS  
   Avg(  
      Descendants(  
         [Ship Date].[Fiscal].CurrentMember,   
            [Ship Date].[Fiscal].[Date]  
      ),   
      Measures.[Gross Profit Margin]  
   )  
SELECT  
   Measures.[Avg Gross Profit Margin] ON COLUMNS,  
      [Ship Date].[Fiscal].[Fiscal Year].[FY 2003].Children ON ROWS  
FROM  
   [Adventure Works]  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../mdx/mdx-function-reference-mdx.md)  
  
  
