---
title: Descendants (MDX) | Documents Microsoft
ms.date: 05/30/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 047c9e0edc8120003cdd8a38953e33e779ab0994
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/02/2018
ms.locfileid: "34580411"
---
# <a name="descendants-mdx"></a>Descendants (MDX)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Retourne le jeu de descendants d'un membre à un niveau spécifié ou à une distance spécifiée, en incluant ou en excluant éventuellement des descendants dans d'autres niveaux.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Member expression syntax using a level expression  
Descendants(Member_Expression [ , Level_Expression [ ,Desc_Flag ] ] )  
  
Member expression syntax using a numeric expression  
Descendants(Member_Expression [ , Distance [ ,Desc_Flag ] ] )  
  
Set expression syntax using a level expression  
Descendants(Set_Expression [ , Level_Expression [ ,Desc_Flag ] ] )  
  
Member expression syntax using a numeric expression  
Descendants(Set_Expression [ , Distance [ ,Desc_Flag ] ] )  
  
```  
  
## <a name="arguments"></a>Arguments  
 *Argument*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un membre.  
  
 *Set_Expression*  
 Une expression MDX (Multidimensional Expressions) valide qui retourne un jeu.  
  
 *Level_Expression*  
 Expression MDX (Multidimensional Expressions) valide qui retourne un niveau.  
  
 *distance*  
 Expression numérique valide qui spécifie la distance depuis le membre spécifié.  
  
 *Desc_Flag*  
 Expression de chaîne valide qui précise un indicateur de description qui identifie de possibles jeux de descendants.  
  
## <a name="remarks"></a>Notes  
 Si un niveau est spécifié, le **Descendants** fonction retourne un jeu qui contient les descendants du membre spécifié ou les membres du jeu spécifié, à un niveau spécifié, éventuellement modifié par un indicateur spécifié dans *Desc_Flag*.  
  
 Si *Distance* est spécifié, le **Descendants** fonction retourne un jeu qui contient les descendants du membre spécifié ou les membres du jeu spécifié qui sont le nombre de niveaux dans la hiérarchie du membre spécifié, éventuellement modifié par un indicateur spécifié dans *Desc_Flag*. En règle générale, cette fonction est utilisée avec l'argument Distance pour le travail avec des hiérarchies irrégulières. Si la distance précisée est zéro (0), la fonction retourne un jeu composé uniquement du membre ou du jeu spécifié.  
  
 Si une expression d’ensemble est spécifiée, la **Descendants** fonction est résolue individuellement pour chaque membre de l’ensemble, et le jeu est créé à nouveau. En d’autres termes, la syntaxe utilisée pour la **Descendants** fonction est fonctionnellement équivalente à l’expression MDX [générer](../mdx/generate-mdx.md) (fonction).  
  
 Si aucun niveau ou la distance n’est spécifiée, la valeur par défaut pour le niveau utilisé par la fonction est déterminée en appelant le [niveau](../mdx/level-mdx.md) (fonction) (<\<membre >>. Niveau) pour le membre spécifié (si un membre est spécifié) ou en appelant le **niveau** de fonction pour chaque membre de l’ensemble spécifié (si un jeu est spécifié). Si aucune expression de niveau, distance ou aucun indicateur n'est spécifié, la fonction s'exécute comme si la syntaxe suivante était employée :  
  
 `Descendants`  
  
 `(`  
  
 `Member_Expression ,`  
  
 `Member_Expression.Level ,`  
  
 `SELF_BEFORE_AFTER`  
  
 `)`  
  
 Si vous spécifiez un niveau mais pas un indicateur de description, la fonction s'exécute comme si la syntaxe suivante était utilisée :  
  
 `Descendants`  
  
 `(`  
  
 `Member_Expression ,`  
  
 `Level_Expression,`  
  
 `SELF`  
  
 `)`  
  
 En modifiant la valeur de l'indicateur de description, vous pouvez inclure ou exclure des descendants au niveau spécifié ou à la distance spécifiée, les enfants avant ou après la distance ou le niveau spécifié (jusqu'au nœud feuille), ainsi que les enfants de la feuille quel que soit le niveau spécifié ou la distance spécifiée. Le tableau suivant décrit les indicateurs autorisés dans les *Desc_Flag* argument.  
  
|Indicateur|Description|  
|----------|-----------------|  
|SELF|Retourne uniquement les membres descendants du niveau spécifié ou au niveau de la distance spécifiée. La fonction englobe le membre spécifié si le niveau spécifié correspond au niveau du membre spécifié.|  
|AFTER|Retourne les membres descendants de tous les niveaux subordonnés au niveau ou à la distance spécifiée.|  
|BEFORE|Retourne les membres descendants de tous les niveaux insérés entre le membre spécifié et le niveau spécifié ou à la distance spécifiée. Cet indicateur inclut le membre spécifié mais pas les membres issus du niveau ou de la distance spécifiée.|  
|BEFORE_AND_AFTER|Retourne les membres descendants de tous les niveaux subordonnés au niveau du membre spécifié. Cet indicateur inclut le membre spécifié mais pas les membres du niveau spécifié ou au niveau de la distance spécifiée.|  
|SELF_AND_AFTER|Retourne les membres descendants du niveau spécifié ou au niveau de la distance spécifiée, ainsi que tous les niveaux subordonnés au niveau spécifié ou au niveau de la distance spécifiée.|  
|SELF_AND_BEFORE|Retourne les membres descendants du niveau spécifié ou au niveau de la distance spécifiée, ainsi que tous les niveaux entre le membre spécifié et le niveau spécifié ou insérés au niveau de la distance spécifiée, y compris le membre spécifié.|  
|SELF_BEFORE_AFTER|Retourne les membres descendants de tous les niveaux subordonnés au niveau du membre spécifié, y compris le membre spécifié.|  
|LEAVES|Retourne les membres descendants feuille insérés entre le membre spécifié et le niveau spécifié ou au niveau de la distance spécifiée.|  
  
## <a name="examples"></a>Exemples  
 L'exemple ci-après retourne le membre spécifié (United States) et les membres situés entre le membre spécifié (United States) et les membres du niveau avant le niveau spécifié (City). Il retourne le membre spécifié lui-même (United States) et les membres du niveau State-Province (niveau avant le niveau City). Cet exemple inclut des arguments avec commentaires qui vous permettent de tester facilement d'autres arguments pour cette fonction.  
  
```  
SELECT Descendants  
   ([Geography].[Geography].[Country].&[United States]  
      //, [Geography].[Geography].[Country]  
   , [Geography].[Geography].[City]  
      //, [Geography].[Geography].Levels (3)  
      //, SELF   
      //, AFTER  
      , BEFORE  
      // BEFORE_AND_AFTER  
      //, SELF_AND_AFTER  
      //, SELF_AND_BEFORE  
      //,SELF_BEFORE_AFTER  
      //,LEAVES   
   ) ON 0  
FROM [Adventure Works]   
```  
  
 L’exemple suivant retourne la moyenne quotidienne de la `Measures.[Gross Profit Margin]` mesure, calculée sur les jours de chaque mois de l’année fiscale 2003 dans le **Adventure Works** cube. Le **Descendants** fonction retourne un ensemble de jours déterminés à partir du membre actuel de la `[Date].[Fiscal]` hiérarchie.  
  
```  
WITH MEMBER Measures.[Avg Gross Profit Margin] AS Avg  
   (  
      Descendants( [Date].[Fiscal].CurrentMember,   
           [Date].[Fiscal].[Date]  
          ),   
        Measures.[Gross Profit Margin]  
   )  
SELECT  
   Measures.[Avg Gross Profit Margin] ON COLUMNS,  
   [Date].[Fiscal].[Month].Members ON ROWS  
FROM [Adventure Works]  
WHERE ([Date].[Fiscal Year].&[2003])  
```  
  
 L'exemple suivant utilise une expression de niveau et retourne la mesure Internet Sales Amount (volume de vente Internet) pour chaque State-Province (état-province) en Australie ; de même, il retourne le pourcentage de volume de vente Internet total pour l'Australie par état-province. Cet exemple utilise la fonction Item pour extraire le premier (et unique) tuple du jeu qui est retourné par la **ancêtres** (fonction).  
  
```  
WITH MEMBER Measures.x AS   
   [Measures].[Internet Sales Amount] /   
   ( [Measures].[Internet Sales Amount],  
      Ancestors   
         ( [Customer].[Customer Geography].CurrentMember,   
           [Customer].[Customer Geography].[Country]  
         ).Item (0)  
   ), FORMAT_STRING = '0%'  
SELECT {[Measures].[Internet Sales Amount], Measures.x} ON 0,  
{Descendants   
   ( [Customer].[Customer Geography].[Country].&[Australia],   
     [Customer].[Customer Geography].[State-Province], SELF   
   )    
} ON 1  
FROM [Adventure Works]  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des fonctions MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
