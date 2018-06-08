---
title: Mot clé EXISTING (MDX) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 08cdfd62f25ec42195418938da37c502f221120d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="mdx-query---existing-keyword"></a>Requête MDX - mot clé EXISTING
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Force un jeu spécifié à être évalué dans le contexte actuel.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Existing Set_Expression  
```  
  
## <a name="arguments"></a>Arguments  
 *Set_Expression*  
 Expression d'ensemble MDX (Multidimensional Expressions) valide.  
  
## <a name="remarks"></a>Notes  
 Par défaut, les jeux sont évalués dans le contexte du cube qui contient les membres de ce jeu. Le mot clé **Existing** contraint un jeu spécifié à être évalué dans le contexte actuel à la place.  
  
## <a name="example"></a>Exemple  
 L’exemple ci-dessous retourne le nombre de revendeurs dont les ventes ont baissé sur la période précédente en se basant sur les valeurs de membres State-Province (état-province) sélectionnées par l’utilisateur et évaluées à l’aide de la fonction **Aggregate** . Le mot clé [Hierarchize &#40;MDX&#41;](../../../mdx/hierarchize-mdx.md) et [DrilldownLevel (MDX)](../../../mdx/drilldownlevel-mdx.md) sont utilisées pour retourner des valeurs de ventes en baisse concernant les catégories de produits inscrites dans la dimension Product. Le mot clé **Existing** force le jeu dans la fonction **Filter** à être évalué dans le contexte actuel, c’est-à-dire pour les membres Washington et Oregon de la hiérarchie d’attribut State-Province.  
  
```  
WITH MEMBER Measures.[Declining Reseller Sales] AS  
   Count  
      (Filter  
         (Existing  
            (Reseller.Reseller.Reseller)  
         , [Measures].[Reseller Sales Amount] <   
            ([Measures].[Reseller Sales Amount]  
               ,[Date].Calendar.PrevMember  
            )  
        )  
      )  
MEMBER [Geography].[State-Province].x AS   
   Aggregate   
      ( {[Geography].[State-Province].&[WA]&[US]  
         , [Geography].[State-Province].&[OR]&[US] }   
      )  
SELECT NON EMPTY HIERARCHIZE   
      (AddCalculatedMembers   
         (   
            {DrillDownLevel  
               ({[Product].[All Products]}  
               )  
            }   
         )   
      ) DIMENSION PROPERTIES PARENT_UNIQUE_NAME ON COLUMNS   
FROM [Adventure Works]  
WHERE   
      ( [Geography].[State-Province].x  
        , [Date].[Calendar].[Calendar Quarter].&[2003]&[4]  
        ,[Measures].[Declining Reseller Sales]  
      )  
  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Nombre & #40 ; Définir le & #41 ; & #40 ; MDX & #41 ;](../../../mdx/count-set-mdx.md)   
 [AddCalculatedMembers & #40 ; MDX & #41 ;](../../../mdx/addcalculatedmembers-mdx.md)   
 [Agrégat & #40 ; MDX & #41 ;](../../../mdx/aggregate-mdx.md)   
 [Filtre & #40 ; MDX & #41 ;](../../../mdx/filter-mdx.md)   
 [Propriétés & #40 ; MDX & #41 ;](../../../mdx/properties-mdx.md)   
 [DrilldownLevel & #40 ; MDX & #41 ;](../../../mdx/drilldownlevel-mdx.md)   
 [Hierarchize & #40 ; MDX & #41 ;](../../../mdx/hierarchize-mdx.md)   
 [Référence des fonctions MDX & #40 ; MDX & #41 ;](../../../mdx/mdx-function-reference-mdx.md)  
  
  
